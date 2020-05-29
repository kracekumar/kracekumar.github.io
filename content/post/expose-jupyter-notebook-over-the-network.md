
+++
date = "2017-01-24 20:21:21+00:00"
draft = false
tags = ["python", "jupyter", "ipython"]
title = "Expose jupyter notebook over the network"
url = "/post/156322146345/expose-jupyter-notebook-over-the-network"
+++
What is the Jupyter notebook?

>  
> The Jupyter Notebook is a web application that allows you to create and share documents that contain live code, equations, visualisations and explanatory text.
> 

The definition is from the <a href="https://jupyter.org/" target="_blank">official site</a>. I use `` IPython/Jupyter `` shell all time. If you haven’t tried, spend 30 minutes and witness the power!

At times, I want to share some code snippet with folks in the same building during work, workshop or training. The Jupyter notebook configuration allows the user to expose the notebook cluster, terminal over the network or the internet. The notebook is available over the network with two changes in the configuration file. The first config value is IP other than default localhost. The second one is the password for users to connect to the notebook.

*   Run the command `` jupyter notebook --generate-config `` to generate the config file. The command creates configuration lies in the path `` ~/.jupyter/jupyter_notebook_config.py ``. By default, the server listens on localhost. Modify the IP address to local machine IP. You can use `` ifconfig `` to find out the IP and change the value of `` c.NotebookApp.ip `` to IP like `` # c.NotebookApp.ip = '192.168.0.101' ``.
*   Next, find the line which contains `` c.NotebookApp.password `` declaration. Then execute `` from the notebook.auth import passwd; passwd() `` in the IPython/Jupyter shell. The code is to generate password, and placed above the attribute assignment. Enter the password; copy the output and assign the output to the attribute `` c.NotebookApp.password ``. Now run the server and open the URL `` http://IP:8888 `` from another PC or mobile to browse notebooks after entering the correct password.

The IP address can be overridden using the flag `` --ip `` like `` jupyter notebook --ip 192.168.0.100 ``
