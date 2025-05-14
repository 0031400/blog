+++
date = '2025-05-10'
draft = false
title = 'python-installation'
slug = 'd56e7c13-cf5b-4e49-8fc0-b39ddee9ee57'
cover = 'https://i.withme.top/2025/05/10/d56e7c13-cf5b-4e49-8fc0-b39ddee9ee57.webp'
+++
We need to set up python environment by setting up a virtual environment.To achieve this,we can use the `venv` python module.
```bash
python -m venv <folder-path>
```
If we don't find the python,we can use the `python3` instead or install system python firstly with `sudo apt install python3-venv`.  
After that,we need to add the python virtual environment folder path to our environment variable `PATH` to make it our default python.Edit our `.bashrc` file.
```bash
export PATH=<python-env-folder-path>/bin:$PATH
```