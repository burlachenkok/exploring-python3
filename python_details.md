# Technical Note. Some Python3 Details (Draft)

[Konstantin Burlachenko](https://burlachenkok.github.io/), et al.

[King Abdullah University of Science and Technology](https://www.kaust.edu.sa/en), Thuwal, Saudi Arabia.

Correspondence to: konstantin.burlachenko@kaust.edu.sa

Revision Update: July 06, 2023

© 2023 Konstantin Burlachenko, all rights reserved.

----

- [Introduction](#introduction)
- [What is a Python](#what-is-a-python)
- [Source of Confusion](#source-of-confusion)
- [Background: Programming Languages](#background--programming-languages)
  * [How typical compute device is working](#how-typical-compute-device-is-working)
  * [Programming Languages Taxonomy](#programming-languages-taxonomy)
  * [What is Object Orientated Programming (OOP)](#what-is-object-orientated-programming--oop-)
- [Phylosophy of Python](#phylosophy-of-python)
- [How to start Interpreter](#how-to-start-interpreter)
- [Language Constructions and sources of confusion for people with another languages background](#language-constructions-and-sources-of-confusion-for-people-with-another-languages-background)
  * [Comment about events](#comment-about-events)
  * [Meaning of Object](#meaning-of-object)
  * [Python does provide access to variables by value](#python-does-provide-access-to-variables-by-value)
- [Context in Python is the same as scopes (in C++)](#context-in-python-is-the-same-as-scopes--in-c---)
  * [Pointers and Object Reference Equality](#pointers-and-object-reference-equality)
  * [All class methods are virtual (in terms of C++)](#all-class-methods-are-virtual--in-terms-of-c---)
- [Interfaces and Protocols](#interfaces-and-protocols)
- [What is a False statement ?](#what-is-a-false-statement--)
- [Possible Python benefits which are absent in most Languages](#possible-python-benefits-which-are-absent-in-most-languages)
- [About classes inside Python](#about-classes-inside-python)
- [Key differences with C++](#key-differences-with-c--)
- [Python Technical Basics](#python-technical-basics)
  * [Source file organization](#source-file-organization)
  * [Source file encoding](#source-file-encoding)
  * [Logical Lines and Parsing](#logical-lines-and-parsing)
  * [Comments](#comments)
- [Operator Precedence](#operator-precedence)
- [Simple built-in types](#simple-built-in-types)
  * [Simple Statements](#simple-statements)
  * [Compound Statements](#compound-statements)
  * [Empty(Pass) Statements](#empty-pass--statements)
  * [Division of numbers in Python](#division-of-numbers-in-python)
  * [Comparision of Containers](#comparision-of-containers)
  * [Strings](#strings)
  * [Printing](#printing)
- [Enumeration and Loops](#enumeration-and-loops)
- [More on Conditions](#more-on-conditions)
- [Python Technical Details. Middle](#python-technical-details-middle)
  * [Convention about Variable Names](#convention-about-variable-names)
  * [Inspect Python Objects](#inspect-python-objects)
  * [Variables Introspection](#variables-introspection)
  * [Global and Nonlocal Variables](#global-and-nonlocal-variables)
  * [Working with Tuples](#working-with-tuples)
  * [Modules and Packages](#modules-and-packages)
    + [Modules](#modules)
    + [Packages](#packages)
    + [Reference between modules and packages](#reference-between-modules-and-packages)
  * [Rules for Search Modules and Packages](#rules-for-search-modules-and-packages)
  * [Comprehensions syntax](#comprehensions-syntax)
  * [Random Interesting Construction](#random-interesting-construction)
- [Technical Details about Functions](#technical-details-about-functions)
  * [About Indentation](#about-indentation)
  * [Arguments of the function and return value](#arguments-of-the-function-and-return-value)
  * [Default Value of Argument for a function](#default-value-of-argument-for-a-function)
  * [Keyword and Positional Arguments](#keyword-and-positional-arguments)
  * [Syntax to split positional and keyword](#syntax-to-split-positional-and-keyword)
  * [Varying Number of Arguments](#varying-number-of-arguments)
  * [Small anonymous functions can be created with the lambda keyword](#small-anonymous-functions-can-be-created-with-the-lambda-keyword)
  * [Function and Type Annotation](#function-and-type-annotation)
- [Function Decorators](#function-decorators)
- [Classes in Python](#classes-in-python)
- [Python Technical Details. Advanced.](#python-technical-details-advanced)
  * [Special or Magic method for classes](#special-or-magic-method-for-classes)
  * [Module Reloading](#module-reloading)
  * [Encoding for Files](#encoding-for-files)
  * [Defaultdict](#defaultdict)
  * [Match Statement](#match-statement)
- [Walrus](#walrus)
  * [Generators](#generators)
- [Conda/Pip/Venv package and environment managers](#conda-pip-venv-package-and-environment-managers)
  * [Package managers](#package-managers)
  * [Environment managers](#environment-managers)
- [Python Notebooks](#python-notebooks)
  * [Working in a web-based interface.](#working-in-a-web-based-interface)
  * [PyTorch resources](#pytorch-resources)
- [Various code snippets](#various-code-snippets)
  * [Numpy](#numpy)
    + [Ellipsis in NumPy](#ellipsis-in-numpy)
    + [Newaxis in Numpy](#newaxis-in-numpy)
  * [Matplotlib](#matplotlib)
    + [Show the image with Matplotlib](#show-the-image-with-matplotlib)
    + [Show a one-dimensional plot (simple)](#show-a-one-dimensional-plot--simple-)
    + [Show sub-images](#show-sub-images)
- [Cython](#cython)
  * [How to optimize Python Code with Cython](#how-to-optimize-python-code-with-cython)
  * [About Cython Language](#about-cython-language)
  * [Easy Interoperability with Standar C Library](#easy-interoperability-with-standar-c-library)
    + [Example of function Integration in Cython and Python](#example-of-function-integration-in-cython-and-python)
- [References](#references)
- [Introduction document](#introduction-document)
- [Reference official materials](#reference-official-materials)
- [Mapping concepts from other languages/libraries to Python language/libraries](#mapping-concepts-from-other-languages-libraries-to-python-language-libraries)
- [Tutorial for Libraries](#tutorial-for-libraries)
- [Howto](#howto)
- [Repositories](#repositories)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


# Introduction

The web search engine Google returns around $6.95 \cdot 10^8$ answers from the search request "Python Tutorial". One of these tutorials is worthwhile to mention explicitly - it is a tutorial on the subject of this programming language originally written by *Guido Van Rossum* - https://docs.python.org/3/tutorial/index.html, which is located on the website of the most popular python distribution [CPython](https://www.python.org/). Another Python distributions [Jython](https://www.jython.org/), [Python for .NET](https://pythonnet.github.io/) are less popular.

Python is a popular scripting language which can be observed from:
* [Tiobe-Index](https://www.tiobe.com/tiobe-index/)
* [Google-Trends](https://trends.google.com/trends/explore?date=now%201-d&q=%2Fm%2F05z1_,%2Fm%2F0jgqg,C%23,%2Fm%2F07sbkfb,BASH&hl=ru)

On this note authors with some mix of backgrounds, but primarily CS
background wants to achieve several goals:

* Share some technical details about some language features. It can be good material after reading [Official Python Tutorial](https://docs.python.org/3/tutorial/index.htm), if you do not have to get all important from the [Official Python Tutorial](https://docs.python.org/3/tutorial/index.htm).
* Share vision which system aware people CS/EE people think about the subject
* Provide a compact survey about several tools

We hope to make it will be an interesting reading for you. So let's go.

# What is a Python

[Python](https://www.python.org/) is an interpretable scripting language. Python has been designed originally only as a replacement for Bash, which has been described in that [Blog Post](https://l.facebook.com/l.php?u=https%3A%2F%2Fpython-history.blogspot.com%2F2009%2F01%2Fpersonal-history-part-1-cwi.html%3Ffbclid%3DIwAR1v3C4KHiJtBbG4NYVY2o__lMchCNVKQGe2ozoI-gcxnwCYNvcdxzD_sHU&h=AT1quzeQEvwmfgFXMnWscdzCzWIJrbgoyQKX22c6w2yzVSaUt9LBMdrL66UgpJaz3rh_-BLBa8FVu3sdV_NzuiuSTU4XPZ5zADu4wGoASMxLcRR-n7Emwogq664lszQUbTZM&__tn__=-UK-R&c[0]=AT3TC-zKWleGu9UDUQg6mUKEWZ-El56OnANy8jfnUXLhGPAIHIfrXp6ZVEhtbJztlbUu_3OhD9sRJ7JA_F3ETiL3BsR0dKi58KfhLRwPsHtyRauqYQXDGtxnIeFWRyAxyop0WlHBapKPdoYnVar9DUy3pudNCdWdZ1c4wlxvNA3qoA) written by original author:

> "...My original motivation for creating Python was the perceived need for a higher-level language in the Amoeba project. I realized that the development of system administration utilities in C was taking too long. Moreover, doing these in the Bourne shell wouldn't work for a variety of reasons. The most important one was that as a distributed micro-kernel system with a radically new design, Amoeba's primitive operations were very different (and finer-grain) than the traditional primitive operations available in the Bourne shell. So there was a need for a language that would "bridge the gap between C and the shell..." - [Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum).

This language does not have any Standards. The only available language standard is Language reference: https://docs.python.org/3/reference/.


# Source of Confusion

Python is a useful and nice *scripting language*, which of course more comfortable to use compared to [Perl](https://www.perl.org/books/beginning-perl/) or [Windows Batch](https://en.wikibooks.org/wiki/Windows_Batch_Scripting) and sometimes more comfortable to use compare to [Bash](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html). In our experience [Bash](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html) is still more easy to use if the script contains executing external processes for different tasks and less logic.

Python language has a very fast learning curve which opened the door fortunately for people not only in CS to create useful scripts in day-to-day life.

> Python is a programming language which...

Unfortunately, if you are around people with CS/EE/System/Compilers backgrounds it may be the case that these people will make a statement: **"Python is not a programming language"**. 

To state that Python is *Programming Language* we need to define what is (1) programming and what is a (2) language. 

One vague (and market) meaning is that Language expresses ideas and you don't care at all how these ideas are materialized.

But it's not the only definition. The message which one scientist [B. Stroustroup](https://scholar.google.com/citations?user=Rr9Y8acAAAAJ&hl=ru&oi=ao) (author of C++) tries to bring people at least for already 3 decades that *Programming Language* is the language that gives you a way to describe the algorithm and this algorithm will be executed in the computer (compute device). With this very strict definition [Python](https://www.python.org/), [Java](https://www.java.com/), [C#](https://learn.microsoft.com/en-us/dotnet/csharp/) what is executed is not your program. It's an interpreter in the case of Python and runtime coupled with a Just-In-Time(JIT) compiler for another two languages. If you only start with Programming or if you have no experience in Compilers/OS/Systems you may not see the difference, but their difference is fundamental even to catch this difference you should have some background. 

It does not say that this language is incorrect, but I hope you should understand that it's not the computer that executes this program, but what is executing is another (one more) level of abstraction.

**"Python is a general purpose programming language"**.

This is a very strong statement. And it can be three points of view on such a statement with anybody who has CS background:

1. By general-purpose programming language you mean that you can create any Algorithm. If this is a definition, then it's correct. Only DSL languages constructed for special purposes maybe have a lack of being [Turing Complete](https://en.wikipedia.org/wiki/Turing_completeness).

2. If by General Purpose you mean that you can use it across a big amount of domains, then how big is big? This is a pretty vague definition.

3. If by General Purpose you mean you can create programs for different computing elements in the Computer, then it's almost *wrong*. You should have big flexibility with what you're doing. Second Python by design is the replacement for Bash, it's not a replacement for C or C++, or any other traditional language. You can not create drivers for your devices.

**"Python is more elegant and short than C++"**.

This is true if look into lines of code, however the first thing in creating algorithms - they should be correct. In our experience, what is interesting after some amount of code there is a very strange asymmetry that you will observe once you will create projects in Python and C++ with 40K lines of code and more.

With such a big size (lower bound) Python is not even close to C++. The thing is that the absence of compilers and toolchains which you need to install and spend time using compile, just-in-time compile languages will hurt you. There are very well-developed tools.

Of course, there is a tool for Python that helps [pylint](https://pypi.org/project/pylint/), but a compiler and linker are far more powerful tools for detecting errors than a static analyzer.

**"Python is everywhere"**.

This overstatement can also be read from Python Tutorial. Please be aware that (any) interpretable language which exists or will be created in the future fundamentally will have the following downsides:
[C++ Technical Note / Downsides of Interpretable Languages
](https://github.com/burlachenkok/CPP_from_1998_to_2020/blob/main/Cpp-Technical-Note.md#downsides-of-interpretable-languages) fundamentally.

# Background: Programming Languages

## How typical compute device is working

It's important to understand that computational devices do not execute code in Java, C#, Matlab, or Python.
Computers can not do it. It is what happening in reality.

The real computation devices execute binary code compiled in the form of Instruction Set Architecture(ISA). A simplified compute CPU device has computation cores that execute arithmetic operations via reading arguments and writing results into some form of memory. Memory allows to store input and output results.
Control units that control the execution of these low-level commands. 

Finer details about how computers are working can be obtained from *System Architecture*, *Performace Engineering* courses and books.

## Programming Languages Taxonomy

There are a lot of programming languages these days. One way to analyze programming languages is from the angle of the implemented type system:

* **Static Type System** - statically typed system languages are those in which type checking is done at compile time.
* **Dynamic Type System** - dynamically typed languages are those in which type checking is done at runtime (execution time).
* **Strong Type System** or **Strong Type safety** - implicit type casting is prohibited
* **Weak Type System** or **Weak Type safety** - implicit type casting is allowed.

C and C++ have a *Strong Static Type System*.

Examples of Languages with *Weak Type System* are Javascript and Perl.

Ruby and Python have a *Strong Dynamic Type System*. This means that types are inferred at runtime (dynamic type system), but implicit conversions between types are not allowed (strong type system).

## What is Object Orientated Programming (OOP)

Please take a good course or read good books. Object-Orientated-Programming requires some special way to organize code, but it does not force to have a `class` keyword in Language.

Please check this Appendix if you're curious about what is OOP from a Computer Science point of view: [C++ Technical Note/Object Orientated Design
](https://github.com/burlachenkok/CPP_from_1998_to_2020/blob/main/Cpp-Technical-Note.md#object-orientated-design).

# Phylosophy of Python

If you want to understand some hidden principles built-in Python language then read from the following command:
```bash
python -c "import this"
```
This provides a point of view of the world from the point of view of Python language.

# How to start Interpreter

Based on [1] the Python interpreter operates somewhat like the Unix shell. In general, there are three ways to start the execution of a Python interpreter:

1. When it is called with standard input connected to a device that can emit symbols - it reads and executes commands interactively. 

2. When a Python interpreter is called with a file name argument or with a file as standard input, it reads and executes a script from that file. For details see [Python Tutorial / Interpreter](https://docs.python.org/3.13/tutorial/interpreter.html)

3. Next way of starting the interpreter is by calling
    ```bash
    python -c command [arg] ...
    ```
    It executes the statement(s) in the command, analogous to the shell s `-c` option. Since Python statements often contain spaces or other characters that are special to the shell, it is usually advised to quote the command in its entirety.

4. Some Python modules are also useful as scripts. These can be invoked using `python -m module [arg] ...`, which executes the source file for the module as if you had spelled out its full name on the command line.


The script name and additional arguments are turned into a list of strings and assigned to the `sys.argv` variable. 

* When no script and no arguments are given, `sys.argv[0]` is an empty string. 

* When the script name is given as `-` it means standard input, and `sys.argv[0]` is set to '-'. 

* When you execute a Python interpreter with the `-c` command used, sys.argv[0] is set to `-c`:
  ```bash 
  python -c "import sys; print(sys.argv)"
  ```

*  When the `-m` module is used, `sys.argv[0]` is set to the full name of the located module.

# Language Constructions and sources of confusion for people with another languages background

## Comment about events
Some programming languages like C# contain native support of event-based communication between objects to support Object Orientated Programming, however, Python (and C, C++) programming languages do not contain a native event-based system, even though there are frameworks on top of it that supported that (for example Qt). 

## Meaning of Object

Now there is a clash of terminology if you have C++, Java, or C# background. In Python, everything that takes up memory in some form is called the **object**. In other languages (Java, C#, C++) object is an instance of the class. So an object in Python terminology is:
* Class instances 
* Exotic built-in objects (e.g. files)
* Fundamental built-in data types (e.g. integers)

It's not true that all things are classes in Python.

## Python does provide access to variables by value

In Python, there are only references to objects. Python has named references to objects. It has no variables available to the programmer for reference by value. Functions return and accept arguments by reference in the terminology of C++, C#, and Java.

Moreover, there is a very strange thing, which was in Python 2.* and still is in the language about default argument. For the default argument - the default value itself is passed by reference and *calculated once*. This (unfortunately) creates an implicit global default value object. For C++ this is not observed, because arguments are passed by value, in the usual way of writing arguments, and is just a shorthand.

# Context in Python is the same as scopes (in C++)

The context in Python is a concept similar to C++'s scope. However, in Python, you can not create scopes inside the function. For example if inside the function you have a nested loop, then its level of nested loop **does not** introduce a new scope, as a consequence the indexing variable will be rewritten.

In fact, in Python, there are only 4 contexts (scopes):

* **Local Context/Scope.** The context inside the current function. Importantly, a new loop with a new indentation does not introduce a new local scope.

* **Enclosing Context/Scope.** Locally declare a function inside a function. Variables from the parent function will be implicitly accessible to the children.

* **Global Context/Scope.** Variables from the top level of the module.

* **Built-in Context/Scope.** Built-in variables and functions built-in into the interpreter.

## Pointers and Object Reference Equality

> Object in Python is everything that takes memory. Fundamentally in Python Object equality can test in one of two types:

> **Value equality** - testing which takes a reference to objects. And what is tested the objects have the same content. The programmer can define what this means via defining `__eq__` in your class. (This operator is the same as `==` in C++)

> **Identity equality** - testing when two references are referenced to the same object. The programmer can not determine what it means, it is defined by the language. Such type of equality is used during the use of operators `is`, `is not` or built-in function `id()`.

In Python 2/3 there are no pointers. However, there is a built-in function called `id`. In fact `id(obj)` according to [1],[2] `id` means the address of an object in the interpreter's memory. In principle that two objects refer to the same memory can be checked in the following way:
```python
id(x) == id(y)
```

However in reality you will not meet this code too much. If you need to compare references to an object, then this action is typically performed by using the operators:
* `is`
* `is not

They have the same semantical meaning as using `id()`.

> If you C++ background. The fact that id(x) is the memory address of object "x" is a CPython implementation detail. CPython can change this in the future. It's also hard to say what is standard and what is not - there is no standard for Language.


## All class methods are virtual (in terms of C++)

Without loss of generality, we can assume that in Python all methods of all classes are `virtual`. But actually, there are no virtual methods and virtual keyword and virtual tables in Python at all mentioned in [1] or [2]. 

In fact, in Python methods are attributes. They are bound at runtime and executed dynamically. In very specific circumstances you can manually remove, get, and set attributes:

* [delattr()](https://docs.python.org/3/library/functions.html?highlight=getattr#delattr) - remove an attribute

* [hasattr()](https://docs.python.org/3/library/functions.html?highlight=getattr#hasattr) - checks if an attribute exist.

* [setattr()](https://docs.python.org/3/library/functions.html?highlight=getattr#setattr) - set the value of an attribute

* [getattr()](https://docs.python.org/3/library/functions.html?highlight=getattr#getattr) - get the value of an attribute

There is also no division of fields into public/private/protected as it is in C++ and there are no different types of inheritances. What is absent in Python is a limited type of support for `private` names, which we will describe later.


# Interfaces and Protocols

In Python languages, you will not find the keyword `interface` such as in Java/C# or pure virtual class methods as in C++.

In contrast, Python does not have interfaces as a language concept. 

Instead of a specific (and robust) interface Python and other scripting languages uses what is known as Duck Typing.

This concept is described typically in the following way:

*"If it walks like a duck and it quacks like a duck, then it must be a duck"*.

In Python, any object may be used in any context until it is used in a way that it does not support. The [AttributeError](
https://docs.python.org/3/library/exceptions.html#AttributeError) will raise.

If your class is in Python and you want the objects of your class can be used in specific language construction (like) iteration you should support **protocol**. The protocol may not be a formal name, however, during the conversation you will hear about it:

* **Container protocol.** Built-in container types in Python
tuple, list, str, dict, set, range, and bytes all support `in`, and `not in` operations for them. Support of this is named container protocol. For your types, you should define 
  ```python
  def __contains__ (self, item)
  ```

* **Sized protocol.** Built-in container types in Python support `len(x)` operator. For your types, you should define:

  ```python
  def __len__(self)
  ```

- **Iterable protocol.** Built-in container types in Python support the [iter(x)](https://docs.python.org/3/library/functions.html#iter), [next(x)](https://docs.python.org/3/library/functions.html#next) operators. 

  Such objects can be used in for-loops:
  ```python
  for I in iterable: smth(i)
  ```
  For your types you should define:
    ```python
    def __iter__(self) 
    def __next__(self)
    ```

  More details:  https://docs.python.org/3/tutorial/classes.html#iterators

- **Sequence protocol.** Except dict, all Built-in container types in Python support indexing. It's known as sequence protocol. For your types, you should define [__getitem__](https://docs.python.org/3/reference/datamodel.html?highlight=__getitem__#object.__getitem__), [__setitem__](https://docs.python.org/3/reference/datamodel.html#object.__setitem__), [__delitem__](https://docs.python.org/3/reference/datamodel.html?highlight=__getitem__#object.__delitem__). Once you will define this operator you can call:

  ```python
  x [integral_index] 
  x.index(someValue) 
  x.count(someValue) 
  produceReverseSeq = reversed(x)
  ```

# What is a False statement ?

The following expressions are considered as `false` in Python:
* None
* 0
* Empty sequence. I.e. sequence which `len() == 0`


# Possible Python benefits which are absent in most Languages

It very depends on your point of view and your style, but there is a point of view where the following things are benefits. Especially if you have limited time for a project:
  1. More correct code from a style point of view
  2. Automatic cross-platform serialization "pickling" for the user and built-in types from the Python standard library.
  3. A lot of free and commercial IDE with intellisense.
  4. Python has a built-in debugger. 
  The parsing of the function is performed only at the moment of the direct call. To run the script with a debugger call `python -m pdb scriptname.py`. After this, you will have the ability to insert text commands in the interactive shell with [pdb commands](https://docs.python.org/3/library/pdb.html#debugger-commands).
  5. Python use <`.`> token to separate packages/subpackage/class like C# and Java. Packages from Perl are called modules in Python.


# About classes inside Python

Python has limited support for private attribute objects when naming attributes as `__attributename`. In this case, the interpreter performs name mangling. In the end, this attribute will be `_classname__attribute`.

In C++ terminology Python classes have the following characteristics:
  1. Normal class members (including the data members) are public except for some small support for  Private Variables.
  2. All member functions/methods are virtual.
  3. Like in C ++, most built-in operators with special syntax (arithmetic operators, etc.) can be redefined for class instances.
  4. Python Data attributes correspond to "data members" in C ++.
  5. Python supports multiple inheritance, and exceptions.
  6. In Python (and in Perl) there is not exist such term as function overloading.

# Key differences with C++

* Assignments do not copy data - the language just binds names to objects. 
* In Python class data attributes override method attributes with the same name. To avoid accidental name conflicts, which may cause hard-to-find bugs in large programs, it is wise to use some kind of convention that minimizes the chance of conflicts.
* Nothing in Python makes it possible to enforce data hiding - it is all based upon convention.
* In Python, when using logical connectives, the result of a compound expression is equal to the result of the last subexpression evaluated. In C and C++ the result of a Boolean expression with short-circuiting is always an integer 0 or 1:

  ```cpp
  #include <iostream>
  int main() {
      std::cout << (12 || 2);
      return 0;
  }
  // Out: 1
  ```

  ```python
  #!/usr/bin/env python3
  print(12 or 2)
  # Out: 12
  ```
* Python has the `elif` keyword. For Python, it's crucial because their syntax rules require creating indentation between `else` and `if`. C++ Language is more flexible, and such a keyword is absent in C/C++ but is presented in C Language Preprocessor. Example:

  ```python
  if x < 0:
      pass
  elif x == 0:
      pass
  else:
      pass
  ```

* Python has a break statement in the else branch. If you did not read Python Tutorial [1] it can be the case that you never heard about it. In programming languages and fact in scripting languages (such as Python) such a concept is absent.
The break statement, like in C/C++, breaks out of the innermost enclosing iteration loop. Loop statements may have an `else` path. This logic is executed when the loop terminates through exhaustion of the iterable or when the condition becomes false. See Also: https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements-and-else-clauses-on-loops

* In Python there is the "Exotic" operator `**` used. This operator can raise integers, real, and complex numbers to specific power. Such a built-in operator is absent in C++.


# Python Technical Basics

In the terminology of Programming Languages, **tokens** are separate words of a program text. One easy case is when such words (tokens) are split between each other by spaces. In most programming languages, the tokens fundamentally can be one of the following types:

* Operators
* Separators
* Identifiers
* Keywords
* Literal constants

## Source file organization

The line `#!/usr/bin/env python3` in well-developed scripts is presented and it's called sha-bang. It has a long history for Unix/Linux OS-es. For Windows, it's possible to use it as well. 

A binary named `py` is a launcher with performs a choice of a used interpreter if using sha-bang under Windows.

## Source file encoding

The source file is represented in characters. Characters can be in any supported encoding https://docs.python.org/3.13/library/codecs.html#standard-encodings. By default, Python source files are treated to be encoded in UTF-8. 

To declare an encoding other than the default one, a special comment line should be added as the first line of the file or a second line in the file if the first line is sha-bang.Example:
```python
#!/usr/bin/env python3
# -*- coding: cp1252 -*-
```

## Logical Lines and Parsing

Physical lines in source code are physical different lines in the text inside the file. 

A Python program is read by a parser. The physical lines in the source text are not necessarily equivalent to the logical lines of the source program. A logical line of a program is constructed from one or more physical lines by following the explicit or implicit line-joining rules:

* When a physical line ends in a backslash `\` character that is not part of a string literal or comment, it is joined with the following forming a single logical line, deleting the backslash and the following end-of-line character.

* A physical and logical line that contains only spaces, tabs, and possibly a comment is ignored by the parser.

* Expressions in parentheses, square brackets, and curly braces can be split over more than one physical line without using the backslash symbol `\`. 

The parsing of Python source code (e.g. of the function) is performed only at the moment of the direct call of this function.

## Comments

Comments in Python start with the hash character `#` and extend to the end of the physical line.

# Operator Precedence

Information about operator precedence is available here:
https://docs.python.org/3.13/reference/expressions.html#operator-precedence

# Simple built-in types

* **Ellipsis.** This type has a single value namely There is a single object with this value. This object is accessed through the literal `...`. If use this expression in condition then `...` is implicitly converted into `True`.

* **NoneType.** This type has a single value and there is a single object with this value. This object is accessed through the built-in name `None`. It is used to signify the absence of a value in many situations. Also, it is returned from functions that do not explicitly return anything. 
 If use this expression in condition then `...` is implicitly converted into `False`.
 
* **Integers (int)**. The type which is used to represent integers in Python does not have any fixed number of bits. Instead, it has a varying size. In this sense, an integer has an unlimited range. Practically you can hold as big an integer as you want until you start having problems with virtual memory.

* **numbers.Real (float).** These represent machine-level double-precision floating point numbers. From Documentation: *"...there is no reason to complicate the language with two kinds of floating point numbers..."* It's the design choice of a language. Of course, for people involved in numerics, such a statement is deeply wrong and sometimes laughable only. Documentaion: https://docs.python.org/3.13/reference/datamodel.html


## Simple Statements

A simple statement is comprised of a single logical line. Several simple statements may occur on a single line separated by semicolons. Example:
```python
a=1;b=2;
```

See Also: https://docs.python.org/2/reference/simple_stmts.html

## Compound Statements

Generally, compound statements are written on multiple logical lines using indentation, although sometimes if a compound statement has only one branch in it, and the body of it contains only simple statements, it can be written on a single logical line. Example:

```python
if 1: print (2); print (2); #legal python code
```

## Empty(Pass) Statements
The `pass` statement does nothing similar to C++ `;` statement.

## Division of numbers in Python
The division operator in Python `/` always returns a float. To do floor division and get an integer result you can use the `//` operator. 

To calculate the remainder you can use `%` similar to C/C++/Java.

## Comparision of Containers

Sequence objects (typically) can be compared to other objects with the same sequence type. The comparison uses lexicographical ordering.

## Strings
Python can also manipulate strings, which can be expressed in several ways. They can be enclosed in single quotes ('...') or double quotes ("...").

## Printing

The standard way to print something in Python 3 is by utilizing the built-in [print()](https://docs.python.org/3/library/functions.html#print) function. There is a style for printing with a C# style string format:
```python
print("Hello {1} / {0}".format(12,14))
```

The default behavior is to output the line into `stdout` with a new line. If you don't want to output a new line then in Python 3 you can specify the end character in the following way:
```python
print("hello",end='!')
```

If you want to print not to `stdout`, but to `stderr` it can be attained in the following way:
```python
import sys
print("hello {0}".format("world"), end='\n',file=sys.stderr)
```

You can use various formatting rules described here to configure output:
https://docs.python.org/3.11/library/string.html#formatspec

Since Python 3.6 is in the interpreter, a string interpolation feature has been added (https://www.python.org/dev/peps/pep-0498/). It is presented in Bash and similar scripting languages.
Example:
```python
a = 123
print (f"Hello {a}")
```

The f-string forms what is known as a formatted string literal. To use formatted string literals, you should begin a string with `for `F` before the opening quotation mark.

Inside this string, you can write a Python expression between `{` and `}`. For example, you can refer to variables or literal values. During using f-string you can pass an integer after the `:`. It will cause that field to be a minimum number of characters wide. This is useful for making columns line up. Example:
```python
a = 123
print (f"Hello {a:10}")
```

Next, there is one extra feature. The `=` specifier can be used to expand an expression to:
* Text of the expression in text form
* An equal sign
* Representation of the evaluated expression

Example:
```python
a = 123
print(f"{a=}")
```

String interpolation (named formatted string literal) is evaluated during script execution. In terms of type, they are just `str` and it's not a new type. Implementing in C++ or any compiling language such functionality is impossible (of course if you don't want to write your interpreter inside your project).

Finally, old string formatting from Python 2 which is still supported in Python 3 for C style `printf()` C++ specification. Example:
```python
print("%i %.2f" % (1, 0.12345))
```

# Enumeration and Loops

Python's `for` statement iterates over the items of any sequence. If you do need to iterate over a sequence of numbers, the built-in function [range()](https://docs.python.org/3/library/stdtypes.html?highlight=range#range) function will help you with that:
```python
for i in range(5):
    print(i)
```

The object returned by `range()` behaves as if it is a list, but it is not. It is an object of class `range` which returns the successive items of the desired sequence when you iterate over it. It does not make the list, thus saving space.

When you are looping through dictionaries you are enumerating through `keys`. If the key and corresponding value are important for you, then can retrieve the value and key at the same time.  For doing it you should use the [items()](https://docs.python.org/3/library/stdtypes.html?highlight=items#dict.items) method in the dictionary built-in type.

Next, when you are looping through a sequence, the position index and corresponding value can be retrieved at the same time using the `enumerate()` function. Example:

```python
for  index, value in enumerate([10,11,23]):
  print(index, value)
```

To loop over a sequence in reverse, first, specify the sequence in a forward direction and then call the `reversed()` function. To loop over a sequence in sorted order, use the `sorted()` function which returns a new sorted list while leaving the source unaltered. Using `set()` on a sequence eliminates duplicate elements because it creates the set.

# More on Conditions

The conditions used in `while` and `if` statements can contain any operators.

* The comparison operators `in` and `not in` are membership tests.
* The operators `is` and `is not` compare whether two objects are the same object in memory.

What can confuse people with C++/Java/C#/C backgrounds is that comparisons can be chained. For example: `a < b == c` tests the following *"a is less than b **and** b equals c"*. Comparisons may be combined using the Boolean operators `and` and `or`, `not`. The Boolean operators `and` and `or` are so-called short-circuit operators and are analogous to `&&` and `||`. Python supports the same set of the bitwise operator as it in C/C++ languages (See [operator precedence](https://docs.python.org/3.13/reference/expressions.html#operator-precedence)) and it inludes: `&`, `|`, `<<`, `>>`, `~`.

In Python when you use it as a general expression (not necessarily Boolean), the return value of a short-circuit operator is the last evaluated expression (which is not the case in C++ where you don't obtain this information).

# Python Technical Details. Middle

## Convention about Variable Names

| **Convention**      | **Example** | **Meaning** |
|---------------|------------|------------------------|
| Single Leading Underscore  | `_var` |  This naming convention indicates a name is meant for internal use. Generally not enforced by the Python interpreter (except in wildcard imports) and meant as a hint to the programmer only. During import using the wildcard symbol (`from my_module import *`), these symbols will not be imported in the global namespace.|
| Single Trailing Underscore     | `var_`         | Used by convention to avoid naming conflicts with Python keywords.          |
| Double Leading Underscore      | `__var`           | Triggers name mangling when used in a class context. Enforced by the Python interpreter to make name mangling                      |
| Double Leading and Trailing Underscore | `__var__`        | Indicates special methods defined by the Python language. Avoid this naming scheme for your attributes. |
| Single Underscore         | `_`          | Sometimes used as a name for temporary or insignificant variables ("don't care"). Also, it is the result of the last expression in a Python REPL (interactive Python mode). |

## Inspect Python Objects
As we have already mentioned In Python, everything is an object. 
The [dir(obj)](https://docs.python.org/3/library/functions.html#dir) built-in function displays the attributes of an object.  Attributes that all objects (typically) have:
* `__name__` - is the name of the object such as a function.
* `__doc__` - documentation string for the object.
* `__class__` - type name of the object.

Example:
```python
a = 1
print(a.__class__)
```

## Variables Introspection

* [globals()](https://docs.python.org/3/library/functions.html#globals) - will give you a dictionary of global variables
* [locals()](https://docs.python.org/3/library/functions.html#locals) and [vars()](https://docs.python.org/3/library/functions.html#vars) - will give you a dictionary of local variables
* [dir()](https://docs.python.org/3/library/functions.html#dir) - will give you the list of in-scope variables which includes locals, and enclosed variables from another function if you use the style in which you define a function inside the function.


Example:
```python
#!/usr/bin/env python3

my_global = 1

def f():
  my_enclosing = 2

  def g():
    nonlocal my_enclosing
    global g

    my_local = my_enclosing

    print("locals:", locals())
    print("globals:", globals())
    print("in-scope:", dir())
  g()

f()

# OUTPUT
#   locals: {'my_local': 2, 'my_enclosing': 2}
#   globals: {'__name__': '__main__', ..., 'my_global': 1, ...}
#   in-scope: ['my_enclosing', 'my_local']
```

There are several different contexts/scopes in Python as has been described in [Context in Python](#context-in-python-is-the-same-as-scopes-in-c) Section.

## Global and Nonlocal Variables

In example [Variables Introspection](#variables-introspection) we have used [nonlocal](https://docs.python.org/3/reference/simple_stmts.html#nonlocal), and [global](https://docs.python.org/3/reference/simple_stmts.html#global) keywords. You should use this variable access modifiers when you're going to write to the variable and provide an interpreter and hint about what you are going to do exactly:

* You want to define a new local variable and you provide the default value with the `=` operator
* You do not want to create a local variable, but instead, you want to write to a global variable or variable from a previous(nested) context.


**global.** The global statement is a declaration that holds for the entire current code block. It means that the listed identifiers are to be interpreted as globals. Global variable scope changed immediately to the module-level binding without any intermediate outer scope, relative to the current scope. Also, it is impossible to assign value to a global variable without using `global`. Although you can refer to global variables if you want to read from them.

**nonlocal.**  The nonlocal statement causes the listed identifiers to refer to previously bound variables in the nearest enclosing scope excluding globals. So nonlocal variables scope changed the scope to the outer function. This is important because the default behavior for binding is to search the local namespace first. Names listed in a nonlocal statement must not collide with pre-existing bindings in the local scope.

In C++/C#/Java, such nonlocal scope is impossible because in these languages you can not define a function inside another function. (except lambda)

## Working with Tuples

A tuple consists of several values separated by commas. It is not possible to assign to the individual items of a tuple, however, it is possible to create tuples that contain mutable objects, such as lists. Tuples are immutable and usually contain a heterogeneous sequence of elements that are accessed via unpacking.

A tuple with one item is constructed by following a value with a comma (it is not sufficient to enclose a single value in parentheses). And so to create a tuple with one element is possible only with this required syntactic trick. Example:
```python
a = (12,)  
```

Empty tuples are constructed by an empty pair of parentheses. Example:
```python
 a = ()
 ```
 
You can create a tuple from the list:
 ```python
 ([1,2,23])
 ```

If you want to include tuples in another tuple you need to use parentheses `()`, so that nested tuples are interpreted correctly. Example of creating a tuple with 2 elements:
 ```python
 a=(1,(2,3,4,5,6))
 ```


Tuple packing, unpacking is an interesting idea at the language level.
https://docs.python.org/3/tutorial/controlflow.html#unpacking-argument-lists


## Modules and Packages

### Modules
A module is a file containing Python definitions and statements. The file name is the module name with the suffix `.py`. Within a module, the module name (as a string) is available as the value of the global variable `__name__`. 

To import the module the following instruction should be used:
```python
 import my_module
```

This does not add the names of the functions defined in `my_module` directly to the current namespace. Each module has its own private namespace, which is used as the global namespace by all functions defined in the module. With this form of import statement all global symbols defined in `my_module` are available through `my_module.<function|variable name>`.

Next, there is a variant of the import statement that imports names from a module directly into the importing modules namespace:
`from my_module import func_f, func_g`
 
This statement does not introduce the module name in the global namespace of the current module.

The next variant is to import all names that a module defines `from my_module import *`, except variables and functions whose names are started with an underscore.

A module can contain function definitions.

But also it can contain executable statements. These statements are intended to initialize the module. They are executed only the first time the module name is encountered in an import statement.

It is typically, but it is not required by interpreter design to place all import statements at the beginning of a module.

If the module name is followed by [as](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement), then the name following [as](https://docs.python.org/3/reference/simple_stmts.html#the-import-statement) is bound directly to the imported module. Example:

```python
import my_module as my
```

To inspect the name defined in module `my` you should use the built-in function `dir()` e.g. in the following way `dir(my)`. It is used to find out which names a module defines. 
 



In some sense, Python module files are files written with Python source code. When the Python module is executed as a script.

There are only two differences from code execution if comparing the launching script of the module and importing the module.

1. During launching the module the code in the module will be executed, just as if you imported it, but with the __name__ set to "__main__".  The part of the code for executing the script in the module typically has the following condition:
    ```python
    if __name__ == "__main__":
        pass
    ```
  It provides means to distinguish behavior when the script is used for execution logic, rather than be a facade interface to some functionality.

2. When you launch Python scripts if all is OK in OS and you have enough privileges inside OS then the script will be launched and executed. However, the `import` statement (by default) is executed only once per Python session. See Also [Module Reloading](#module-reloading).

### Packages
Packages are a way of structuring Python's module namespace by using "dotted module names". Firstly you should have source code files with modules. To group these modules into the package you should place modules in one directory and create file `__init__.py` in this directory.

The `__init__.py` files are required to make Python treat directories containing the file as packages. This prevents directories with a common name, such as string, from unintentionally hiding valid modules that occur later on the module search path. In the simplest case, `__init__.py` can just be an empty file, but it can also execute the initialization code for the package or set the `__all__` variable.

If execute code:
```python
from package_name.module_name import *
```

Then `__all__` variables define the index of modules.



### Reference between modules and packages

To use intra-package references imports use leading dots to indicate the current and parent packages involved in the relative import. Relative imports are based on the name of the current module.

A single dot means that the module or package referenced is in the same directory as the current location:
```python
from . import echo         
```

Two dots mean that it is in the parent directory of the current location—that is, the directory above:
```python
from .. import formats 
```

## Rules for Search Modules and Packages

When you import a module via keyword [import](https://docs.python.org/3/reference/simple_stmts.html#import) runtime importing the module via following the next rules:

1. Interpreter first searches for a built-in module. The built-in module names are built-in into the language and are listed in `sys.builtin_module_names`.

2. In a list of directories given by the variable `sys.path`.

Python programs can modify the `sys.path` variable itself, but the variable `sys.path` after starting the interpreter as a process Python Runtime initializes this variable with these locations:

* The directory containing the input script (or the current directory)
* Paths in environment variable PYTHONPATH
* The installation-dependent default such as the site-packages directory.

## Comprehensions syntax

Comprehensions syntax provides a short syntax to create several iterable types.

List comprehensions in Python:
```python
[expr(item) for an item in iterable if condition (item)]  
```

set comprehensions in Python:
```python
{expr(item) for an item in iterable if condition (item)}
```

Dictionary comprehensions in Python:
```python
{someKey:someValue for someKey, someValue in someDict.items() if condition(someKey, someValue)}
``` 

Generator comprehensions in Python: 
```python
(expr(item) for item in iterable if condition(item))
```

List comprehension is described in Python Tutorial [1]: 
[list comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions), [nested list comprehensions](https://docs.python.org/3/tutorial/datastructures.html#nested-list-comprehensions)


## Random Interesting Construction

1. Work with the reverse sequence:
    ```python
    for i in reversed ([1,3]): 
        print i  
    ```

2. Dictionary (also known as a map or associative array in other languages) can be built through `{}`, which takes as input list of pair-tuples with key and value separated by a column.
    ```python
    emptyDictinoary = {}
    ```

3. Dict can be merged with another dict via [update()](https://docs.python.org/3/library/stdtypes.html?highlight=update#dict.update) method.

4. In Python another datatype which is built-in in the Language is set. On sets, you can perform set-theoretic operations on it. Example:
    ```python
    aSet = {1,2}   # set
    ```

5. Creating an empty seta, possible only through the constructor because `{}` is reserved as an empty dictionary description and interpreter due to its design (it has a Dynamic Type System) can not derive information that you want an empty set, no dictionary. Example:
    ```python
    emptySet = set()
    ```

6. Exceptions with variable names.

    See [Handling Exceptions](https://docs.python.org/3/tutorial/errors.html#handling-exceptions). Example:


    ```python
    def hello(a): 	
      try:  		
        raise ValueError() 
        return +2  	
      except (ValueError, TypeError)  as e :    		
        return -1 	
      finally:  		
        #clean code  		
        pass
    ```

7. There is a ternary expression similar to C/C++ `?:` expression. Example:
    ```python
    sym = '0' if 1> 5 else ''1" 
    ```

    Documentation: https://docs.python.org/2/reference/expressions.html#conditional-expressions

8. Launch the Python web server to share files from the current directory:
    ```bash
    python -m http.server
    ```

9. Deleted statement. Delete statement `del` is used to delete elements from a container such as a list. And also it is used to delete variables from the interpreter. After variable deletion, the variable is not defined. If a variable is not "defined" which means it has not been assigned a value or deleted, trying to use it for reading will give you an error. Trying to use it for write will create a new variable. Objects are never explicitly destroyed. When they become unreachable they may be garbage-collected. An implementation is allowed to postpone garbage collection or omit it altogether it is a matter of implementation quality how garbage collection is implemented.

10. Multiple assignments in Python:
    ```python
    a, b = 0, 1
    ```

# Technical Details about Functions

## About Indentation

Unfortunately, Python was designed using indentation, and there are no scopes constructed with `{}` as in C/C++. During writing language construction sometimes it can be shorter, but sometimes when you 3-9 nested loops of Algorithm logic, the indentation makes the problem less readable and more error-prone. In principle, you can use both spaces and tabs, but it is recommended to use spaces instead of tabs according to this recommendation:
https://www.python.org/dev/peps/pep-0008/#tabs-or-spaces

## Arguments of the function and return value

The first statement of the function body can optionally be a string literal - this string literal is the functions documentation string, or docstring.

The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table.

Variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names. Global variables and variables of enclosing functions cannot be directly assigned a value within a function (unless you use `global` and `nolocal` statements). Although they may be referenced.

The actual parameters (arguments) to a function call are introduced in the local symbol table of the called function when it is called. Arguments are passed by value, where the value is always an object reference, not the value of the object. A function definition associates the function name with the function object in the current symbol table.

The `return` statement returns with a value from a function. Return without an expression argument returns None. Falling off the end of a function also returns `None`.

## Default Value of Argument for a function

The most useful form is to specify a default value for one or more arguments. Important warning: The default value is evaluated only once.

## Keyword and Positional Arguments

Functions can be called using keyword arguments of the form `kwarg = value`. Keyword parameters are also referred to as *named parameters*. Rules for using keyword parameters are the following:

* In a function call, keyword arguments must follow positional arguments
* All the keyword arguments passed must match one of the arguments accepted by the function
* No argument may receive a value more than once

## Syntax to split positional and keyword

```python
def f(pos_only, pos_only_2, /, pos_or_kwd, *, kwd_only_1, kwd_only_2):
    pass
```

Optionally used symbols `/` and `*` indicate the kind of parameter by how the arguments may be passed to the function: positional-only, positional-or-keyword, and keyword-only.

## Varying Number of Arguments

You can specify that a function can be called with an arbitrary number of arguments.
One way is to use syntax to wrap varying positional arguments in a tuple. Example:

```python
def f(a, *args):
    print(type(args))
```

The second way is to use syntax to wrap varying keyword arguments into a dictionary. Specifically, when a final formal parameter in the function is in the `**name` it receives a dictionary containing all keyword arguments except for those corresponding to a formal parameter. Example:

```python
def f(a, **args):
    print(type(args))
```

Next, if you want to call a function by passing a *tuple* and *dictionary*, but transfer this not as objects but make a substitution of the content of *tuple* and *dictionary* as a formal argument you should:
* Expand list and tuple with `*`
* Expand the dictionary (dict) with `**`

Example:
```python
def printInfo(*x, **kv):
   print(x)
   print(kv)

x = (1,2) 
kv = {'a':2,'b':4,'c':6}

printInfo(*x, **kv)
printInfo(*x, aa = 1, bb = 9, cc = 1)

# Output:
# (1, 2)
# {'a': 2, 'b': 4, 'c': 6}
# (1, 2)
# {'aa': 1, 'bb': 9, 'cc': 1}
```

## Small anonymous functions can be created with the lambda keyword

Lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. 

## Function and Type Annotation

Function annotations ([link](https://docs.python.org/3/tutorial/controlflow.html#function-annotations)) and type annotation ([link](https://docs.python.org/3/library/typing.html)) are completely optional metadata information about the types used by user-defined functions (see [PEP 3107](https://peps.python.org/pep-3107/) and [PEP 484](https://peps.python.org/pep-0484/) for more information). 

Annotations are stored in the `__annotations__`attribute of the function as a dictionary. 

Parameter annotations are defined by a colon after the parameter name, followed by an expression evaluating the value of the annotation. Return annotations are defined by a literal `->`, followed by an expression, similar to the trailing return type in C++.

Annotations do not affect any part of the function (if the function does not use this system meta information in its logic).

General Convention is using parameter annotations with expressions that produce a `type` value, and this functionality is used to augment Python with type information. Example:
```python
def f(ham:str , eggs:str  =  'eggs' )->str: 
    print( "Annotations from function:" , f . __annotations__ )
    return "1"

print( "Annotations outside function:" , f . __annotations__ )

f("1", "2")
```

# Function Decorators

A function definition may be wrapped by one or more decorator expressions. Decorator expressions are evaluated when the function is defined, in the scope that contains the function definition. The result must be callable, which is invoked with the function object as the only argument. The returned value is bound to the function name instead of the function object. Multiple decorators are applied in a nested fashion. 

More information is available in [2]:
https://docs.python.org/3/reference/compound_stmts.html#grammar-token-python-grammar-decorators.

For example, the following code will measure the time for function execution (it has been taken from https://stackoverflow.com/questions/5478351/python-time-measure-function):

```python
#!/usr/bin/env python3
import time   

def timing (f):
  def wrap(*args):
      time1 = time.time()
      ret = f(*args) 
      time2 = time.time()
      
      print ('{} function took {:.3f} ms'.format(f.__name__, (time2-time1) * 1000.0))           
      return ret     

  return wrap   

# Use Attributes
@timing 
def f1(seconds): 
  time.sleep(seconds)   

# Similar code without attributes
def f2(seconds):
  time.sleep(seconds)   

def f2_with_time(seconds):
  funcion_to_call = timing(f2)
  funcion_to_call(seconds)

f1(1.5)
f2_with_time(1.5)
```

This concept is called decorator because the intention of using it is to decorate function calls with specific pre/post-processing.

# Classes in Python

Classes provide a means of bundling data and functionality together. In C++ normally terminology class members, the data members are public, and all member functions are virtual. 

The method function is declared with an explicit first argument representing the object, which is provided implicitly by the call. Unlike C++ built-in types can be used as base classes for extension by the user. Also, like in C++, most built-in operators with special syntax can be redefined for class instances.

The [super()](https://docs.python.org/3/library/functions.html#super) lets you avoid referring to the base class explicitly, which can be nice sometimes.

```cpp

#!/usr/bin/env python3

class Base:
  def f(self): print("Base")
  def fproxy(self): self.f() 

class DerivedA(Base):
  def f(self): print("DerivedA")

class DerivedB(Base):
  def f(self): print("DerivedB")


class DerivedAB(DerivedA, DerivedB):
  def f(self): 
      print("DerivedAB")
      Base.f(self)                 # Call Base explicitly
      super(DerivedAB, self).f()   # Call one of direct base with using super()
      DerivedB.f(self)             # Call DerivedB explicitly

obj = DerivedAB()
obj.fproxy()
```

# Python Technical Details. Advanced.

## Special or Magic method for classes

Magic is an official term used by the Python community, even though in professional literature this term is used rarely. This informal name shines a light that a lot of things inside the Python community happen informally without any standardization. The effect that both styles (formal and informal) can coexist can be obtained into look into API and development style for Android OS and Linux/Windows OS. The development for Android OS is mostly cowboy style.

A class can implement certain operations that are invoked by special syntax.
A complete list of these special methods is available in The Python Language Reference [2] https://docs.python.org/3/reference/datamodel.html#special-method-names.


## Module Reloading

For efficiency reasons, each module is only imported once per interpreter session. 
If you change your modules source code you have two options on how to reload this module:

* Restart the interpreter
* Use [reload()](https://docs.python.org/3/library/imp.html?highlight=reload#imp.reload) function from [imp](https://docs.python.org/3/library/imp.html) module which provides access the import internals. Example: 
  ```python
  import imp
  imp.reload(module name)
  ```

## Encoding for Files

The text file is iterable and the content of the file is returned line by line. You can check the used default encoding for reading text files:
```python
import sys 
print(sys.getdefaultencoding())
```

If you want to open the file in the specified encoding you can specify this in [open()](
https://docs.python.org/3/library/functions.html#open).

To open and close files you can use open()/close() calls. One shorthand provides:
* The opening of the file 
* Closing in success/exception
is [with](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement) statement. 

Example:
```python
with open("my_file.txt", "rt") as f:
  for line in f:
      print(line)

```

The reason for closing files is in that some objects contain references to external resources such as open files in the filesystem. It is understood that these resources are freed when the object is garbage-collected, but since garbage collection is not guaranteed to happen, such objects also provide an explicit way to release the external resource, usually with a `close()` method.

## Defaultdict

[Defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict) is a subclass (derived class) of the built-in [dict](https://docs.python.org/3/library/stdtypes.html?highlight=dict#dict) class. It can be found in the [collections](https://docs.python.org/3/library/collections.html) module in Python standard library.

 [Defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict) is working mostly like a [std::map](https://en.cppreference.com/w/cpp/container/map) in C++. When the key is encountered for the first time and it is not already in the mapping then an entry value corresponding to the requested *key* is automatically created using the default_factory function and is instantiated.

Example:
```python
from collections import defaultdict

m1 = defaultdict(int) 
# default value will be an integer value of 0
m2 = defaultdict(str)
# default value will be an empty string ""
m3 = defaultdict(list)
# default value will be an empty list []
```

## Match Statement

A match statement takes an expression and compares its value to successive patterns given as one or more case blocks. This is similar to a switch statement in C/C++ conceptually.

```python
 match status:
        case 400:
            return "Bad Request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the internet"
```

The variable name `_` acts as a wildcard and never fails to match, similar to `default:` in C/C++.

# Walrus

In Python, unlike C and C++, assignment inside expressions which is used in the conditional statement must be done explicitly with the walrus operator `:=`. 

The operator `:=` in Python can be used in the context when you want to perform an assignment inside an expression that is used for a condition.

It's the same as the operator `=` in C++ but with restricted context.

```python
a=2
if a:=1:
  print(a)

# Output
# 1
```

## Generators

The class-based iterators is a functionality that is implemented via definition:
* `__iter__` method
* `__next__` method

See also [Interface and Protocols](#interfaces-and-protocols) in this document. What makes generators so compact is that the `__iter__()` and `__next__()` methods are created automatically.

Next the key feature of **generator** is that the local variables and execution state are automatically saved between calls. When generators terminate, they automatically raise [StopIteration](https://docs.python.org/3/library/exceptions.html?highlight=stopiteration#StopIteration) exception.

The generators functionality dies not return a value via [return](https://docs.python.org/3/reference/simple_stmts.html?highlight=return#return) statement, instead, they send a value via [yield](https://docs.python.org/3/reference/simple_stmts.html?highlight=return#grammar-token-python-grammar-yield_stmt) statement. See also: [link to generators](
https://docs.python.org/3/tutorial/classes.html#generators).

```python
def gen():     
    print ("Entry point in gen") # This will be executed only once, even a function yields several values
    yield 1 # emit value      
    yield 2 # emit one more value   

a = gen ()
print(next(a))   # use it to explicitly first yeild statement
print(next(a))   # run to second yield statement, etc. 

# Next yield will throw a StopIteration exception    
```

# Conda/Pip/Venv package and environment managers

## Package managers

Conda has two goals:
  1. Conda is a package manager.
  2. Conda is an environment manager. 

Each project can have a specific requirement in a specific version of the library and this is the motivation behind the environment manager idea.

To install Conda there are two ways to:
  1. Anaconda distribution contains Conda and other things.
  2. Install Miniconda (https://docs.conda.io/en/latest/miniconda.html)

I prefer not to use pip directly, but instead use command for call pip directly from Python:
```python
python -m pip list
```
It's useful to eliminate problems with various installations of Python interpreters.

Package manager commands comparison:

| # | **Command in Conda**      | **Command in Pip** | **Description** |
|---|---------------|------------|------------------------|
| 1 | conda search  | pip search | Search package                       |
| 2 | conda install/uninstall	     | pip install/uninstall name	  | install/uninstall package              |
| 3 | conda update	| pip install --upgrade name	   | upgrade package                       |
| 4 | conda install package=version | pip install package=version  | Install a specific version of package   |
| 5 | conda list	| pip list 	 | List of installed packages      |
| 6 | conda install mnist --channel conda-forge | pip install somepkg --extra-index-url http://myindex.org  |     Install package from non-standart place |
| 7 | conda update package_name | pip install -U package_name | Upgrade package |
| 8 | [Use pip for it, not conda](https://docs.conda.io/projects/conda-build/en/latest/user-guide/wheel-files.html) | pip install *.whl | Install package from local WHL ([Python wheel packaging standard](https://packaging.python.org/en/latest/specifications/binary-distribution-format/)) distribution.
| 9 | conda list \| grep <package_name> | pip show <package_name> | Show information about package |

Conda cheatsheet:
https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf

Conda documentation by the tasks:
https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/index.html

How to use pip:
https://pip.readthedocs.org/en/stable/

Python Package Index repository:
https://pypi.python.org/pypi - standard packages repository

## Environment managers

The venv module is a standard virtual environment in Python. In this section, we will compare venv with conda. If you want to obtain a list of available conda environments please use [conda info -e](https://docs.conda.io/projects/conda/en/main/commands/info.html).


| # | **Command in Conda**      | **Command in venv module** | **Description** |
|---|---------------|------------|------------------------|
| 1 |  conda create --name my	  |  python -m venv my	| Create an environment with the name "my" |
| 2 |  conda create --name my python==3.6.4 scikit-learn |  pip install	 | Create an environment with several packages in it|
| 3 |  conda activate my |  source ~/envs/my/bin/activate | Activate specific environment |
| 4 |  conda deactivate	|  source ~/envs/my/bin/deactivate	 | Deactivate environment |
| 5 |  conda list	|  pip freeze	 | Get List of installed packages in the current environment |
| 6 |  conda env export --file myenv.yml |  pip freeze > requirements.txt | Export  information about packages in the current environment | 
| 7|   conda env create --file myenv.yml | pip install -r requirements.txt | Install all packages from requirement list |
| 8 |  conda remove --name myenv --all	| Remove directory in filesystem with environment and it's scripts | Remove environment |

# Python Notebooks

Jupyter documentation is available here https://docs.jupyter.org/en/latest/. Jupyter Notebooks presents a way to create source code mixed with documentation in the form of Markdown. Also, Notebooks can serialize the results of numerical computation in the form of graphics into a single file.

Python Notebooks by itself does not provide means to debug Python Notebooks, however some IDE as [Visual Studio Code](https://code.visualstudio.com/) support debugging of code inside Python Notebooks.

The following command will start the web server and open web-client:

```bash
python -m notebook
```

Start the web server and allow connection from outside (incoming connection from all network interfaces will be accepted):

```bash
python -m notebook --ip 0.0.0.0 --port 8888
```

> If you have problems with accessing the web server the first thing is to take a look if the port that you have specified listens to the port for incoming network connection. In Posix OS it can be done by using the [netstat](https://linux.die.net/man/8/netstat) command:
>  ```python
>  netstat -nap | grep -E "tcp(.)*LISTEN"
>  ```
> Please be aware that if the server is running on some machine it can be a case that the computer administrator blocks the ports. In Linux-based OS, by default, in most cases, people use iptables to configure acceptance of incoming connections. 
> The first thing you can try to execute from the root is the following command to allow all incoming connections from all interfaces:
> ```python
> iptables -I INPUT -j ACCEPT
> ```

Python notebook files extension is `*.ipynb`. The notebook contains code snippets and markdown text organized by cells. Cells roughly speaking can be of two types:

* Code
* Markdown

## Working in a web-based interface.

| **Command**      | **Description** |
|---------------|------------|
| ALT+Enter          | Append cell (something like the section in Matlab)          |
| \$a_1+a_2\$     | It's possible to use Latex in Markdown         |
| \%matplotlib nbagg      | Append separate widget where output should be plotted          |
| \%matplotlib inline | Inline output into the notebook          |
| Left click on cell | Select to edit |
| Esc | Deselect for edit |
| Shift + down / Shift + up | Append cell to selection |
| Shift + Enter | Execute (selected) cell and go to next cell  |
| \%timeit | Built-in command in Jupyter does measure the time of command. [Documentation](https://ipython.org/ipython-doc/dev/interactive/magics.html#magic-timeit).|
|  CTRL+M, B | Create code cell|
| CTRL+M, M	| Convert code cell to text cell |
| CTRL+M, Y | Convert text cell to code cell |
| %%bash; ls -1; lsb_release --all | Magic sequence to execute bash commands via `%%bash`. [Documentation](https://ipython.org/ipython-doc/dev/interactive/magics.html#cellmagic-bash). |
| !pip install numpy | Magic command for executing bash command at code cell via using `!` mark. [Documentation](https://ipython.org/ipython-doc/dev/interactive/magics.html#magic-sx).|


Example of usage timeit inside code cells:
```python
%timeit for _ in range(10**3): True
```

Documentation: 
https://ipython.org/ipython-doc/dev/interactive/magics.html


## PyTorch resources

PyTorch is a big numerical package.

| **Description**      | **Link** |
|------------------|------------|      
| PyTorch index for several topics | https://pytorch.org/docs/stable/index.html|
| The PyTorch Cheat Sheet | https://pytorch.org/tutorials/beginner/ptcheat.html |
| PyTorch API | https://pytorch.org/docs/stable/torch.html |
| Tensors | https://pytorch.org/docs/stable/tensors.html |
| Parameters | [https://pytorch.org/docs/stable/generated/torch.nn.parameter.Parameter.html](https://pytorch.org/docs/stable/generated/torch.nn.parameter.Parameter.html#torch.nn.parameter.Parameter) |
| Tensors required grad modification | https://pytorch.org/docs/stable/tensors.html#torch.Tensor.requires_grad_ |
| Moving tensor to GPU | [https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html](https://pytorch.org/tutorials/beginner/blitz/data_parallel_tutorial.html#sphx-glr-beginner-blitz-data-parallel-tutorial-py) |
| Data relative/Data loader | https://pytorch.org/docs/stable/data.html#torch.utils.data.DataLoader |
| Data relative/Dataset | https://pytorch.org/docs/stable/data.html#torch.utils.data.Dataset |
| Data relative/Custom data loader | https://pytorch.org/tutorials/beginner/data_loading_tutorial.html |
| Module/About Modules | https://pytorch.org/tutorials/beginner/blitz/neural_networks_tutorial.html |
| ModuleBase class for all neural network Module/Layer | https://pytorch.org/docs/master/generated/torch.nn.Module.html#torch.nn.Module |
| The module which allows building a network sequentially |https://pytorch.org/docs/master/nn.html#torch.nn.Sequential |
| Turn on/off module training mode | https://pytorch.org/docs/master/generated/torch.nn.Module.html#torch.nn.Module.train |
| Linear Layer/Module | https://pytorch.org/docs/master/generated/torch.nn.Linear.html#torch.nn.Linear |
| Relu layer | https://pytorch.org/docs/master/generated/torch.nn.ReLU.html |
| Data Transform | https://pytorch.org/vision/stable/transforms.html |
| Image/Tensor transformer | https://pytorch.org/vision/0.8/transforms.html#torchvision.transforms.ToTensor |
| Tensor values normalization | https://pytorch.org/vision/0.8/transforms.html#torchvision.transforms.Normalize |
| Losses/Binary Cross-Entropy loss | https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html |
Losses/Binary Coss-Entropy with logits loss | [https://pytorch.org/docs/master/generated/torch.nn.BCEWithLogitsLoss.html](https://pytorch.org/docs/master/generated/torch.nn.BCEWithLogitsLoss.html#torch.nn.BCEWithLogitsLoss) |
| Losses/Cross-Entropy loss | https://pytorch.org/docs/stable/generated/torch.nn.CrossEntropyLoss.html |
| Optimizers | https://pytorch.org/docs/stable/optim.html
| Autograd | https://pytorch.org/tutorials/beginner/blitz/autograd_tutorial.html |
|

> Comment: In the context of deep learning the logits mean the layer or scalars from R that are fed into softmax or similar layer in which the image(or output) is a probabilistic simplex.

# Various code snippets

## Numpy 
###  Ellipsis in NumPy
The ellipsis is used in NumPy to slice higher-dimensional data structures. It's designed to mean at this point, insert as many full slices (`:`) to extend the multi-dimensional slice to all dimensions.

```python
import numpy as np
a = np.array ([[1,2,3], [4,5,6]])
b = a [...]
b[0,0]=11
print(a == b)
```

###  Newaxis in Numpy

Simply put, the new axis is used to increase the dimension of the existing array by one more dimension when used once. 
```python
import numpy as np
a = np.array ([[1,2,3], [4,5,6]])
b = a [..., np.newaxis]
print(b.shapea)
```
## Matplotlib
### Show the image with Matplotlib
```python
import sys 
import matplotlib.pyplot as plt 
import numpy as np 
img = np.random.randint(low =  0, high =  255, size =  (16 ,  16,  3)) 
plt.imshow(img) 
plt.show()
```

### Show a one-dimensional plot (simple)
```python
import matplotlib.pyplot as plt 
import numpy as np
import matplotlib.pyplot as plt 
x = np.arange(0 , 10 , 0.1)
y = np.sin(x) 
plt.plot(x, y)  
plt.grid(True)
plt.show()
```
### Show sub-images
```python
import sys 
import matplotlib.pyplot as plt 
import numpy as np 
out_arr = np.random.randint(low =  0, 
                            high = 255, 
                            size = (16, 16, 3))
plt.figure(figsize = (15 , 15)) 

for n in range(16):
  ax = plt.subplot( 4 , 5, n + 1 )
  ax.title.set_text( 'Plot #'  +  str ( n + 1 ) )     
  plt.imshow ( out_arr )  
  plt.axis( 'off' ) 
  plt.subplots_adjust (wspace = 0.01 , hspace = 0.1 )  

plt.show( )
```

# Cython

[Cython](https://cython.readthedocs.io/en/latest/index.html) is Python with C data types. Official documentation: https://cython.readthedocs.io/en/latest/index.html.

Cython has two major use cases: 
1. Extending the CPython interpreter with fast binary modules.
2. Interfacing Python code with external C libraries.

Almost any piece of Python code is also valid Cython code. Usually, the speedups are between **2x** to **1000x**. It depends on how much you use the Python interpreter compared to situations where your code is in C/C++ Libraries.

There are two file types for Cython:
* `*.pyx` - something similar to C/C++ source files.
* `*.pxd` - something similar to C-header files. PXD files are imported into PYX files with the `cimport` keyword.

The files in Cython have several functions inside them:
* `def-functions` - Python functions are defined using the def statement, as in Python. Only Python functions can be called from this function.

* `cdef-functions` - C functions are defined using the new compare to Python `cdef` statement. Within a Cython module, Python functions and C functions can call each other.

* `cpdef-functions` - is a hybrid of `cdef` and `def`. It uses the faster C calling conventions when being called from other Cython code and uses a Python interpreter when they are called from Python. Essentially it will create a C function and a wrapper for Python.


During passing argument from `cdef` to `def` functions and vice versa there is an automatic type casting is occurring:
https://cython.readthedocs.io/en/latest/src/userguide/language_basics.html#automatic-type-conversions

## How to optimize Python Code with Cython

0. Install Cython for your Python interpreter: `python -m pip install Cython`.

1. First step is to take the usual Python file "*.py" and change the extension to "*.pyx".

2. Second and most powerful step that can be used now during using Cython you can append type information to variables. In practice especially if you are using loops it brings good speedup immediately. Examples:

    ```python
    #!/usr/bin/env python3

    cdef int x,y,z
    cdef char *s
    cdef float f1 = 5.2    # single precision
    cdef double f2 = 40.5  # double precision
    cdef list languages
    cdef dict abc_dict
    cdef object thing
    ```

3. Next step you can append extra modifiers to functions. Example:
    ```python
    #!/usr/bin/env python3

    # Can not be called from Python
    cdef sumCDef(): 
      cdef int i
      cdef int s = 0
      for i in range(10):
        s += i
      return s

    # Can be called from Python and Cython
    def sumDef(): 
      cdef int i
      cdef int s = 0
      for i in range(10):
        s += i
      return s

    # Can be called from Python and Cython effectively
    cpdef sumCpDef(): 
      cdef int i
      cdef int s = 0
      for i in range(10):
        s += i
      return s
    ```

4. Next Step is to build your code because Cython is not an interpretable Python language extension. The script that describes that you want to build all *.pyx files in the current directory, which is typically used for projects that use Cython:
    ```python
    #!/usr/bin/env python
    # filename: setup.py
    # pip install Cython
    from setuptools import setup
    from Cython.Build import cythonize
    setup(
        name        = 'Test',
        ext_modules = cythonize("*.pyx"),
        zip_safe    = False,
    )
    ```
5. Launch the build process from the previous description:
    ```bash
    python build.py build_ext --inplace
    ```
    The output of this command is
    * `*.so` in Unix-like OS.
    * `*.pyd` in Windows.

6. If the build process was successful then to import your module into the Python interpreter you can use the usual `import` statement:
    ```python
    import my_module
    ```

## About Cython Language

The `cdef`  statement is used to declare C variables, either in the local function scope or in the module level:
```python
cdef int i, j, k
```

With cdef, you can declare `struct`, `union`, `enum`
(See [Cython documentation](https://cython.readthedocs.io/en/latest/src/userguide/language_basics.html#structs-unions-enums))
:
```python
cdef struct Grail:     
  int age     
  float volume
```

You can use `ctypedef` as an equivalent for C/C++ typedef:
```
ctypedef unsigned long ULong
```

It's possible to declare functions with cdef, making them C functions:
```python
cdef int eggs(unsigned long l, float f)
```

References with further details:
* https://cython.readthedocs.io/en/latest/index.html
* https://pythonprogramming.net/introduction-and-basics-cython-tutorial

## Easy Interoperability with Standar C Library
```python
#!/usr/bin/env python3

cdef extern from "math.h":     
    double sin(double x)

from libc.math cimport sin
from cpython.version cimport PY_VERSION_HEX

cpdef double f(double x):     
  print("CPython version for which code is compiled:", 
        PY_VERSION_HEX)
  return sin(x * x)
```

External declaration in form `cdef extern from "math.h":     double sin(double x)` declares the `sin()` function in a way that makes it available to Cython code. It instructs Cython to generate C code that includes the `math.h` header file. 

The C compiler will see the original declaration in math.h at compile time. Importantly Cython does not parse "math.h" and requires a separate definition in such form.

It is possible to declare and call into any C library as long as the module that Cython generates is properly linked against the shared or static library.

### Example of function Integration in Cython and Python
```
# Source File: integration.pyx 
# Depencies:
#  python -m pip install cython

import time

def timing(f):
    def wrap(*args):
        time1 = time.time()
        ret = f(*args)
        time2 = time.time()
        print('{:s} function took {:.3f} ms'.format(f.__name__, (time2-time1)*1000.0))
        return ret   
    return wrap  

def f(double x):     
    return x ** 2 - x  

@timing 
def integrate_f(double a, double b, int N):  
    cdef int i     
    cdef double s, dx

    s = 0    
    dx = (b - a) / N  
    for i in range(N):
        s += f(a + i * dx)
    return s * dx  

@timing 
def integrate_f_std(a, b, N):     
    s = 0     
    dx = (b - a) / N   
    for i in range(N):    
        s += f(a + i * dx)  
    return s * dx

# Build with:
#   python setup.py build_ext --inplace
#
# Where setup.py:
#  from setuptools import setup
#  from Cython.Build import cythonize
#  setup(
#     name        = 'Reduction Test',
#      ext_modules = cythonize("*.pyx"),
#      zip_safe    = False,
#    )

# Use from Python after build:
#   import integration
#   integration.intergrate_f(0.0,100.0,1000)
#   integration.integrate_f_std(0.0,100.0,1000)
```

# References

# Introduction document
[1] Python Tutorial: https://docs.python.org/3/tutorial/

# Reference official materials
[2] Python Language Reference: https://docs.python.org/3.8/reference/index.html

[3] Built-in Types: https://docs.python.org/3/library/stdtypes.html 

[4] Table of content (index) for Python: https://docs.python.org/3/contents.html

[5] Python Enhancement Proposals: https://www.python.org/dev/peps/

[6] Description of the meaning of various special member functions: https://docs.python.org/3/reference/datamodel.html#emulating-callable-objects

[7] Python standard library: https://docs.python.org/3/library/index.html#library-index

# Mapping concepts from other languages/libraries to Python language/libraries

[8] Matlab/Numpy translation: http://mathesaurus.sourceforge.net/matlab-numpy.html

# Tutorial for Libraries

[9] http://cs231n.github.io/python-numpy-tutorial/

# Howto

[10]  http://www.java2s.com/Tutorial/Python/0400__XML/AccessingChildNodes.htm

[11] http://book.pythontips.com/

[12] How to for Python: https://docs.python.org/3/howto/index.html

# Repositories

[13] Find, install, and publish Python packages:
https://pypi.org/
