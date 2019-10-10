# pytato
Python environment tool

## Requirements
ptpython: `pip install ptpython`  
pipenv: `pip install --user pipenv`  
(May need to update path to use `pipenv` command without full path) . 

## Installation
Simply download the `pytato.py` file, make it executable, and place it in your path.

## Description  
pyenv is a tool that allows you to organize your python virtual environments and easily switch between them. The two main functionalities include spawning a ptpython REPL and running a python script in a specific environment. 
### Default Setup
Environments are expected to be stored and organized in `~/env/`. A simple example of a supported structure looks like this:
```
~/env
├── dev
│   ├── .venv
│   ├── Pipfile
│   └── Pipfile.lock
├── master
│   ├── .venv
│   ├── Pipfile
│   └── Pipfile.lock
└── test
    ├── .venv
    ├── Pipfile
    └── Pipfile.lock
```
Any environment you would like to define, you would create a directory under `~/env/`, add a .venv folder and create a Pipfile that defines that environment. You'll need to intialize and maintain the environments outside of this tool.  
### Custom Environments
If you have a Pipfile or virtual environment defined somewhere else on your system and you would still like to use this tool, you can use the `-p` command to supply the path to a different folder containing environments.  
Example:
```
~/git/repos
├── project
│   ├── .venv
│   ├── Pipfile
│   └── Pipfile.lock
├── other_project
│   ├── .venv
│   ├── Pipfile
│   └── Pipfile.lock
└── testing
    ├── .venv
    ├── Pipfile
    └── Pipfile.lock
```
`pytato -p ~/git/repos other_project -r`  
Above command will spawn a ptpython repl in the virtual environment defined in other_project/Pipefile.

## Usage
```
usage: pytato [-h] [-r | -s SCRIPT | -a ADD_ENVIRONMENT | -u | -e] [-p PATH]
              env

positional arguments:
  env                   Environment to activate. By default this is a
                        directory name after ~/env/ that contains a .venv
                        directory. Should be a directory name following your
                        path argument if default path is not present. Example:
                        if env=dev then ~/env/dev/.venv should exist.

optional arguments:
  -h, --help            show this help message and exit
  -r, --repl            Use to activate a ptpython REPL in selected
                        environment. Cannot use this with -s option.
  -s SCRIPT, --script SCRIPT
                        Supply a script to run in the virtual environment
                        chosen. Cannot use this with -r option.
  -a ADD_ENVIRONMENT, --add-environment ADD_ENVIRONMENT
                        Supply the path to a directory that contains a .venv
                        file
  -u, --update          Use this to run pipenv update in the supplied
                        environment.
  -e, --edit            Use this to open up the Pipefile in VIM and make edits
  -p PATH, --path PATH  The path to where your virtual environments are
                        located. ~/env/ by default

    For an environment located in ~/env/dev/.venv:
    pytato dev -r
    pytato dev -s script.py

    For an environment located in non-default location: [/path/to/env/dev/.venv]
    pytato -p /path/to/env dev -r
    pytato -p /path/to/env dev -s script.py

    To move to a environments directory:
    cd $(pytato dev) -> cd ~/env/dev
    cd $(pytato -p ~/path/ env) -> cd ~/path/env

    Adding an existing environment to pytato:
    pytato new_env -a /path/to/existing/env
    This will create a softlink in the default path location to
    this new environment.

    Editing a Pipfile in a pytato environment:
    pytato dev -e
    This will open up the Pipfile in VIM for editing.

    Updating an environment in pytato:
    pytato dev -u
    In this example, this will run 'pipenv update' within the directory of the dev environment.
```
