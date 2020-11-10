# Python Cheatsheet

## Index
 
 * [General](#General)
   * [Links](#Links)
 * [pip](#pip)
   * [Links](#Links)
   * [Questions](#Questions)
   * [Commands](#Commands)
 * [virtualenv](#virtualenv)
   * [Links](#Links)
   * [Install](#Install)
   * [Setup](#Setup)
   * [Questions](#Questions)
 * [pylint](#pylint)
   * [Links](#Links)
 * [VSCode](#VSCode)
   * [Links](#Links)
   * [Packages](#Packages)
   * [Configs](#Configs)
 * [Python 2 and 3 compatibility](#Python%202%20and%203%20compatibility)
   * [Links](#Links)


## General

### Links

 * [Online repl](https://www.onlinegdb.com/online_python_compiler)


## pip

### Links

 * [pip site](https://pip.pypa.io/en/stable/)

### Questions

 * [How can I upgrade specific packages using pip and a requirements file?](https://stackoverflow.com/questions/4536103/how-can-i-upgrade-specific-packages-using-pip-and-a-requirements-file)

### Commands

 * Get `requirements.txt`:
```console
pip freeze > requirements.txt
```


## virtualenv

### Links

 * [Guide](https://docs.python-guide.org/dev/virtualenvs/)
 * [virtualenv site](https://virtualenv.pypa.io/en/latest/)

### Install

```console
pip install virtualenv
```

### Setup

```powershell
virtualenv -p C:\Python27\python.exe venv2
virtualenv -p "C:\Users\$env:USERNAME\AppData\Local\Programs\Python\Python38-32\python.exe" venv3
```

### Questions

 * [How do I add a path to PYTHONPATH in virtualenv](https://stackoverflow.com/questions/10738919/how-do-i-add-a-path-to-pythonpath-in-virtualenv)
 * [Setting an environment variable in virtualenv](https://stackoverflow.com/questions/9554087/setting-an-environment-variable-in-virtualenv)
 * [Visual Studio Code - How to add multiple paths to python path?](https://stackoverflow.com/questions/41471578/visual-studio-code-how-to-add-multiple-paths-to-python-path/)


## pylint

### Links

 * [pylint site](http://pylint.pycqa.org/en/latest/)
 * [Readable pylint messages](https://github.com/janjur/readable-pylint-messages)


## VSCode

### Links
 * [VSCode Python](https://code.visualstudio.com/docs/python/python-tutorial)
 * [VSCode Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)

### Packages
  
```console
pip install -U pylint
pip install -U autopep8
pip install -U rope
```

### Configs

 * `settings.json`:
```json5
{
    "python.pythonPath": "c:\\evg656e\\Projects\\_venv\\sample\\Scripts\\python.exe",
    "python.linting.pylintEnabled": true,
    "python.linting.enabled": true,
    "python.linting.pylintArgs": [
        "--disable",
        "all",
        "--enable",
        "F,E,redefined-builtin,unreachable,duplicate-key,unnecessary-semicolon,global-variable-not-assigned,unused-variable,unused-import,binary-op-exception,bad-format-string,anomalous-backslash-in-string,bad-open-mode"
    ],
    "python.formatting.autopep8Args": [
        "--max-line-length",
        "120"
    ],
    "python.testing.unittestArgs": [
        "-v",
        "-s",
        "./sample/test",
        "-p",
        "test_*.py"
    ],
    "python.testing.autoTestDiscoverOnSaveEnabled": true,
    "python.testing.pytestEnabled": false,
    "python.testing.nosetestsEnabled": false,
    "python.testing.unittestEnabled": true
}
```

 * `launch.json`:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Run sample",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/sample/main.py",
            "args": [
                "--usage",
            ],
            "console": "integratedTerminal",
            // "env": {
            //     "PYTHONPATH": "C:\\evg656e\\Projects\\py\\common;${env:PYTHONPATH}"
            // }
        }
    ]
}
```

## Python 2 and 3 compatibility

### Links

 * [Six: Python 2 and 3 Compatibility Library](https://six.readthedocs.io/)
 * [Cheat Sheet: Writing Python 2-3 compatible code](https://python-future.org/compatible_idioms.html)
