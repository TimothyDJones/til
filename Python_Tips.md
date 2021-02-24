# Python Tips/Shortcuts

Tips and important things that I've learned for the [Python](https://www.python.org/) programming language.

## Cheat Sheets

- [Comprehensive Python Cheatsheet](https://gto76.github.io/python-cheatsheet/)
- [Learn X in Y Minutes](https://learnxinyminutes.com/docs/python/)
- [wikiPython](https://www.wikipython.com/) - Focused on Tkinter and GUI programming and RaspberryPi.

## Online Python Shells/IDEs

### Shells
- [Python.org](https://www.python.org/shell/)
- [Python Anywhere IPython](https://www.pythonanywhere.com/try-ipython/)

### IDEs
- [Python Tutor](http://www.pythontutor.com/visualize.html#mode=edit) - Graphical visualization of code execution
- [Python Fiddle](http://pythonfiddle.com/)
- [CodeSkulptor3](https://py3.codeskulptor.org/) - Supports GUI applications
- [IDEOne.com](https://ideone.com/l/python-3)
- [JDoodle](https://www.jdoodle.com/python3-programming-online/)
- [Paiza.io](https://paiza.io/en/projects/new?language=python3)
- [Programiz](https://www.programiz.com/python-programming/online-compiler/)
- [Online Python](https://www.online-python.com/)
- [Repl.it](https://www.programiz.com/python-programming/online-compiler/)
- [Rextester](https://rextester.com/l/python3_online_compiler)
- [Trinket.io](https://trinket.io/python)
- [Try It Online (TIO)](https://tio.run/#python3)
- [TutorialsPoint (Python 2.7)](https://www.tutorialspoint.com/execute_python_online.php)
- [W3Schools](https://www.w3schools.com/python/trypython.asp?filename=demo_compiler)

## Forums/Social Media
- [HackerNews](https://news.ycombinator.com/)
- [Dev Community Python](https://dev.to/t/python)
- [Lobsters Python](https://lobste.rs/t/python)
- [Reddit /r/Python](https://old.reddit.com/r/Python/)
- [Reddit /r/LearnPython](https://old.reddit.com/r/learnpython/)
- [Reddit /r/MadeInPython](https://old.reddit.com/r/madeinpython/)
- [Reddit /r/CoolGithubProjects](https://old.reddit.com/r/coolgithubprojects/search?q=flair%3A%27python%27&restrict_sr=on&sort=new&t=all#python)
- [Slashdot](https://slashdot.org/)

## Tutorials and Learning
- [Offical Python Beginner's Guide](https://wiki.python.org/moin/BeginnersGuide)

### Free Books

- [Python Crash Course, Second Edition](https://ehmatthes.github.io/pcc_2e/regular_index/) - [Eric Matthes](https://ehmatthes.com/)

## Modules and Exports

### The `__all__` directive
- Defines the _public_ interface of the module. In other words, it provides the **explicit** list of the names (methods, classes, etc.) that are exported by the module and that are available when imported by other Python modules. In particular, `__all__` ensures that **only** the symbols listed are available for use by import, allowing you to control access (i.e., to make methods, variables, and classes "private").
- For example, if you have a module named `module`, with `__init__.py` containing the following:
```python
# __init__.py

__all__ = ['foo', 'Bar']
```
Then, when `module` is used by your program by either of the following methods:
```python
import *
```
or (preferably)
```python
from module import *
```
This will import **only** the symbols `foo` and `Bar` (presumably a class, since it is capitalized) into program.
- Should be placed in the `__init__.py` file of the module.

[Reference1](https://stackoverflow.com/a/35710527)  
[Reference2](https://docs.python.org/tutorial/modules.html#importing-from-a-package)

## Virtual Environments and Dependency Management
Dependency management in Python is a key aspect of successful development just as with any modern programming language. Python's solution to depenency management is the [virtual environment](https://docs.python.org/3/tutorial/venv.html), often abbreviated `venv`. Virtual environments provide an isolated run-time environment (self-contained directory tree) with the Python environment plus additional packages with specific versions applicable to the given application.

The Python community differs on whether virtual environments should be contained within the project directory for an application or separate from (outside of) it. My _preference_ is to keep the `venv` **in** the project directory in a **hidden** directory named `.venv`. This helps to avoid confusion about which virtual environment directories correspond to which projects, especially if you rename your project directories.

The basic workflow for virtual environments is:
1. Create
2. Activate
3. Use
4. Deactivate  
Let's look at each of these phases in turn.

### Create
To create a virtual environment, in your shell (command prompt), navigate to the desired directory for it and run:
```bash
python3 -m venv .venv
```
Substitute whatever name you prefer for the virtual environment for `.venv` above. (Again, I use `.venv` so that it will be a hidden directory under Linux and the name shows that it is for the virtual environment, but you can use whatever name you like, such as the project name itself.)

### Activate
Once the virtual environment has been created, it has not yet been activated, meaning that the _system_ Python (or perhaps even another virtual environment!) is still active. To confirm this, on Linux, run `which python3` and it is likely to return `/usr/bin/python3`. We must _activate_ our new virtual environment. The process is slightly different between Linux/MacOS and Windows. On Windows, run:
```bash
.venv\Scripts\activate.bat
```
On Linux/MacOS, run:
```bash
source .venv/bin/activate
```
Again, as above, substitute the name of your virtual environment of `.venv` in these commands.

Activating the environment will update your command prompt by prepending the name of the virtual environment in parentheses to your original prompt. Likewise, it set several other environment variables to configure the environment to use the virtual environment. 

As above, run `which python3` to determine the path of the Python executable for the virtual environment. You should see it in a directory under the virtual environment now! You can further confirm this by opening the Python shell (REPL) and checking the `sys.path` parameter:
```bash
(.venv) $ python3
Python 3.8.5 (default, Jul 28 2020, 12:59:40) 
[GCC 9.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/usr/lib/python38.zip', '/usr/lib/python3.8',  '/home/tim/.venv/lib/python3.8/site-packages']
```

### Deactivate
Before we look at using our virtual environment, let's discuss how to _deactivate_ it. When we finished with a specific virtual environment, such as when we want to switch to another project, we want to make sure that we deactivate the virtual environment. This is necessary to avoid accidentally installing a package (or the wrong version of a package!) in that environment.

To deactivate our `venv`, we simply "undo" the _activate_ process from above. In Windows, run:
```bash
.venv\Scripts\deactivate.bat
```
In Linux/MacOS, _from any directory_ run:
```bash
deactivate
```
As with activating the virtual environment, the command prompt will change to _remove_ the reference to the virtual environment name and the other settings will likewise be removed, setting things back to the system Python environment.

### Using Your Virtual Environment
Once you've created and activated the virtual environment, you are ready to use it for your development work.

Typically, the first thing that you want to do is [ensure that `pip` is up to date](https://pythonspeed.com/articles/upgrade-pip/):
```bash
python3 -m pip install --upgrade pip
```
Now that you have the latest version of `pip`, you can install the other packages that you need, including any version-specific instances. For example, let's say that you need to install version _2.24_ of the [`requests`](https://requests.readthedocs.io/) HTTP library, due to a known issue with the later versions. In this case, you would run:
```bash
python3 -m pip install -v 'requests==2.24.0'
```
Since this is in the virtual environment, it does not interfere with any other projects that depend on `requests` which require the newer versions of the package.

Finally, you can use `pip` to create a `requirements.txt` file for your project with the version-specific instances of the packages, so that you can reproduce the same configuration in your production environment:
```bash
python3 -m pip freeze > requirements.txt
more requirements.txt
certifi==2020.12.5
chardet==3.0.4
idna==2.10
pkg-resources==0.0.0
requests==2.24.0
urllib3==1.25.11
```

### Tools for Managing Virtual Environments and Dependencies
While [`venv`](https://docs.python.org/3/library/venv.html) Python standard library module is the _de facto_ tool for creating and managing virtual environments, the Python community has many options that you may wish to evaluate for your particular situation.
- [pyenv]