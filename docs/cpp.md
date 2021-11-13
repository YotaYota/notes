# C++

Notes from _learncpp.com_.

## Introduction

*Best practice*: Name code files _something.cpp_.

Compiler

- checks _.cpp_ files that they are syntactically correct
- creates machine language object files

Linker

- combines all object files into a single executable program
- links library files
- makes sure all cross file dependencies are resolved properly

*Best practice*: Use compiler flags
`-Werror -Wall -Weffc++ -Wextra -Wsign-conversion`.

## Basics

Every program must have a `main` function.

After a variable has been defined, it can be assigned a value with
*copy assignment*:

```c++
int x;
x = 3;
```

### Initialization

Initialize variables upon creation.

*Copy initialization*:

```c++
int x = 3;
```

Efficient for simple types, but sometimes inefficient for complex types.

*Direct initialization*:

```c++
int x( 3 );
```

Efficient for simple types and tends to be more efficient for complex types.

*List initialization*:

```c++
int x{ 3 };     // direct list initialization
int y = { 5 };  // copy list initialization
int z {};       // value initialization
```

Same as direct, but disallows narrowing conversions and allows initialization
with more types, such as lists.

*Best practice*: use list initialization.

### iostream

*Best practice*: use `\n` instead of `std::endl` since the latter flushes
output.

--- OLD CONTENT ---

## Pass by

### Pass by Value

```c++
void pbv(int x);
```

Pass by value copies parameter. Can be `const`.

- prevents side effects
- can be heavvy on performance
- preffered with fundamental types

### Pass by Reference

```c++
void pbr(int& x);
```

Allows the function to modify the parameter.

Avoid _out parameters_.

### Pass by const Reference

```c++
void pbcr(const int& x);
```

Cannot change parameter (as when passed by value), but the value is not copied,
which saves performance.

### Pass by Address

```c++
void pba(int* x);
```

```c++
void pbca(const int* x);
```

## Pointers and References

References are pointers under the hood.

### Operators

`&`: adress-of operator
`*`: indirection operator / dereference operator

_Note_: Adress-of operator returns a pointer.

_Note_: Adress-of and indirection operator should not be confused with reference
and pointer variable declarations.

### Pointer variables

Variable that holds a memory address as value.

`const int*`: pointer to a `const int`

`int* const`: pointer that itself is `const`

### Reference variables

Reference variable is an alias for an already exisiting variable.

_Note_: Cannot be null.

_Note_: Cannot be changed to refer to another object.

_Note_: Must be initialized at creation.

#### Reference to Non-`const` Value

Can only be initialized with

- non-const l-values.

#### Reference to `const` Value

Can be initialized with

- non-`const` l-values
- `const` l-values
- r-values

#### r-value Reference

Extends the life-time of the r-value.

### Pointers and References as Parameters

Parameters are initialized with the variables passed to the function, so
possible variables are limited to what the type can hold.

Passing by reference or by address avoids copying as is done when passing by
value.

_Note_: Passing by non-const reference has the downside that it is unclear if
the variables will be changed or not.

_Note_: Cannot be assigned to literals or temporaries.

#### Move Semantics

r-values?

### Pointers and References as Return Values

return by adress

```c++
return &variable;
```

_Note_: Be careful not to create dangling pointers.
