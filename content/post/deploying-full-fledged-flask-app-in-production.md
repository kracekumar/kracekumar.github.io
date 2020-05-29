+++
date = "2013-12-25 18:08:34+00:00"
draft = false
tags = ["python", "flask", "nginx", "uwsgi", "deployment"]
title = "Deploying full fledged flask app in production"
url = "/post/71120049966/deploying-full-fledged-flask-app-in-production"
+++
This article will focus on deploying flask app starting from scratch like creating separate linux user, installating database, web server. Web server will be <a href="http://wiki.nginx.org/Main" target="_blank">nginx</a>, database will be <a href="http://www.postgresql.org/" target="_blank">postgres</a>, python 2.7 middleware will be <a href="http://uwsgi-docs.readthedocs.org" target="_blank">uwsgi</a>, server <a href="http://www.ubuntu.com/download/server" target="_blank">ubuntu 13.10 x64</a>. Flask app name is `` fido ``. Demo is carried out in <a href="https://www.digitalocean.com/?refcode=6c1c3a08e4ab" target="_blank">Digital ocean</a>.

### Step 1 - Installation


__Python header__

    root@fido:~# apt-get install -y build-essential python-dev

__Install uwsgi dependencies__

    root@fido:~# apt-get install -y libxml2-dev libxslt1-dev

__Nginx, uwsgi__

    root@fido:~#  apt-get install -y nginx uwsgi uwsgi-plugin-python

Start nginx

    root@fido:~# service nginx start
    * Starting nginx nginx                                                          [ OK ]

__Postgres__

    root@fido:~# apt-get install -y postgresql postgresql-contrib libpq-dev

### Step 2 - User

__Create a new linux user fido__

    root@fido:~# adduser fido

Enter all the required details.

    root@fido:~# ls /home
    fido

Successfully new user is created.

Grant `` fido `` root privilege.

    root@fido:~# /usr/sbin/visudo
    # User privilege specification

    root    ALL=(ALL:ALL) ALL
    fido    ALL=(ALL:ALL) ALL

Since `` fido `` is not normal user, delete fido’s home directory.

    root@fido:~# rm -rf /home/fido
    root@fido:~# ls /home
    root@fido:~#

__Create a new db user fido__

    root@fido:~# su - postgres
    postgres@fido:~$ createuser --pwprompt
    Enter name of role to add: fido
    Enter password for new role:
    Enter it again:
    Shall the new role be a superuser? (y/n) y

`` --pwprompt `` will prompt for password. `` release `` is the password I typed (we need this to connect db from app).

__Create a new database fido__

    postgres@fido:~$ createdb fido;
    postgres@fido:~$ psql -U fido -h localhost
    Password for user fido:
    psql (9.1.10)
    SSL connection (cipher: DHE-RSA-AES256-SHA, bits: 256)
    Type "help" for help.
    fido=# \d
    No relations found.

Done. New database role `` fido `` and database is created. We are successfully able to login.

### Step 3 - Python dependencies

__Install pip__

<pre><code>root@fido:# cd /tmp
root@fido:/tmp# wget <a href="https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py" target="_blank">https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py</a>
root@fido:/tmp# python ez_setup.py install
root@fido:/tmp# easy_install pip
# What is easy_install ? Python package manager.
# what is pip ? Python package manager.
# How to install pip ? easy_install pip.
# Shame on python :-(
...
Installed /usr/local/lib/python2.7/dist-packages/pip-1.4.1-py2.7.egg
Processing dependencies for pip
Finished processing dependencies for pip
</code></pre>

__Install virtualenv__

    root@fido:/tmp# pip install virtualenv

### Step 4 - Install app dependencies

Here is the sample app code. The app is just for demo. The app will be placed in `` /var/www/fido ``. Normally in production, this will be similar to `` git clone <url> `` or `` hg clone <url> `` inside directory. Make sure you aren’t sudo while cloning.

    root@fido:/tmp# cd /var
    root@fido:/var# mkdir www
    root@fido:/var# mkdir www/fido

Change the owner of the repo to `` fido ``.

    root@fido:/var# chown fido:fido www/fido
    root@fido:/var# ls -la www/
    total 12
    drwxr-xr-x  3 root root 4096 Dec 25 03:18 .
    drwxr-xr-x 14 root root 4096 Dec 25 03:18 ..
    drwxr-xr-x  2 fido fido 4096 Dec 25 03:18 fido

