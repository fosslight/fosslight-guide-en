---
published: true
---
# Virtualenv setting guide

Guides how to set up virtualenv environment to run Python Package.

## Contents
- [Install Python and python-dev](#python)
- [Set up virtualenv](#virtualenv)
- [Virtualenv command](#command)

## 💻 <a name="python"></a>Install Python, python-dev

- Refer to the [Installation Guide][install] for how to install Python.
- Install python-dev according to the python version you are using.
    - ex. In case of python 3.6,
      `sudo apt-get install python3.6-dev`

[install]: https://realpython.com/installing-python

## 📋 <a name="virtualenv"></a>Create and activate virtualenv

See the [Python virtaulenv page][venv] for details.
```
$ pip3 install virtualenv
$ virtualenv -p /usr/bin/python3.6 venv
$ source venv/bin/activate
```

[venv]: https://docs.python.org/3.6/library/venv.html

## ⌨️ <a name="command"></a>Virtualenv commands

| Command description  | command |
| ------------- | ------------- |
| Create a virtual environment. | virtualenv -p [python_version] [env_name] |
| Activate a virtual environment. | source [env_name]/bin/activate |
| Deactivate a virtual environment | deactivate | 
