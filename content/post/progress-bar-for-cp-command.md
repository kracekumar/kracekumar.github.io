
+++
date = "2013-06-14 17:48:41+00:00"
draft = false
tags = ["linux", "command line"]
title = "progress bar for cp command"
url = "/post/52958861147/progress-bar-for-cp-command"
+++
I want `` cp `` command to display the progress of copy. I found three options

1.   Use tools like `` gcp ``, `` pv ``. Answer from <a href="http://askubuntu.com/questions/17275/progress-and-speed-with-cp" target="_blank">askubuntu</a>.
2.   Use `` rsync ``. `` alias cp='rsync --progress -ah' ``
3.   Use this <a href="https://chris-lamb.co.uk/posts/can-you-get-cp-to-give-a-progress-bar-like-wget" target="_blank">shell script</a>.
