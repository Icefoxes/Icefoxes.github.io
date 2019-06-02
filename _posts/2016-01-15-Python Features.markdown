---
layout:       post
title:        "Python Features"
subtitle:     "Python Features"
date:         2016-01-15 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Python
---

#### Genearl Concept
##### What is .pyd
`.pyd` files are DLL’s, but there are a few differences.

If you have a DLL named `foo.pyd`, then it must have a function `initfoo()`. You can import this dll in Python 
```python
import foo
```
Python will search for `foo.pyd` (as well as `foo.py`, `foo.pyc`) and if it finds this pyd, Python will attempt to call `initfoo()` to initialize it. 
You do not link your `.exe` with `foo.lib`, as that would cause Windows to require the DLL to be present.
Note that the search path for `foo.pyd` is **PYTHONPATH**, not the same as the path that Windows uses to search for foo.dll. 
Also, `foo.pyd` need not be present to run your program, whereas if you linked your program with a dll, the dll is required. 
Of course, foo.pyd is required if you want to say import foo. In a DLL, linkage is declared in the source code with `__declspec(dllexport)`. 
In a `.pyd`, linkage is defined in a list of available functions.

#### Python Property Define

```Python
class Student:
    def __init__(self):
        self._score = None

    def get_score(self):
        return self._score

    def set_score(self, value):
        self._score = value

    def del_score(self):
        del self._score

    score = property(get_score, set_score, del_score, "doc")


class Student:
    def __init__(self):
        self._score = None

    @property
    def score(self):
        """doc"""
        return self._score

    @score.setter
    def set_score(self, value):
        self._score = value

    @score.deleter
    def del_score(self):
        del self._score
```


#### Partial Function

##### Partial
```Python
import functools
int2 = functools.partial(int, base=2)
```

##### Wraps
```python
import time
from functools import wraps
 
def timethis(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(func.__name__, end-start)
        return result
    return wrapper
```

#### Built-in Variables

##### `globals()`
Return a dictionary representing the current global symbol table. This is always the dictionary of the current module (inside a function or method, this is the module where it is defined, not the module from which it is called.

##### `hash(object)`
Return the hash value of the object (if it has one). Hash values are integers. They are used to quickly compare dictionary keys during a dictionary lookup. Numeric values that compare equal have the same hash value (even if they are of different types, as is the case for 1 and 1.0).


#### IPDB
```python
import pdb;pdb.settrace();
```
**n[ext]**
`n` simply continues program execution to the next line in the current function. This will be your most-typed command.
**s[tep]**
`s` steps to the very next line of executable code, whether it’s inside a called method or just on the next line. I think of this as “step into” for tunneling into a function or method.
**c[ontinue]**
`c` continues program execution until another breakpoint is hit.
**l[ist] [first[, last]]**
`l` lists a larger portion of code–11 lines–moving down the file with successive calls. You can optionally provide a line number to center the view on, as well as a second number for a range.
**p[rint] and pp**
`p` and `pp` are print and pretty-print. pp uses the pprint module. I use pp almost exclusively.
**a[rgs]**
`a` is one of my favorites. It prints out all the arguments the current function received.
**j[ump] line_number**
j will jump to the given line of code, actually skipping the execution of anything between.

##### Tips for effective pdb use
- `pp locals()`and `pp globals()`
Use these commands to see all variables in the given scope, local or global. This is super useful 
for a quick glance at all the objects your program has access to at that moment.

- Change variables
You can change the value of variables while stepping through your code.

- Use tab completion and introspection
This is specific to ipdb users, since pdb doesn’t have this functionality. 

#### Python File Operation

##### Retrive Infomartion
```python
import os
 
path = r'D:\Data\1.txt'
# Get the last component of the path
os.path.basename(path)
# Get the directory name
os.path.dirname(path)
# Split the file extension
os.path.splitext(path)
 
# *******************************************************************
# Testing for the Existence of a File
os.path.exists(r'D:\1.txt')
# Is a regular file
os.path.isfile(path)
# Is a directory
os.path.isdir(path)
 
# get file/folder property
os.path.getsize(path)
os.path.getmtime(path)

import time
time.ctime(os.path.getmtime(path))
 
# *******************************************************************
# Getting a Directory Listing
folder = r'D:\data'
import os
names = os.listdir(folder)
# Get all regular files
names = [name for name in os.listdir(folder)if os.path.isfile(os.path.join(folder, name))]
# Get all dirs
dirnames = [name for name in os.listdir(folder) if os.path.isdir(os.path.join(folder, name))]
```
##### Create 
```Python
path = r'D:\1\2\3'
filename = r'D:\1\2\3\1.txt'
# make subdirs
os.makedirs(path)
# remove dir
os.rmdir(path)
# remove files
os.remove(filename)
# rename file
os.rename(oldfilename, newfilename)
```

##### Shutil
```python
# copyfile oldfile and newfile must be file
shutil.copyfile("oldfile","newfile")
# copyfile or dir 
# oldfile and newfile can be file or dir 
shutil.copy("oldfile","newfile")          
# copy dir
# old dir and new dir muse be dir cannot be file, and new dir cannot be exsit
shutil.copytree("olddir","newdir")        
# raname dir or file
os.rename("oldname","newname")  
# move dir or file
shutil.move("oldpos","newpos")   
# delete dir or file
os.remove("file")
# can delete empty dir
os.rmdir("dir")
```