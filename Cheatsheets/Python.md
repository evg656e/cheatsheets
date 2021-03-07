# Python Cheatsheet

## Index
 
 * [General](#General)
   * [Links](#Links)
 * [pip](#pip)
   * [Links](#Links-1)
   * [Questions](#Questions)
   * [Commands](#Commands)
 * [virtualenv](#virtualenv)
   * [Links](#Links-2)
   * [Install](#Install)
   * [Setup](#Setup)
   * [PYTHONPATH](#PYTHONPATH)
   * [Questions](#Questions-1)
 * [pylint](#pylint)
   * [Links](#Links-3)
 * [VSCode](#VSCode)
   * [Links](#Links-4)
   * [Install](#Install-1)
   * [settings.json](#settings.json)
   * [launch.json](#launch.json)
 * [Python 2 and 3 compatibility](#Python-2-and-3-compatibility)
   * [Links](#Links-5)
 * [Unit Testing with `unittest`](#Unit-Testing-with-unittest)
   * [Code samples](#Code-samples)
   * [Running](#Running)
   * [Links](#Links-6)
 * [Code coverage](#Code-coverage)
   * [Install](#Install-2)
   * [Usage with `unittest`](#Usage-with-unittest)
   * [Config](#Config)
   * [Links](#Links-7)


## General

### Links

 * [Online repl](https://www.onlinegdb.com/online_python_compiler)


## pip

### Links

 * [pip site](https://pip.pypa.io/en/stable/)
 * [Better Python dependency while packaging your project](https://medium.com/python-pandemonium/better-python-dependency-and-package-management-b5d8ea29dff1)

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
virtualenv -p C:/Python27/python.exe venv2
virtualenv -p "$env:LOCALAPPDATA/Programs/Python/Python39/python.exe" venv
```

### PYTHONPATH

 1. Create `.pth` file with one path to dir with modules per line, e.g. `mylib.pth`:
```
C:/evg656e/Projects/py/lib/common
C:/evg656e/Projects/py/lib/extras
```

 2. Place it into your project virtualenv's `site-packages` dir, e.g. `C:/evg656e/Projects/py/sample/venv/Lib/site-packages`

### Activate

```powershell
cd C:/evg656e/Projects/py/sample
./venv/Scripts/activate.ps1
# you will enter virtualenv here
# to exit from virtualenv type
deactivate
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

### Install
  
```console
pip install -U pylint
pip install -U autopep8
pip install -U rope
```

 * [pylint](https://www.pylint.org/)
 * [autopep8](https://pypi.org/project/autopep8/)
 * [rope](https://github.com/python-rope/rope)

### `settings.json`

```json5
{
    "python.pythonPath": "c:/evg656e/Projects/venv/stub/Scripts/python.exe",
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
        "./test",
        "-p",
        "test_*.py"
    ],
    "python.testing.autoTestDiscoverOnSaveEnabled": true,
    "python.testing.pytestEnabled": false,
    "python.testing.nosetestsEnabled": false,
    "python.testing.unittestEnabled": true
}
```

### `launch.json`

Launch program:
```json5
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Run main",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "args": [
                "--help",
            ],
            "console": "integratedTerminal",
        }
    ]
}
```

Launch program with `PYTHONPATH` set (better use [virtualenv](#virtualenv) with [.pth file](#PYTHONPATH)):
```json5
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Run main",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/main.py",
            "args": [
                "--help",
            ],
            "console": "integratedTerminal",
            "env": {
                "PYTHONPATH": "C:/evg656e/Projects/py/common;${env:PYTHONPATH}"
            },
        }
    ]
}
```

Launch module:
```json5
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Test",
            "type": "python",
            "request": "launch",
            "module": "unittest",
            "args": [
                "test/test_tools.py",
            ],
            "console": "integratedTerminal",
        }
    ]
}
```

Attach to process:
```json5
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Attach using Process Id",
            "type": "python",
            "request": "attach",
            "processId": "${command:pickProcess}"
        }
    ]
}
```


## Python 2 and 3 compatibility

### Links

 * [Six: Python 2 and 3 Compatibility Library](https://six.readthedocs.io/)
 * [Cheat Sheet: Writing Python 2-3 compatible code](https://python-future.org/compatible_idioms.html)


## Unit Testing with `unittest`

### Code samples

`mystring.py`:
```py
def capitalize(s):
    if len(s) == 0:
        return s
    return s[0].upper() + s[1:].lower()
```

`test_mystring.py`:
```py
import unittest

from mystring import capitalize


class TestMySrting(unittest.TestCase):
    # skip test
    @unittest.skip('')
    def test_capitalize_old(self):
        self.assertEqual(capitalize('foo'), 'Foo')
        self.assertEqual(capitalize('BUZZ'), 'Buzz')
        self.assertEqual(capitalize(''), '')

    # data driven test
    def test_capitalize(self):
        for i, (string, result) in enumerate([
            ['foo', 'Foo'],
            ['BUZZ', 'Buzz'],
            ['', '']
        ]):
            with self.subTest(i=i):
                self.assertEqual(capitalize(string), result)
```

### Running

 * Run by path:
```console
python -m unittest test/test_mystring.py test/test_mymath.py
```

 * Run by module name:
```console
python -m unittest test.test_mystring test.test_mymath
```

 * Run with test discover:
```console
python -m unittest discover -s ./test -p "test_*.py"
```

### Links:
 * [unittest](https://docs.python.org/3/library/unittest.html)


## Code coverage

### Install

```console
pip install coverage
```

### Usage with `unittest`

`python` becomes `coverage run` (with some extra `coverage` options if needed):
```console
coverage run --branch --omit "venv/*","test/*" -m unittest discover -s ./test -p "test_*.py"
```

Show results:
```console
coverage report
```

### Config

Create `.coveragerc` in the project's root directory:
```ini
[run]
branch = True

omit =
    venv/*
    test/*
```

### Links
 * [coverage](https://coverage.readthedocs.io/en/coverage-5.3/)
