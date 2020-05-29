+++
date = "2013-12-16 16:38:44+00:00"
draft = false
tags = ["docker", "ubuntu"]
title = "Autogenerate Dockerfile from ubuntu image"
url = "/post/70198562577/autogenerate-dockerfile-from-ubuntu-image"
+++
I was learning docker to use in one of my projects. I kept installing packages, finally created required Ubuntu image. Suddenly thought it would be cool if I can generate `` Dockerfile `` like `` pip freeze > requirements.txt ``.

    sudo docker run ubuntu dpkg --get-selections | awk '{print $1}' >  base.list &amp;&amp; sudo docker run pyextra dpkg --get-selections | awk '{print $1}'> pyextra.list &amp;&amp; sort base.list pyextra.list | uniq -u > op.list &amp;&amp; python -c "f = open('Dockerfile', 'w');f.write('from ubuntu\n');f.write('RUN apt-get update\n');for line in open('op.list').readlines():f.write('run apt-get install -y {0}'.format(line));"

### Breakdown

Start the docker in daemon mode, then execute following commands.

`` sudo docker run ubuntu dpkg --get-selections | awk '{print $1}' >  base.list ``

Displays all packages installed in ubuntu base, extract the package name and write to `` base.list `` file.

`` sudo docker run pyextra dpkg --get-selections | awk '{print $1}'> pyextra.list ``

`` pyextra `` is docker image name. Get all installed packages names and write to `` pyextra.list ``.

`` sort base.list pyextra.list | uniq -u > op.list ``

Find the packages not in `` base `` distro and add to `` op.list ``. Now freshly installed package names are available.

    python -c "f = open('Dockerfile', 'w');
    f.write('from ubuntu\n');f.write('RUN apt-get update\n');
    for line in open('op.list').readlines():f.write('run apt-get install -y    {0}'.format(line));

Convert `` op.list `` file into `` Dockerfile ``.

### Disadavantages

*   This method produces dependencies as packages. `` apt-get install vim `` will depend on `` vim-common ``, `` vim-common `` is specified as package.

Is it worth to create a library which will do this ?
