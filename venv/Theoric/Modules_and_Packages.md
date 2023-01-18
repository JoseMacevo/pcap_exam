# _Python Modules and Packages - An introduction._

## Table of Contents
* Python Modules: Overview.
* The Module Search Path.
* The Import Statement.
	* Import (module_name).
	* from (module_mame) import (name(s))
	* from (module_name) import (name) as (alias_name)
	* import (module_name) as (alias_name)
* The dir() function.
* Executing a Module as a Script.
* Reloading a Module.
* Python Packages.
* Package initialization.
* Importing * from a Package.
* Subpackages.
* Conclusion.

This article explores Python modules and Python Packages, two mechanisms that facilitate **_modular programming._**

**Modular programming:** Refers to the process of breakings a large, unwieldy
programming task into separate, smaller and more manageable subtasks or **Modules**.

There are several advantages to **modularizing** code in a large application:

* _Simplicity:_ Rather than focusing on the entire problem at hand, a module
typically focuses on one relatively small portion of the problem. If you're working on a single module, you'll have a smaller problem domain to wrap your head around. This makes development easier and less error-prone.
	
* _Maintainability:_ Modules are typically designed so that they enforce logical boundaries between different problem domains. If modules are written in a way that minimizes interdependency, there is decreased likelihood that
modifications to a single module will have an impact on other parts of the program. (You may even be able to make changes to a module without
having any knowledge of the application outside that module.)This makes
it more viable for a team of many programmers to work collaboratively on a
large application.

* _Reusability:_ Functionality defined in a single module can be easily reused
(through an appropriately defined interface) by other parts of the application. This eliminates the need to duplicate code.

* _Scoping:_ Modules typically define a separate namespace, which helps
avoid collisions between identifiers in different areas of a program. (One of the tenets in the Zen of Python is Namespaces are one honking great idea let's more of those!).

**Functions, modules and packages are all constructs in Python that promote code modularization.**

## Python Modules: Overview.
There are actually three different ways to define a _module_ in Python:
1. A module can be written in Python itself.
2. A module can be written in C and loaded dynamically at run-time, like the _re module_ [regular expresions](https://docs.python.org/3/library/re.html?highlight=re#module-re).
3. A _built-in_ module is intrinsically contained in the interpreter, like the [intertools module](https://realpython.com/python-itertools/)

A module's contents are accessed the same way in all three cases: With the **import** statement.

Here, the focus will mostly be on modules that are written in Python. The cool
thing about modules written in Python is that they're exceedingly straightforward to build. All you need to do is create a file that contains legitimate Python code and then give the file a name with a _.py_ extension. That's it! No special syntax or voodoo is necessary.

For example, suppose you have created a file called _mod.py_ containing the following.

### mod.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages.png)



Several objects are defined in _mod.py_

* s (a string)
* a (a list)
* foo() (a function)
* Foo (a class)


 Assuming _mod.py_ is in an appropriate location, which you'll learn more about shortly, these objects can be accessed by **importing** the module as follows.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_1.png)


## The Module Search Path

Continuing with the above example, let's take a look at what happens when
Python executes the statement.

### Python
![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_2.png)


When the interpreter executes the above _import_ statement, it searches for _mod.py_ in a [list](https://realpython.com/python-lists-tuples/) of directories assembled from the following sources:

* The directory from which the input script was run o the **current directory** if the interpreter is being run interactively.
* The list of directories contained in the [PYTHONPATH](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH) environment variable, if it is set. (The format for PYTHONPATH is OS-dependent but should mimic the [PATH](https://realpython.com/add-python-to-path/) environment variable.)
* An installation-dependent list of directories configured at the time Python is installed.

The resulting search path is accesible in the Python variable **sys.path**, wich is obtained from a module named **sys**.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_3.png)


Thus, to ensure your module is found, you need to do one of the following.

* Put **mod.py** in the directory where the input script is located or the **current directory**, if interactive.
* Modify the PYTHONPATH environment variable to contain the directory where **mod.py** is located before starting the interpreter.
	* Or:Put **mod.py** in one of the directories already contained in the
	PYTHONPATH variable.
* Put **mod.py** in one of the installation-dependent directories, which you may
or may not have have write-access to, depending on the OS.

There's actually one additional option: you can put the module file in any
directory of your choice and then modify **sys.path** at run-time so that it contains that directory. For example, in this case, you could put **mod.py** in directory C:\Users\john and then issue the following statements.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_4.png)


Once a module has been imported, you can determine the location where it was found with the module's __file__ attribute.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_5.png)


The directory portion of __file__ should be one of the directories in _sys.path_.


## The _import_ statement.

**Module** contents are made available to he caller with the _import_ statement. The _import_ statement takes many different forms, shown below.

**_IMPORT <MODULE_NAME>_**

The simplest form is the one already shown above.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_6.png)

Note that this _does not_ make the module contents _directly_ accessible to the caller.

Each module has its own **private symbol table,** which serves as the global
symbol table for all objects defined **_in the module_**. Thus, a module creates a separate **namespace,** as already noted.

The statement **import <module_name>** only places **<module_name>** in the caller's symbol table. The _objects_ that are defined in the module **remain in the module's private symbol table.**

From the caller, objects in the module are only accessible when prefixed with <nodule_name> via _dot notation_, as illustrated below.

After the following **import** statement, **mod** is placed into hte local symbol table.

Thus, **mod** has meaning in the caller's local context:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_7.png)


But  **->S<-** and **->foo<-** remain in the module's private symbol table and are not meaningful in the local context:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_8.png)


To be accessed in the local context, names of objects defined in the module must
be prefixed by **mod**:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_9.png)

Several comma-separated modules may be specified in a single **import** statement.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_10.png)


## from <module_name> import <name(s)>

An alternate form of the **import** statement allows individual objects from the module to be imported _directly into the caller's symbol table_

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_11.png)


