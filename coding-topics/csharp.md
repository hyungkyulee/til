# C# Language (source : exercism)
## Overview
### What is C#?
a multi-paradigm, statically-typed programming language with object-oriented, declarative, functional, generic, lazy, integrated querying features and type inference.
C# also has features, amongst others, to make programming with multiple threads/processors, parallelisation, asynchrony, unmanaged code in a managed environment and language interoperability easier. It is developed and maintained by Microsoft, who provides the official documentation.

### C#'s characteristics
**Statically-typed **
identifiers have a type set at compile time--like those in Java, C++ or Haskell--instead of holding data of any type like those in Python, Ruby or JavaScript.

**Object-oriented ** 
C# provides imperative class-based objects with features such as single inheritance, interfaces and encapsulation.

**Declarative ** 
programming what is to be done, as opposed to how it is done (a.k.a imperative programming) (which is an implementation detail which can distract from the domain or business logic).

**Functional ** 
functions are first-class data types that can be passed as arguments to and returned from other functions.

**Generic** 
algorithms are written in terms of types to-be-specified-later that are then instantiated, when needed, for the specific types provided as parameters.

**Lazy** (a.k.a "deferred execution") 
the compiler will put off evaluating an item until required. This lets one safely do weird stuff like operating on an infinite list--the language will only create the list up to the last value needed.

**Integrated Querying** 
a language feature called LINQ "Language-Integrated Query", which enables lazy querying directly within the language, not only its own objects but, also, external data sources through formats such as XML, JSON, SQL, NoSQL DBs and event streams.

**Type inference** 
a compiler will often figure out the type of an identifier by itself so you don't have to specify it. Scala and F# both do this.

**Syntax** is similar to that of other C-style languages such as C, C++ and Java.

# .NET Framework
a managed environment within which C# runs, so you get access to the entire .NET ecosystem, including all packages on nuget.org. .NET used to be Windows-only but, with the release of .NET Core -- as well as Mono -- you can also use C# on Mac, Linux or Unix-based systems and on mobile platforms too.
