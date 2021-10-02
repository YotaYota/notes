# Pass By

## Pass by Value

```
void pbv(int x);
```

Pass by value copies parameter. Can be `const`.

- prevents side effects
- can be heavvy on performance
- preffered with fundamental types

## Pass by Reference

```
void pbr(int& x);
```

Allows the function to modify the parameter.

Avoid _out parameters_.


## Pass by const Reference

```
void pbcr(const int& x);
```

Cannot change parameter (as when passed by value), but the value is not copied,
which saves performance.

## Pass by Address

```
void pba(int* x);
```

```
void pbca(const int* x);
```


# Pointers and References

References are pointers under the hood.


## Operators

`&`: adress-of operator
`*`: indirection operator / dereference operator

_Note_: Adress-of operator returns a pointer.

_Note_: Adress-of and indirection operator should not be confused with reference
and pointer variable declarations.


## Pointer variables

Variable that holds a memory address as value.

`const int*`: pointer to a `const int`

`int* const`: pointer that itself is `const`


## Reference variables

Reference variable is an alias for an already exisiting variable.

_Note_: Cannot be null.

_Note_: Cannot be changed to refer to another object.

_Note_: Must be initialized at creation.

### Reference to Non-`const` Value

Can only be initialized with
- non-const l-values.


### Reference to `const` Value

Can be initialized with
- non-`const` l-values
- `const` l-values
- r-values

### r-value Reference

Extends the life-time of the r-value.


## Pointers and References as Parameters

Parameters are initialized with the variables passed to the function, so
possible variables are limited to what the type can hold.

Passing by reference or by address avoids copying as is done when passing by
value.

_Note_: Passing by non-const reference has the downside that it is unclear if
the variables will be changed or not.

_Note_: Cannot be assigned to literals or temporaries.


### Move Semantics

r-values?


## Pointers and References as Return Values

return by adress
```
return &variable;
```

_Note_: Be careful not to create dangling pointers.