Following execution of the above statement, <name(s)> can be referenced in the
caller's environment without the <module_name> prefix.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_12.png)


Because this form of **import** places the object names directly into the caller's symbol table, any objects that already exist with the same name will be
**overwritten**.


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_13.png)


It is even possible to indiscriminately _import_ everything from a module at one fell swoop:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_14.png)


This will place the names of _all_ objects from <module_name> into the local symbol table, with the exception of any that begin with the underscore(_) character.

For example:



![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_15.png)


This isn't necessarily recommended in large-scale production code. It's a bit dangerous because you're entering names into the local symbol table _en masse_.
Unless you know them all well and can be confident there won't be a conflict, you have a decent chance of overwriting an existing name inadvertently. However, this syntax is quite handy when you're just mucking around with the interactive interpreter, for testing or discovery purposes, because it quickly gives you access to everything a module has to offer without a lot of typing.


## from <module_name> import <_name_> as <alt_name>

It is also possible to _import_ individual objects but enter them into the local symbol table with alternate names:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_16.png)



This makes it possible to place names directly into the local symbol table but avoid conflicts with previously existing names:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_17.png)


## import <module_name> as <alt_name>

You can also import an entire module under an alternate name:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_01.png)

Module contents can be imported from within a [function definition](https://realpython.com/defining-your-own-python-function/). In that case, the _import_ does _not occur_ until the function is called:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_02.png)

However, **Python 3** does not allow the indiscriminate import sintax (*) from within a function:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_03.png)

Lastly, a [try statement with an except ImportError](https://realpython.com/python-exceptions/) clause can be used to guard against unsuccessful _import_ attempts:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_04.png)


## The **_dir()_** Function.

The built-in function _dir()_ returns a list of defined names in a namespace.
Without arguments, it produces an alphabetically sorted list of names in the current **local symbol table:**


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_05.png)

Note how the first call to _dir()_ above lists several names that are automatically defined and already in the namespace when the interpreter starts. As new names are defined(qux, Bar, x), they appear on subsequent invocations of _dir()._

This can be useful for identifying what exactly has been added to the namespace by an import statement:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_06.png)

When given an argument that is the name of a module, dir() lists the names defined in the module:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_07.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_08.png)


## Executing a Module as a Script

Any _.py_ file that contains a **module** is essentially also a Python **script,** and there isn't any reason it can't be executed like one.

Here again is _mod.py_ as it was defined above.

### mod.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_09.png)

This can be run as a script:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_10.png)

There are no errors, so it apparently worked. Granted, it's not very interesting. As it is written, it only defines objects. It doesn't do anything with them, and it doesn't generate any output.

