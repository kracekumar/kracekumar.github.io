+++
date = "2013-12-22 11:50:16+00:00"
draft = false
tags = ["ipython", "python", "nbviewer", "ipynb2viewer"]
title = "ipynb2viewer - Afternoon hack"
url = "/post/70778191617/ipynb2viewer-afternoon-hack"
+++
`` ipython nbconvert `` has lot of handy options to convert `` ipynb `` to `` markdown ``, `` html `` etc… But I wanted to upload `` ipynb `` to `` gist.github.com `` and create a link in `` nbviewer.ipython.org ``. Started with `` curl `` and soon realized, it is getting messy. So wrote a small python program `` ipynb2viewer ``, <a href="https://github.com/kracekumar/ipynb2viewer" target="_blank">Source code</a>.

## Install

*   `` pip install ipynb2viewer ``

## Usage

Upload all ipynb files in the given path to gist.github.com and return nbviewer urls.

*   `` ipynb2viewer all <path> ``

Upload mentioned file to gist.github.com and return nbviewer url.

*   `` ipynb2viewer file <filename> ``

Upload mentioned file to gist.github.com and open nbviewer url in webbrowser.

*   `` ipynb2viewer file <filename> --open ``

Upload all ipynb files in the given path to gist.github.com and open nbviewer urls in webbrowser.

*   `` ipynb2viewer all <path> --open ``

## Example

<pre><code>➜  ipynb2viewer git:(master) ipynb2viewer all tests --open

Uploaded test.ipynb file to gist <a href="https://gist.github.com/8081202" target="_blank">https://gist.github.com/8081202</a>
nbviewer url: <a href="http://nbviewer.ipython.org/gist/anonymous/8081202" target="_blank">http://nbviewer.ipython.org/gist/anonymous/8081202</a>
Uploaded try.ipynb file to gist <a href="https://gist.github.com/8081203" target="_blank">https://gist.github.com/8081203</a>
nbviewer url: <a href="http://nbviewer.ipython.org/gist/anonymous/8081203" target="_blank">http://nbviewer.ipython.org/gist/anonymous/8081203</a>
</code></pre>
