# Typhon

Programming language design.

A statically typed Pythonic langauge with ability to be compiled into binary, intepreted, or 
converted into JavaScript.

Named after the monster in Greek mythology with the heads of a thousand snakes, the origin of 
the word 'Typhoon', and is a play on words such that it is a 'typed' language.

Value-types are recommended (minimal OOP), and a different approach to async syntax shall be 
used

## Features

### Fancy-Features

Features from Python are still present:
* List/Dict/Set comprehension (`[x for x in myList]`, `[]`)
* Context managers (`with`)
* Operator overloading (`__+__`, `__*__`)
* Generators/Iterables (`yield`)
* Decorators/closures (`@decorator`)

### Docstrings and Comments

Comments are defined as `#<comment>`. Multi-line comments not supported.

Docstrings will be defined using double hash symbol `##<docstring>`. i.e.
```python
def myFunction(int a, float b, decimal c, Object d) -> List:
    ## This is my function. It does very cool things.
    ## 
    ## Args:
    ##   a (int): is a number.
    ##   b (float): is a float.
    ##   c (Object): is an Object.
    ##
    ## Returns:
    ##   (List): a list of things.
```

> Docstrings below the function header (like Python) positions your function and docstring 
> in a cleaner and easy to understand way. Using double hash '##' is better than multi-line 
> strings (`"""Stuff"""` as used in Python) because it avoids annoyances of inconsistent
> indentation, and deferentiates the docstrings from strings. 

### Unpacking/Spread Operator

Unpacking will be the same as Python, using the `**`/`*` operators. i.e.
```python
# foo, Hello as hello = **{"foo": "bar", "Hello": "World"} # WOULD NOT BE AVAILABLE
myFunction( **{"foo": "bar", "Hello": "World"})

one, two, three = *[1, 2, 3]
myFunction(*[1, 2, 3])
```
> JavaScript spread assignment syntax `{}`/`[]` is very nice functionality, however, I 
> do not want to use braces so carelessly. 
> Object/Dict unpacking will not be allowed into the spec of this language (unless something 
> better can be figured).

### Annonymous Functions

We will continue to use `lambda x: x` from Python. It is difficult to implenent braces in a 
indentation-based language, and arrow functions are less obvious for new-comers.

### Imports/Exports

Imports are the same as Python. i.e. `import module from package`.

All attributes of a module will be exported by default. If the `export` keyword is present,
then only those attributes will be exported. Alternatively, the `@export` decorator may be
a good idea.

### Asyncrony/parallelism

There will be no async-await syntax.

Instead, function calls may be prefixed with `go` (i.e. `go myFunction()`), which will run the
code in a seperate thread and returns a Promise. 

These threads act the same way as Python multithreading - which is 
not actually parallel, but switches between running Bytecode between each thread. When a 
blocking operation/IO occurs, then the other threads will continue running. 

This method will allow ability to easily distinguish between parallel/sequencial code, and makes
it easy to switch between the two methods.

### Typing

Interfaces may be defined. Abstract classes not recommended.

It may be possible to create interfaces automatically, using `MixedType<A,B>`. But I have not
figured precisely how this would be optimal. It could be potentially used to ensure that 
interfaces do not need to be defined at all - when mixed-type lists are present.

Types are defined in function (header) definitions. (The return type and input parameters). 
Types declarations are not written within the function, they are inferred by function outputs.

Function definitions are defined like below:
```python
def myFunction(int a, float b, decimal c, Object d) -> List:
```

### Primitive Types and Collections

Primitive types will be slightly more comprehensive than in Python. 

Some primitive types:
* int (e.g. `1`)
* float (e.g. `1.2`)
* decimal (e.g. `1.2dec`)
* double (e.g. `1.2doub`)
* Promise

Structs are also available. They are similar to C#. They are mutable value-types.

Since this language is statically-typed, collections will need interfaces. Value-typed collection 
variants may also be available.

Some of the available collections:
* tuple (e.g. `tuple<int, decimal>`, `(1, 2, 3)`)
* array (e.g. `array<int>(shape=(2,2))`)
* List (e.g. `List<int>();`, `[1, 2, 3]`)
* Dict (e.g. `Dict<int>();`, `{"A": 1, "B": 2, "C": 3}`) [note: any type may be passed into the key]
* Set (e.g. `Set<int>();`, `{1, 2, 3}`)

### Package Index (TyPi)

There are three levels within the package index:
1. 'Base' packages, which form the core library. They hold no dependencies.
2. 'Second-level' packages. They rely on base packages.
3. Third-level' packages. They rely on anything, and are less recommended.

The packages will be tagged with labels (JS-compatible, BIN-compatible) so that developers
know if their dependencies and entire application can be compiled into a web applications 
or binary application. (i.e. browser package will not be available for binary applications).

Installation of project-package can potentially be done using the `typhon_packages` directory. 
(i.e. your `node_modules` and `package.json` in JS). **I have not figured this out yet**.

> The goal is to have a strong and robust standard library and dependencies system.
> It is noteworthy that supporting both JS and BIN will be a dificult task for the language.

