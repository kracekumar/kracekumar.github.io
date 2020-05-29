
+++
date = "2017-12-09 19:19:08+00:00"
draft = false
tags = ["python", "debugging", "multiprocessing"]
title = "Debugging Python multiprocessing program with strace"
url = "/post/168364964555/debugging-python-multiprocessing-program-with"
+++
Debugging is a time consuming and brain draining process. It’s essential part of learning and writing maintainable code. Every person has their way of debugging, approaches and tools. Sometimes you can view the traceback, pull the code from memory, and find a quick fix. Some other times, you opt different tricks like the print statement, debugger, and rubber duck method.

Debugging multi-processing bug in Python is hard because of various reasons.

*   The print statement is simple to start. When all the processes write the log to the same file, there is no guarantee of the order without synchronization. You need to use `` -u `` or `` sys.stdout.flush() `` or synchronizing the log statements using process ids or any identifier. Without PID it’s hard to know which process is stuck.
*   You can’t put `` pdb.set_trace() `` inside a multi-processing pool target function.

Let’s say a program reads a metadata file which contains a list of JSON files and total records. Files may have a total of 100 JSON records, but you may need only 5. In any case, the function can return first five or random five records.

Sample code

<div class="gist"><a href="https://gist.github.com/kracekumar/20cfeec5dc99b084c879ce737f7c7214" target="_blank">https://gist.github.com/kracekumar/20cfeec5dc99b084c879ce737f7c7214</a></div>

The code is just for demonstrating the workflow and production code is not simple like above one.

Consider `` multiprocessing.Pool.starmap `` a function call with `` read_data `` as target and number of processes as 40.

Let’s say there are 80 files to process. Out of 80, 5 are problematic files(function takes ever to complete reading the data). Whatever be the position of five files, the processes continues forever, while other processes enter `` sleep `` state after completing the task.

Once you know the PID of the running process, you can look what system calls process calls using `` strace -s $PID ``. The process in `` running `` state was calling the system call `` read `` with same file name again and again. The while loop went on since the file had zero records and had only one file in the queue.

Strace looks like one below

<div class="gist">
<a href="https://gist.github.com/kracekumar/8cd44ff27b81e200343bc58d194dd595" target="_blank">https://gist.github.com/kracekumar/8cd44ff27b81e200343bc58d194dd595</a>
</div>

You may argue a well-placed print may solve the problem. All times, you won’t have the luxury to modify the running program or replicate the code in the local environment.
