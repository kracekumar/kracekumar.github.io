+++
date = "2013-07-02 16:50:39+00:00"
draft = false
tags = ["python", "ssl", "https", "flask"]
title = "SSL for flask local development "
url = "/post/54437887454/ssl-for-flask-local-development"
+++
Recently at <a href="https://blog.hasgeek.com/2013/https-everywhere-at-hasgeek" target="_blank">HasGeek</a> we moved all our web application to `` https ``. So I wanted to have all my development environment urls to have `` https ``.

_How to have `` https `` in <a href="http://flask.pocoo.org" target="_blank">flask</a> app_

_Method 1_

    from flask import Flask
    app = Flask(__name__)
    app.run('0.0.0.0', debug=True, port=8100, ssl_context='adhoc')

In the above piece of code, `` ssl_context `` variable is passed to `` werkezug.run_simple `` which creates SSL certificates using `` OpenSSL ``, you may need to install `` pyopenssl ``. I had issues with this method, so I generated self signed certificate.

_Method 2_

*   Generate a private key
    ` openssl genrsa -des3 -out server.key 1024 `


*

    Generate a CSR
    ` openssl req -new -key server.key -out server.csr `


*

    Remove Passphrase from key
    `cp server.key server.key.org `
    `openssl rsa -in server.key.org -out server.key `


*

    Generate self signed certificate
    `openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt `



_code_

    from flask import Flask
    app = Flask(__name__)
    app.run('0.0.0.0', debug=True, port=8100, ssl_context=('/Users/kracekumarramaraju/certificates/server.crt', '/Users/kracekumarramaraju/certificates/server.key'))

`` ssl_context `` can take option `` adhoc `` or `` tuple of the form (cert_file, pkey_file) ``.

In case you are using `` /etc/hosts ``, add something similar <code>127.0.0.1    <a href="https://hacknight.local" target="_blank">https://hacknight.local</a></code> for accessing the web application locally.

Here is much detailed post about generating self signed <a href="http://www.akadia.com/services/ssh_test_certificate.html" target="_blank">certificates</a>.
