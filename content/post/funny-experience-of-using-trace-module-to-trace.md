+++
date = "2013-07-27 21:35:32+00:00"
draft = false
tags = ["python", "trace", "httpie"]
title = "Funny experience of using trace module to trace function call"
url = "/post/56633639978/funny-experience-of-using-trace-module-to-trace"
+++
I came across this issue in <a href="https://github.com/jkbr/httpie/issues/128" target="_blank">httpie</a> and started my investigation.

The problem is while pretty printing the json, output is alpha sorted because keys are hashed and user wanted to preserve the order. Then I made 3 comments to the issue. <a href="https://github.com/jkbr/httpie/issues/128#issuecomment-21661339" target="_blank">First comment</a> was half correct and explained why it isn’t possible to get the desired output, quickly I figured my assumptions were wrong and <a href="https://github.com/jkbr/httpie/issues/128#issuecomment-21661648" target="_blank">second comment</a> explained what is actually happening, finally I proposed the <a href="https://github.com/jkbr/httpie/issues/128#issuecomment-21661700" target="_blank">solution</a>. Since I made wrong assumptions and to make further debugging easy, I want to find easiest way to trace all functions/methods invocations.

I remember <a href="http://nibrahim.net.in/" target="_blank">Noufal</a> sharing small snippet in twitter, but I wanted to use <a href="http://docs.python.org/2/library/trace.html" target="_blank">trace</a> and wrote small snippet(final one).

     #! -*- coding: utf-8 -*-

     from httpie.core import main
     #import pdb
     import sys
     #import os
     #import trace

     #pdb.set_trace()
     main(args=sys.argv[1:])

First I ran the command <code>python -m trace --trace httpie_test.py --pretty=all <a href="http://httpbin.org/get" target="_blank">http://httpbin.org/get</a></code> and it printed some 20 million lines. Then I got clever idea of using <a href="http://pythonconquerstheuniverse.wordpress.com/2009/09/10/debugging-in-python/" target="_blank">pdb</a>.

I debugged with pdb for half an hour pressing `` s `` key and fed up. The pdb was beautiful like her, I was enjoying each line it was printing, it was like watching her speak and I was mesemerized. After half an hour I gave up and went back to trace command. Finally I figured out I can use `` --ignore-module `` from command line.

After one hour of spending time, final command looked like(scroll completely)

<pre><code> ➜  snippets  python -m trace --trace --ignore- module=os,sre_compile,sre_parse,zipfile,text_file,sysconfig,pkg_resources,re,posixpath,genericpath,decoder,hex_codec,socket,httplib,pkgutil,stat,token,style,calendar,spawn,util,collections,abc,decimal,lexer,StringIO,plist,argparse,_abcoll,structures,Queue,threading,_collections,urlparse,cookielib,platform,cookielib,terminal256,formatter,__init__,stringprep,_weakrefset,filter,encoder,codecs,latin,connectionpool,plugin,html,pygmentplugin,htmlentitydefs,__future__,weakref,UserDict,atexit,functools,base64,struct,hashlib,ssl,textwrap,six,exceptions,warnings,mimetools,tempfile,random,tempfile,rfc822,urllib,filepost,uuid,_endian,dyld,dylib,framework,io,poolmanager,pyopenssl,utils,cgi,netrc,shlex,compat,copy,numbers,locale,locale,unicode_escape,urllib2,Cookie,_LWPCookieJar,_MozillaCookieJar,ordered_dict,cookies,certs,ascii,status_codes,gettext,scanner,config,minicompat,domreg,minidom,xmlbuilder,NodeFilter,shutil,copy_reg,string,plistlib,_mapping,bbcode,img,Image,FixTk,ImageMode,ImagePalette,ImageColor,ImageDraw,ImageFont,latex,other,console,rtf,svg,terminal,solarized,getpass,pprint,downloads,idna,agile,web,unistring,functional,jvm,compiled,mimetypes httpie_test.py --pretty=all --verbose <a href="http://headers.jsontest.com/" target="_blank">http://headers.jsontest.com/</a> >> op.txt
 ➜  snippets  wc -l op.txt
     2344 op.txt
</code></pre>

The essay is 1269 characters to terminal and the result of it is <a href="https://github.com/jkbr/httpie/pull/151" target="_blank">pull request</a>.
