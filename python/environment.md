## Setting up a virtual environment in Ubuntu

Install virtualenvwrapper

> pip install virtualenvwrapper

Create directory for virtual environments:

> mkdir ~/.venvs

Add in file ~/.bashrc following strings

```
export WORKON_HOME=~/.venvs
source /usr/local/bin/virtualenvwrapper.sh
```