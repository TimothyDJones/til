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