`` app.py `` - fido application.

    # /usr/bin/env python

    from flask import Flask, request
    from flask.ext.sqlalchemy import SQLAlchemy

    app = Flask(__name__)
    app.config['SQLALCHEMY_DATABASE_URI'] = "postgres://fido:release@localhost:5432/fido"
    db = SQLAlchemy(app)


    class Todo(db.Model):
        id = db.Column(db.Integer(), nullable=False, primary_key=True)
        name = db.Column(db.UnicodeText(), nullable=False)
        status = db.Column(db.Boolean(), default=False, nullable=True)


    @app.route("/")
    def index():
        return "Index page. Use /new create a new todo"


    @app.route('/new', methods=['POST'])
    def new():
        form = request.form
        name, status = form.get('name'), form.get('status') or False
        todo = Todo(name=name, status=status)
        db.session.add(todo)
        db.session.commit()
        return "Created todo: {}".format(name)


    if __name__ == "__main__":
        db.create_all()
        app.run('0.0.0.0', port=3333, debug=True)

Add a `` wsgi `` file `` website.py ``

    root@fido:/var/www/fido# cat website.py
    import sys
    import os.path
    sys.path.insert(0, os.path.dirname(__file__))
    from app import app as application

Files in `` fido `` directory.

    root@fido:/var/www/fido# tree .
    .
    ├── app.py
    ├── __init__.py
    └── website.wsgi

    0 directories, 3 files

### Step 5 - Virtual env and dependencies

    root@fido:/var/www/fido# virtualenv --no-site-packages env
    root@fido:/var/www/fido# . env/bin/activate
    (env)root@fido:/var/www/fido# pip install flask sqlalchemy flask-sqlalchemy psycopg2

### Step 6 - final setup

__Create uwsgi config file__

    # Add following lines to fido.ini file
    root@fido:/etc# cat uwsgi/apps-enabled/fido.ini
    [uwsgi]
    socket = 127.0.0.1:5000
    threads = 2
    master = true
    uid = fido
    gid = fido
    chdir = /var/www/fido
    home = /var/www/fido/env/
    pp = ..
    module = website

Check whether uwsgi is booting up properly.

    root@fido:/var/www/fido# uwsgi --ini /etc/uwsgi/apps-enabled/fido.ini
    ...
    ...
    Python version: 2.7.5+ (default, Sep 19 2013, 13:52:09)  [GCC 4.8.1]
    Set PythonHome to /var/www/fido/env/
    Python main interpreter initialized at 0xb96a70
    python threads support enabled
    your server socket listen backlog is limited to 100 connections
    your mercy for graceful operations on workers is 60 seconds
    mapped 165920 bytes (162 KB) for 2 cores
    *** Operational MODE: threaded ***
    added ../ to pythonpath.
    WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0xb96a70 pid:  17559 (default app)
    *** uWSGI is running in multiple interpreter mode ***
    spawned uWSGI master process (pid: 17559)
    spawned uWSGI worker 1 (pid: 17562, cores: 2)

`` uwsgi `` is able to load the app without any issues. Kill the uwsgi using keyboard interrupt.

Now lets create table.

<pre><code>root@fido:/var/www/fido# . env/bin/activate
(env)root@fido:/var/www/fido# python app.py
* Running on <a href="http://0.0.0.0:3333/" target="_blank">http://0.0.0.0:3333/</a>
* Restarting with reloader
</code></pre>

Exit the program, `` db.create_all() `` must have created the table. Normally in production environment, it is advised to use `` python manage.py db create `` or any similar approach.

__Configure nginx__

    root@fido:/var/www/fido# cat /etc/nginx/sites-enabled/fido.in
    upstream flask {
        server 127.0.0.1:5000;
    }

    # configuration of the server
    server {
        # the domain name it will serve for
        listen 127.0.0.1; # This is very important to test the server locally
        server_name fido.in; # substitute your machine's IP address or FQDN
        charset     utf-8;

        location / {
            uwsgi_pass  flask;
            include uwsgi_params;
        }
    }

Now `` nginx `` and `` uwsgi `` will be running in background. Restart them.

    root@fido:/var/www/fido# service nginx restart
    * Restarting nginx nginx                                                             [ OK ]
    root@fido:/var/www/fido# service uwsgi restart
    * Restarting app server(s) uwsgi                                                    [ OK ]

Machine name is `` fido ``, so lets try `` curl http://fido ``

    root@fido:/var/www/fido# curl http://fido
    Index page. Use /new create a new todo

Create a new task.

    root@fido:/var/www/fido#curl --data "name=write blog post about flask deployment"  http://fido/new
    Created todo: write blog post about flask deployment

We have successfully deployed `` flask `` + `` uwsgi `` + `` nginx ``.

Since we installed `` uwsgi `` from ubuntu repo, it is started as `` upstart `` process, that is why we issue commands like `` service uwsgi restart ``.

To see all upstart service, try `` service --status-all ``.

If you are running multiple web application in a single server, create one user per application.
