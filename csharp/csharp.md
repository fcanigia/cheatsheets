# C# / Dotnet

## Common

### String vs string builder
The string is an immutable type in C#, which means it can’t be changed once it’s been created. StringBuilder, on the other hand, is mutable, which means that if an operation is performed on the string object, it won’t construct a new instance in memory every time, unlike string.

### [Access Modifiers](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)
- *public*: The type or member can be accessed by any other code in the same assembly or another assembly that references it. The accessibility level of public members of a type is controlled by the accessibility level of the type itself.
- *private*: The type or member can be accessed only by code in the same class or struct.
- *protected*: The type or member can be accessed only by code in the same class, or in a class that is derived from that class.
- *internal*: The type or member can be accessed by any code in the same assembly, but not from another assembly. In other words, internal types or members can be accessed from code that is part of the same compilation.
- *protected internal*: The type or member can be accessed by any code in the assembly in which it's declared, or from within a derived class in another assembly.
- *private protected*: The type or member can be accessed by types derived from the class that are declared within its containing assembly.

| Caller's location                      | public | protected internal | protected | internal | private protected | private |
|----------------------------------------|:------:|:------------------:|:---------:|:--------:|:-----------------:|:-------:|
| Within the class                       |    ✔️️   |          ✔️         |     ✔️     |     ✔️    |         ✔️         |    ✔️    |
| Derived class (same assembly)          |    ✔️   |          ✔️         |     ✔️     |     ✔️    |         ✔️         |    ❌    |
| Non-derived class (same assembly)      |    ✔️   |          ✔️         |     ❌     |     ✔️    |         ❌         |    ❌    |
| Derived class (different assembly)     |    ✔️   |          ✔️         |     ✔️     |     ❌    |         ❌         |    ❌    |
| Non-derived class (different assembly) |    ✔️   |          ❌         |     ❌     |     ❌    |         ❌         |    ❌    |

### Override vs overload

### Generics

### Delegates

### Events

### Anon data type vs regular data type

### Diff between a.equals(b) and a == b

### Why .net Core?

### IQueryable vs IEnumerable

### Single/First/FirstOrDefault

### Struct vs class

### Heap and stack

### Action vs function

### Authentication methods

### Property binding

### Value type vs reference type

### Deffered vs immmediate execution

### Hosted services

### Dependency injection

#### Lifecycle

- AddTransient
- AddScoped
- AddSingleton

### Middleware

### Map

### Async/await

### Threads

### Readonly vs constants

### Private function in interface

### Abstract class sealed

### Linq let

## EF

### How does EF prevents sql injection?

### Types of inheritance in EF

## Data types

### Linked list

### Stack

### Array

## Patterns

### Unit of work
### CRQS
### Repository pattern

## API

### Verboses
