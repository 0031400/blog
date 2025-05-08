---
title: "python installation"
description: 
date: 2025-05-07T19:40:47+08:00
image: "image.png"
math: false
draft: false
---
### installation
#### check the python at present
Check if python has been installed.  
We should use the python 3 rather the python 2.  
`python3 -V` or `python -V`
If the output is like this `Python 3.11.2` ,your cloud server is equipped with python.All we need is to set up a python virtual environment and make it the environment we use in the shell.
If not,we need the download the python using `apt` , run the command `sudo apt install python3-venv` and repeat the step above.
#### make the parent folder for python environment
Cd the parent folder where you want to put your python.Like me,I like to put it `/home/lxz/app/python3.11.2`.So I run the command `cd /home/lxz/app` after I assure the folder `/home/lxz/app` has been created after I run the command `mkdir -p /home/lxz/app`.  
#### set up the env
Run the command to set up python environment `python3 -m venv <name>`.The `name` is the position you want to setup the python environment and it is also the python environment folder name.Like me,I usually to set it the python version like `python3.11.2`.You can also run from everywhere to run `python3 -m venv <the absolute path>`.
#### add it to the bash path
We need to add python environment to our path to make sure when we run the python command,we are using the virtual environment rather the system python to make sure not to harm the system.  
Edit the file `~/.bashrc` using editor like `vim`  
Put the below sentence to the end of the file.
```bash
export PATH=<python_dir>/bin:$PATH
```
Run the command to activate the `.bashrc` `. ~/.bashrc`
#### test the python
Run the command to see if the python has been successfully installed.  
`<python_dir>/bin/python -V`  
If it print linke this `Python 3.11.2` ,the python has been successfully installed. 
#### set up the pip
Some system is not equipped with pip.We can download it manually.  
download the file [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) using the command `wget https://bootstrap.pypa.io/get-pip.py`  
Run the python file using `python3 get-pip.py`  
After install the pip,delete the `get-pip.py` with `rm get-pip.py`
#### test the pip
Run the command `pip -V`,you can see the pip path from the output like this.
```
pip 23.0.1 from /home/lxz/app/python3.11.2/lib/python3.11/site-packages/pip (python 3.11)
```
See if the path is right.If not,check the above step about the `.bashrc`