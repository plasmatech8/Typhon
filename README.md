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
* List/Dict/Set comprehension (`[x for x in myList]`, `{key: value for key, value in myDict.items()}`)
    * Note: You cannot initialise an empty collection using list/dict/set comprehension due to type ambiguity
* Context managers (`with`)
* Operator overloading (`__+__`, `__*__`, `__>=__`)
* Generators/Iterables (`yield`)
* Decorators/closures (`@decorator`)
* Chained comparisons (`1 < x <= 10`)
* Nullish fallback operator / nullish coalescing (`null ?? 'default_value'`) (a nullish operator)
* Falsy fallback operator - maybe (`false or 'default_value'` or perhaps `false || 'default_value'`)
* Optional chaining - stops with null if null (`myObject?.mySubObject?.myAttribute` or `myObject?mySubObject?myAttribute` or `myObject?.mySubObject?.myList?[]`) (a nullish operator)

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

**on second thought, I might stick to `a: int` notation for type annotations**

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
> better can be figured). >>> why? idk - doesn't feel right. maybe it is fine?

### Annonymous Functions

We will continue to use `lambda x: x` from Python. It is difficult to implenent braces in a 
indentation-based language, and arrow functions are less obvious for new-comers.

### Imports/Exports

Imports are the same as Python. i.e. `from package import module`.

All attributes of a module will be exported by default. If the `export` keyword is present,
then only those attributes will be exported. (e.g. `export my_function`)

It is also possible to add a `@export` decorator for a more convenient.

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

Class/struct attributes are defined like below:
```
attr str name = "Mark"
attr int age;
```

> Static typing decreases effort required for input validation because there is no need for
> type checking all the variables.

**on second thought, I might stick to `name: str` notation for type annotations**

### Basic Types 

Primitive types will be slightly more comprehensive than in Python. 

Some primitive types:
* int (e.g. `1`)
* float (e.g. `1.2`)
* decimal (e.g. decimal('1.0') or posibly `1.2dec`)
* double (e.g. double(1.34) or possibly `1.2doub`)

Structs are also available. They are similar to C#. They are mutable value-types.

### Collection Types

Since this language is statically-typed, collections will need interfaces (when not using the list/dict comprehension). 
Immutable or value-typed collection variants may also be available.

Some of the available collections:
* tuple (e.g. `tuple<int, decimal>`, `(1, 2, 3)`)
* array (e.g. `array<int>(shape=(2,2))`, `[1; 2; 3; 4]`, `[1, 2, 3; 4, 5, 6; 7, 8, 9]`) [fits better alongside tuples, but want to use square brackets, idk]
* List (e.g. `List<int>();`, `[1, 2, 3]`)
* Dict (e.g. `Dict<int>();`, `{"A": 1, "B": 2, "C": 3}`) [note: any type may be passed into the key]
* Set (e.g. `Set<int>()`, `{1, 2, 3}`)

For dynamic typing, the `Any` type may be used. Dynamic typing will be against recommendation, and it is
recommended to check types and cast variables whenever possible, or create multiple function definitions. 
If this is not possible, then use try-catch blocks. The `Any` type is useful when dealing with JSON objects 
from a POST request.

### Generic Types 

Specified in a definition using `<T>` notation. e.g.
```python
class MyList<T>:
   pass
```

### Public/Private Members

Private members start with an underscore `_variable` and are not shown outside of the scope of the 
module/function/class/struct.

When a variable is just an underscore `_`, it will not conflict with the static typing system, and 
will not actually assign any variable. (Used when a variable is not needed).

### Asyncronous Types

Pull-based (on-demand):
* Promise<T>

Push-based (on-recieve / event-based):
* Stream<T>

These will implement functions such as:
* `.get()`
* `.then()` 
* `.catch()`
* `.subscribe()`

### Asyncronous Functions and Generators/Iterators Functions

A function is asyncronous if it contains at least one `await` keyword. The return type must
say: `-> Promise<MyReturnType>`.

A function is a generator if it contains at least one `yield` keyword. The return type must
say: `-> Generator<MyReturnType>`.

> Note: Generator expressions are also possible `(x for x in range(10))`.

If it contains at both `await` and `yield` keywords the return type must say: 
`-> Generator<Promise<MyReturnType>>`. 

> Note: This is different from `-> Stream<MyReturnType>` because generators are **pull-based** data 
> while streams are **push-based** - Generators demand data and streams recieve data. 

The `await` keyword should be used if you want to convert a function into an async function and
the `next()` or `.get()` function should be used if you want to block the current thread.

Operations that do not change the function definition:
* `promise.get()` OR `promise.then(func)`
* `next(generator)` OR `generator.__next__()` OR `for item in stream:`
* `next(stream)` OR `stream.__next__()` OR `for item in stream:` OR `stream.subscribe(func)`

### Keyword Syntaxes

Similar to Python.

Possible loop variants (not really necessary):
```python
# Possible
for (i=0; i<10; i++):
    pass
   
for i from 0 to 9:
    pass

for i in range([10, 20, 30]):
    pass

# Existing
for i in range(0, 10):
    pass
    
for i in range(len([10, 20, 30])):
   pass
   
for i, n in enumerate([10, 20, 30]):
    pass
```

Possible exception handling refactor ('catch' instead of 'except'):
``` 
try:
    pass
catch Exception as e:
    raise e
```

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

## To consider

- Consider using `{}` brackets for code blocks for functions, but indentation `:` for control statements
- Consider adding more type specifiers like `int in (0,10]` for additional compile-time validation
