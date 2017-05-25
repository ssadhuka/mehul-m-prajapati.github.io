---
title: "Basics of Python Packaging"
excerpt: "Let's understand pip commands"
category: Python
tags: [pip, packaging]
header:
  image: https://i.imgur.com/tOdLKhy.png
  overlay_color: "#000"
  overlay_filter: "0.6"
  overlay_image: https://i.stack.imgur.com/Sedia.jpg
---
## Python Package Path 
To find location of python packages values of sys.path list is used,
```
import requests
import sys

# prints out path where packages could be located
print(sys.path)
['', '/usr/lib/python35.zip', '/usr/lib/python3.5', '/usr/lib/python3.5/plat-x86_64-linux-gnu', '/usr/lib/python3.5/lib-dynload', '/usr/local/lib/python3.5/dist-packages', '/usr/lib/python3/dist-packages']
```
In any project, we should setup virtual env and install packages related to project in that environment only.

## Python Packaging Authority (PyPA)

- Use setuptools for creating and distributing packages
- Use pip to install packages

### pip Commands
```
# Installing
pip install requests
pip install requests==1.0
# Comma is used to eliminate redirection in linux
pip install 'requests>=2.1'
# Un-installing
pip uninstall requests
```

```
# List all installed packages
pip3 list

# Get info of specific package
pip3 show requests

# Search for a package
pip3 search requests
```
**Cheese shop**: PyPi {Central repository for all the packages}
```
# List all installed packages with version number
pip3 freeze > requirements.txt

# Install all packages on other project member's machine
pip3 install -r requirements.txt
```

Let's have coffee :coffee:
