
+++
date = "2013-06-23 17:20:35+00:00"
draft = false
tags = ["cp", "go-lang", "python", "gevent", "lua", "rsync"]
title = "cp command implementation and benchmark in python, go, lua"
url = "/post/53685731325/cp-command-implementation-and-benchmark-in-python"
+++
I was wondering how much will be the speed difference between `` cp `` command, `` rsync `` and implementation in `` python ``, `` go ``, `` lua `` and so wrote this <a href="https://github.com/kracekumar/cp-tests" target="_blank">code</a>.

_Background_

1.   `` python `` has two versions one with `` gevent `` and without `` gevent ``. Both the version uses `` shutil `` for copying files and directory tree.
2.   `` go `` uses <a href="https://github.com/opesun/copyrecur" target="_blank">https://github.com/opesun/copyrecur</a> for copying recursively.
3.   `` lua `` uses `` lfs - LuaFileSystem `` module. `` lfs `` has support for creating directory but not for files, in order to copy the files low level file opening and writing to file technique is used.
4.   `` rsync --progress -ah -R `` was also added to the test.

_Code_

Directory chosen for the test has `` 28 `` repos which are basically python projects, go repos with git version control. Total size of the directory `` /Users/kracekumarramaraju/code `` is `` 300M ``(du -sh /Users/kracekumarramaraju/code) and destination is external disk which supports `` USB3.0 ``.

`` Python without gevent ``

    import sys
    import shutil
    
    
    def cp(source, dest):    
        shutil.copytree(source, dest)
    
    
    if __name__ == "__main__":
        if len(sys.argv) != 3:
            print("Help")
            print("python cp.py source dest is the format")
            sys.exit(1)
        cp(sys.argv[1], sys.argv[2])

`` Python with gevent support ``

    import sys
    import os
    import shutil
    import gevent
    
    
    def cp(source, dest):
        shutil.copytree(source, dest)
    
    
    def cpfile(source, dest):
        shutil.copy2(source, dest)
    
    
    if __name__ == "__main__":
        if len(sys.argv) != 3:
            print("Help")
            print("python cp.py source dest is the format")
            os.exit(1)
        source, dest = sys.argv[1], sys.argv[2]
        os.mkdir(dest)
        tasks = []
        for name in os.listdir(source):
            source_path, dest_path = os.path.join(source, name), os.path.join(dest, name)
            if os.path.isdir(source_path):
                tasks.append(gevent.spawn(cp, source_path, dest_path))
            else:
                tasks.append(gevent.spawn(cpfile, source_path, dest_path))
        gevent.joinall(tasks)

`` go ``

    package main
    
    import (
        "fmt"
        "github.com/opesun/copyrecur"
        "log"
        "os"
    )
    
    func cp(source, dest string) {
        err := copyrecur.CopyDir(source, dest)
        if err != nil {
            log.Fatal(err)
        } else {
            log.Print("Files copied.")
        }
    }
    
    func main() {
        if len(os.Args) != 3 {
            fmt.Println("Syntax: go run cp.go source destination")
            os.Exit(1)
        }
        cp(os.Args[1], os.Args[2])
        fmt.Println("cp.go completed")
    }

`` lua ``

    require "lfs"
    
    
    function cp(source, dest)
        -- body
        for filename in lfs.dir(source) do
            if filename ~= '.' and filename ~= '..' then
                local source_path = source .. '/' .. filename
                local attr = lfs.attributes(source_path)
                --print(attr.mode, path)
                if type(attr) == "table" and attr.mode == "directory" then 
                    local dest_path = dest .. "/" .. filename
                    lfs.mkdir(dest_path)
                    cp(source_path, dest_path)
                else
                    local f = io.open(source_path, "rb")
                    local content = f:read("*all")
                    f:close()
                    local w = io.open(dest .. "/" .. filename, "wb")
                    w:write(content)
                    w:close() 
                end
            end
        end
    end
    
    if #arg == 2 then
        cp(arg[1], arg[2])
    else
        print("Syntax:")
        print("lua lua.go source dest")
    end

`` rsync --progress -ah -R ``.  
 Plain `` cp `` command.

_Tests_

Shell <a href="https://github.com/kracekumar/cp-tests/blob/master/test.sh" target="_blank">script</a> to run the tests.

    echo "source directory size"
    du -sh /Users/kracekumarramaraju/code
    echo "cp.py - Python without gevent"
    time python cp.py /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/1
    du -sh /Volumes/My\ Passport/test/1
    echo "cp-gevent.py - Python with gevent"
    time python cp-gevent.py /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/2
    du -sh /Volumes/My\ Passport/test/2
    echo "alias cp='rsync --progress -ah' - Rsync"
    time cp -R /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/3
    du -sh /Volumes/My\ Passport/test/3
    echo "Plain cp command"
    time /bin/cp -R /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/4
    du -sh /Volumes/My\ Passport/test/4
    echo "cp.go - cp in Go lang"
    time go run cp.go /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/5
    du -sh /Volumes/My\ Passport/test/5
    echo "cp.lua - cp in lua"
    time lua cp.lua /Users/kracekumarramaraju/code /Volumes/My\ Passport/test/6
    du -sh /Volumes/My\ Passport/test/6

_Results_

    ➜  cp-tests  ./test.sh
    source directory size
    300M    /Users/kracekumarramaraju/code
    cp.py - Python without gevent
    
    real    1m23.354s
    user    0m1.818s
    sys 0m5.032s
    302M    /Volumes/My Passport/test/1
    cp-gevent.py - Python with gevent
    
    real    1m24.212s
    user    0m1.772s
    sys 0m4.748s
    302M    /Volumes/My Passport/test/2
    alias cp='rsync --progress -ah' - Rsync
    
    real    1m21.145s
    user    0m0.230s
    sys 0m5.172s
    302M    /Volumes/My Passport/test/3
    Plain cp command
    
    real    1m24.065s
    user    0m0.232s
    sys 0m5.174s
    302M    /Volumes/My Passport/test/4
    cp.go - cp in Go lang
    2013/06/23 21:04:38 Files copied.
    cp.go completed
    
    real    1m27.786s
    user    0m1.106s
    sys 0m3.369s
    302M    /Volumes/My Passport/test/5
    cp.lua - cp in lua
    
    real    1m19.340s
    user    0m1.905s
    sys 0m3.893s
    302M    /Volumes/My Passport/test/6

_Conclusion_

1.   Surprisingly `` lua `` was fastest with `` 1m19.340s `` and next one was `` rsync `` with `` 1m21.145s ``.
2.   Slowest one was `` go `` with `` 1m27.786s ``, I expected it to be faster than `` python gevent ``. Probably extra time was due to compiling go code.
3.   `` Python `` non `` gevent `` version took `` 1m23.354s `` and `` gevent `` version took `` 1m24.212s ``, `` gevent `` version spent less time in `` user `` and `` system `` space.
4.   `` cp `` command took `` 1m24.065s ``, which was second slowest.
5.   Since the test was basically I/O there ins’t much difference in speed of all versions.

_Further work_

1.   Benchmark 1GB single file transfer using `` lua `` and `` rsync ``.
2.   Add all features of cp command to any one of the implementation and bench mark.
