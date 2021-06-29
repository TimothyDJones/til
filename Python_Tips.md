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
- [**Official** Python Beginner's Guide](https://wiki.python.org/moin/BeginnersGuide)

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
Dependency management in Python is a key aspect of successful development just as with any modern programming language. Python has a [somewhat notorious reputation](https://xkcd.com/1987/) when it comes to dependency management, but things are markedly better with Python 3. Python's solution to depenency management is the [virtual environment](https://docs.python.org/3/tutorial/venv.html), often abbreviated `venv`. Virtual environments provide an isolated run-time environment (self-contained directory tree) with the Python environment plus additional packages with specific versions applicable to the given application.

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
$ python3 -m pip freeze > requirements.txt
$ more requirements.txt
certifi==2020.12.5
chardet==3.0.4
idna==2.10
pkg-resources==0.0.0
requests==2.24.0
urllib3==1.25.11
```

### Tools for Managing Virtual Environments and Dependencies
While [`venv`](https://docs.python.org/3/library/venv.html) Python standard library module is the _de facto_ tool for creating and managing virtual environments, the Python community has many options that you may wish to evaluate for your particular situation. For a more comprehensive overview of these tools, see [this article](https://dev.to/bowmanjd/python-tools-for-managing-virtual-environments-3bko).
- [virtualenv](https://virtualenv.pypa.io/en/latest/) - This is the "granddaddy" of virtual environment tools and is actually the forerunner of the `venv` standard library module. Nevertheless, it has some handy extensions, such as "seeders" that allow you to configure all of your virtual environments with a set of standard packages. If you decide to use `virtualenv`, be sure to check out [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.io/), as well, since it provides a nice convenience wrapper around it to simplify use.
- [Poetry](https://python-poetry.org/) - Poetry takes a **declarative** approach to depenency management. You specify your dependencies in a configuration file ([`pyproject.toml`](https://snarky.ca/what-the-heck-is-pyproject-toml/)) or via the Poetry command-line tool (`poetry add package`). Poetry takes care of setting up your virtual environment, installing the packages, activating/deactivating the virtual environments and so forth. You just interact with Poetry itself.
- [tox](https://tox.readthedocs.io/) - At first glance, `tox` seems a bit more than just a virtual environment tool and that is, in fact, true. It is intended to be used as part of a CI/CD pipeline, specifically for testing against multiple Python versions using [`pytest`](https://docs.pytest.org/). I'm including here, since it's a widely used tool (you've probably seen the ubiquitous `tox.ini` file in various Github or Gitlab repositories). Moreover, tox provides a good framework for _gently_ transitioning from virtual environments to a more rigorous CI/CD environment.
- [Pyflow](https://github.com/David-OConnor/pyflow) - Pyflow is a new tool (built in Rust no less!) that also uses [`pyproject.toml`](https://snarky.ca/what-the-heck-is-pyproject-toml/) configuration. However, its "superpower" is that you can point it to a Python script itself that contains a `__requires__` directive with a list of package names and it will install those packages as dependencies and build out the appropriate `pyproject.toml` configuration.
- [pipenv](https://pipenv.pypa.io/en/latest/) - Pipenv was developed by [Kenneth Reitz](https://kenreitz.org/) of `requests` fame, so it's simple, solid and well-designed. It's designed to resolve some of the drawbacks of deterministic versioning with `requirements.txt` by using hash-based version management of its `Pipfile` and `Pipfile.lock` configuration files, similarly to other dependency management tools like `npm` for Node.JS and `composer` for PHP. As explained [here](https://www.mattlayman.com/blog/2017/using-pipfile-for-fun-and-profit/), `Pipfile` manages the _logical_ dependencies, meaning the explicit dependencies that your code has, such as `requests` library in the example above. `Pipfile.lock` acts as a dependency _manifest_ for the **transitive** dependencies resulting from your explicit logical dependencies, such as `urllib3` and `certifi` in the example above.
- [pyenv](https://github.com/pyenv/pyenv) - Pyenv is not actually a _virtual environment_ management tool _per se_, but instead is a Python _version_ management tool. It allows you to install multiple versions of the Python compiler/run-time and switch between them quickly and easily. Each time you add a new Python version, pyenv downloads the source code for that version and compiles it from scratch, so you also have flexibility with optimizations and so forth with the Python installation, as well.

## Generate random strings in Python
A common task in Python programming is generating random strings, such as passwords or dictionary keys. Here are some simple techniques for generating random strings of most any length using the Python [`random`](https://docs.python.org/3/library/random.html) and [`string`](https://docs.python.org/3/library/string.html) modules.

First, you need to determine the population of characters to use for your random strings. Good choices include the following constants from the `string` module:
- [`ascii_lowercase`](https://docs.python.org/3/library/string.html#string.ascii_lowercase) - `'abcdefghijklmnopqrstuvwxyz'`
- [`ascii_uppercase`](https://docs.python.org/3/library/string.html#string.ascii_uppercase) - `'ABCDEFGHIJKLMNOPQRSTUVWXYZ'`
- [`ascii_letters`](https://docs.python.org/3/library/string.html#string.ascii_letters) - The combination of `ascii_lowercase` and `ascii_letters`.
- [`digits`](https://docs.python.org/3/library/string.html#string.digits) - `'0123456789'`
- [`punctuation`](https://docs.python.org/3/library/string.html#string.punctuation) - `!"#$%&'()*+,-./:;<=>?@[\]^_``{|}~`

Several of the `punctuation` characters don't make great choices for such things as passwords, so perhaps you want to define your own subset like:
```python
SPECIAL_CHARS = '!#$%^&+-_=/@~'
```
You can then use any combination of these by concatenating them together:
```python
RAND_STR_CHARS = "".join(string.ascii_uppercase, string.digits, SPECIAL_CHARS)
```
If you want some of the characters to be more likely to be selected, just include them more than once in your definition of `RAND_STR_CHARS`.

### Use `lambda` function for simple random string generator
Now, you can create a simple `lambda` function that allows you pass in the length of string to generate.
```python
import random
import string

RAND_STR_CHARS = "".join(string.ascii_uppercase, string.digits, SPECIAL_CHARS)

# lambda function to generate random string of length _n_
rand_str = lambda n: "".join(random.SystemRandom().choice(RAND_STR_CHARS) for _ in range(n))

# Generate a random string of 10 characters
rand_str_10 = rand_str(10)
```

### Use UUID module
Another quick way to generate _unique_ random strings is to use the Python [`uuid`](https://docs.python.org/3/library/uuid.html) module. Since this module conforms to [RFC 4122](https://tools.ietf.org/html/rfc4122.html) the results are guaranteed to be unique. In particular, you can use the [`uuid4()`](https://docs.python.org/3/library/uuid.html#uuid.uuid4) method, which returns a _unique_ 36-character value, including **4** hyphens, or 32 [_hexadecimal_](https://en.wikipedia.org/wiki/Hexadecimal) digits: 0123456789ABCDEF. 

If you don't need all 32 hexadecimal digits, you can take a slice from the result, although remember that it may **not** be unique. In the technique below, we randomize the portion of the result returned in the slice to help _reduce_ the likelihood of duplication.
```python
import random
import uuid

# function to generate "random" string _n_ characters long (up to 32-characters)
def uuid_rand_str(n):
    n = int(n)
    if n > 32:
        raise ValueError("'rand_str' does not support strings greater than 32 characters in length.")
    slice_start = random.SystemRandom().randint(0, (31-(n-1)))
    return (uuid.uuid4().hex[slice_start:(slice_start+(n-1))])
    
# Generate a "random" string of 22 characters
rand_str_22 = uuid_rand_str(22)
```
Of course, you aren't _really_ limited to 32 characters with this method. You can simply use `uuid4()` multiple times to get longer strings. For example:
```python
def uuid_rand_str(n):
    n = int(n)
    # Number of times to run uuid4() method
    base = (n % 32) + 1
    slice_start = random.SystemRandom().randint(0, (((base*32)-1)-(n-1)))
    uuid_str = ""
    for _ in range(base):
        uuid_str = uuid_str.join(uuid.uuid4().hex)
    return (uuid_str[slice_start:(slice_start+(n-1))])
    
# Generate a "random" string of 100 characters
rand_str_100 = uuid_rand_str(100)
```

## Install Python package to global environment with `pip`
In most cases, you should use Python [virtual environments](https://docs.python.org/3/library/venv.html) for your work. However, in some situations, you may need to install a package globally, such as for linters (code-formatting checkers) like [pylint](https://pylint.org/) or [flake8](https://flake8.pycqa.org/) or the [IPython](http://ipython.org/) shell.

Typically, if you try to run `pip` against the global environment you'll get an error similar to the following.
```bash
$ python3 -m pip install flake8
ERROR: Could not find an activated virtualenv (required).
```
On Windows, this error will occur even if you are running as Administrator.

To correct the problem, you need to _temporarily_ disable the virtual environment requirement with the `PIP_REQUIRE_VIRTUALENV` environment variable. In Linux/Unix, at the shell prompt:
```bash
$ export PIP_REQUIRE_VIRTUALENV=false
```
And on Windows, at the command or Powershell prompt:
```powershell
$ set PIP_REQUIRE_VIRTUALENV=false
```
In the same shell prompt, run your `pip` install command as normal.