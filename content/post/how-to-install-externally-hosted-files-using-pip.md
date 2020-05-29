
+++
date = "2014-05-12 18:56:00+00:00"
draft = false
tags = ["python", "pip"]
title = "How to install externally hosted files using pip"
url = "/post/85545169530/how-to-install-externally-hosted-files-using-pip"
+++
As of writing (12, May 2014) latest version of `` pip `` is `` 1.5.1 ``. `` pip `` doesn’tallow installing packages from non <a href="https://pypi.python.org" target="_blank">PyPI</a> based url.It is possible to upload `` tar `` or `` zip `` or `` tar.gz `` file to `` PyPI `` or specify`` download url `` which points other sites(Example: `` pyPdf `` points to <code><a href="http://pybrary.net/pyPdf/pyPdf-1.13.tar.gz" target="_blank">http://pybrary.net/pyPdf/pyPdf-1.13.tar.gz</a></code>).`` pip `` considers externally hosted packages as insecure. Agreed.

This is one of the reason why I kept using `` pip 1.4.1 ``. Finally decided to fix this issue.Below is the sample error which `` pip `` throws.

    (document-converter)➜  document-converter git:(fix_requirements) pip install pyPdf
    Downloading/unpacking pyPdf
    Could not find any downloads that satisfy the requirement pyPdf
    Some externally hosted files were ignored (use --allow-external pyPdf to allow).
    Cleaning up...
    No distributions at all found for pyPdf
    Storing debug log for failure in /Users/kracekumar/.pip/pip.log
    
    (document-converter)➜  document-converter git:(fix_requirements) pip install --allow-external pyPdf
    You must give at least one requirement to install (see "pip help install")
    (document-converter)➜  document-converter git:(fix_requirements) pip install pyPdf --allow-external pyPdf
    Downloading/unpacking pyPdf
    Could not find any downloads that satisfy the requirement pyPdf
    Some insecure and unverifiable files were ignored (use --allow-unverified pyPdf to allow).
    Cleaning up...
    No distributions at all found for pyPdf
    Storing debug log for failure in /Users/kracekumar/.pip/pip.log

The above method is super confusing and counter intutive. Fix is

    (document-converter)➜  document-converter git:(fix_requirements) pip install pyPdf --allow-external pyPdf --allow-unverified pyPdf
    Downloading/unpacking pyPdf
    pyPdf an externally hosted file and may be unreliable
    pyPdf is potentially insecure and unverifiable.
    Downloading pyPdf-1.13.tar.gz
    Running setup.py (path:/Users/kracekumar/Envs/document-converter/build/pyPdf/setup.py) egg_info for package pyPdf
    
    Installing collected packages: pyPdf
    Running setup.py install for pyPdf
    
    Successfully installed pyPdf
    Cleaning up...

The above method is not used in production environment. In production environment it is recommended to do `` pip install -r requirements.txt ``.

    # requirements.txt
    --allow-external pyPdf
    --allow-unverified pyPdf
    pyPdf
    --allow-external xhtml2pdf==0.0.5

`` pyPdf `` has two issues, two flags needs to mentioned in `` requirements.txt ``. Since `` xhtml2pdf `` requires `` pyPdf `` `` --allow-external `` flag is passed. Wish it was possible to pass both the switches in same line. If you do so `` pip `` will ignore it. Now running `` pip install -r requirements.txt `` will works like a charm(with warnings).

Since current approach is super confusing, there is a <a href="https://github.com/pypa/pip/pull/1812" target="_blank">discussion</a>. Thanks <a href="https://github.com/Ivoz" target="_blank">Ivoz</a> for helping me to <a href="https://github.com/pypa/pip/issues/1816#issuecomment-42848611" target="_blank">resolve</a> this.