Let's modify the above Python module so it does generate some output when run as a script.


### mod.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_11.png)


Now it should be a little more interesting:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_12.png)


Unfortunately, now it also generates output when imported as a module:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_13.png)


This is probably not what you want. It isn't  usual for a module to generate output when it is imported.

Wouldn't it be nice if you could distinguish between when the file is loaded as a module and when it is run as a standalone script?

Ask and yet shall receive.

When a _.py_ file is imported as a module, Python sets the special _dunder_ variable (\_\_name__) to the name of the module. However, if a file is run as a standalone script,  (\_\_name__) is (creatively) set to the string  ('\_\_main__'). Using this fact, you can discern which is the case at run-time and alter behavior accordingly:

### mod.py


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_14.png)


Now, if you run as a script, you get output:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_15.png)


But if you import as a module, you don't:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_16.png)


Modules are often designed with the capability to run as a standalone script for purposes of testing the functionality that is contained within the module. This is referred to as [Unit testing](https://realpython.com/python-testing/). For example, suppose you've created a module _fact.py_ containing a **factorial** function, as follows?


## fact.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_17.png)


The file can be treated as a module, and the _fact()_ function imported:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_18.png)


But it can also be run as a standalone by passing an integer argument on the command-line for testing.


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_19.png)


## Reloading a Module

For reasons of efficiency, a module is only loaded once per interpreter session.
That is fine for function and class definitions, which typically make up the bulk of a module's contents. But a module can contain executable statements as well, usually for initialization. Be aware that these statements will only be executed the _first time_ a module is imported.

Consider the following file _mod.py_

## mod.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_20.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_21.png)


The _print()_ statement isn't executed on subsequent imports. (For that matter, neither is the assignment statement, but as the final display of the value of _mod.a_ shows, that doesn't matter. Once the assignment is made, it sticks.)

If you make a change to a module and need to reload it, you need to either restart the interpreter or use a function called _reload()_ from module _importlib_:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_22.png)


## Python Packages.

Suppose you've developed a very large application that includes many modules. As the number of modules grows, it becomes difficult to keep track of them all if they're dumped into one location. This is particularly so if they have similar names or functionality. You might wish for a means of grouping and organizing them.

**Packages** allow for a hierarchical structuring of the module namespace using _dot notation_. In the same way that __modules__ help avoid collisions between global variable names, _packages_ help avoid collisions between module names.

Creating a _package_ is quite straightforward, since it makes use of the operating system's inherent hierarchical file structure. Consider the following arrangement:


>![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_23.png)

Here, there is a **directory named _pkg_** that contains two modules, _mod1.py_ and _mod2.py_. The contents of the modules are:

### mod1.py


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_24.png)


### mod2.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_25.png)


Given this structure, if the _pkg_ directory resides in a location where it can be found (in one of the directories contained in _sys.path_), you can refer to the two **modules** with **dot notation** (pkg.mod1, pkg.mod2) and import them with the syntax you're already familiar with:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_26.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_18.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_19.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_20.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_21.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_22.png)


You can import modules with these statements as well:


![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_23.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_24.png)


You can technically import the package as well:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_25.png)

But this is of little avail. Though this is, strictly speaking, a syntactically correct Python statement, it doesn't do much of anything useful. In particular _it doesn't place_ any of the modules in _pkg_ into the local namespace.

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_26.png)

To actually import the modules or their contents, you need to use one of the forms shown above.


## Package Initialization.


If file named  < \_\_init__.py > is present in a package directory, it is invoked when the package or a module in the package is imported. This can be used for execution of package initialization code, such as initialization of package-level data.

For example, consider the following  <\_\_init__.py> file:


### \_\_init__.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_27.png)

Let's add this file to the _pkg_ directory from the above example:

Now when the package is imported, the global list A is initialized:

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_27.png)

A _module_ in the package can access the global variable by importing it in turn:

### mod1.py

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_packages_28.png)

![](/home/josemacevo/Downloads/markdown_tutorial_images/modules_and_packages_28.png)

\_\_init__.py can also be used to effect automatic importing of modules from a package. For example, earlier you
saw that the statement _import_ pkg only places the name pkg in the caller's local symbol table and doesn't import
any modules. But if \_\_init__ .py in the pkg directory contains the following.

### \_\_init__.py


