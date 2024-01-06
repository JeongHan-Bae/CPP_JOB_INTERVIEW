tags: const, pointer
---

# Difference between `const pointer` and `pointer const` in C++

In C++, the concepts of `const pointer` and `pointer const` involve the use of the `const` keyword in relation to pointers. Understanding the difference between these two is crucial for writing robust and maintainable C++ code.

## Pros of Using `const`

**In C++, the keyword `const` allows the assignment of a read-only value to a variable, ensuring that its value cannot be changed once initialized.**

1. **Type Check Protection in Functions:**
   The use of `const` in function parameters, such as `func(const obj& input)`, helps enforce that the input object should not be changed within the function. This provides type-checking benefits and enhances code safety.

2. **Facilitates Assignment of Constants:**
   Similar to macros, the `const` keyword facilitates the assignment of several coefficients.
   For example,
   ```cpp
    const int modulo = 10000007;
   ```
   allows for a named constant, improving code readability and maintainability.

4. **Space Saving Compared to Macros:**
   Unlike macros, using `const` for constants, such as
   ```cpp
    result %= modulo;
   ```
   eliminates the need for a separate preset to allocate space for the constant. This can lead to space-saving advantages in the code.

6. **Aids Overloading with the `const` Keyword:**
   The `const` keyword can be utilized in function overloading, allowing for the creation of different versions of a function based on whether the object is `const` or non-`const`. This enhances flexibility and code expressiveness.

## `const pointer`

A `const pointer` in C++ means that the pointer itself is constant and cannot be reassigned to point to a different memory location. However, the data at the memory location the pointer is pointing to can be modified.

Example:
```cpp
int value = 42;
const int *ptr = &value;

// Valid: Modifying the data
value = 50;

// Invalid: Reassigning the pointer
// ptr = &another_value;
```

In this example, `ptr` is a constant pointer to an integer. It cannot be changed to point to a different integer, but the value it points to (`value`) can be modified.

## `pointer const`

On the other hand, a `pointer const` in C++ means that the data the pointer is pointing to is constant and cannot be modified. However, the pointer itself can be reassigned to point to a different memory location.

Example:
```cpp
int value = 42;
int *const ptr = &value;

// Invalid: Modifying the data
// *ptr = 50;

// Valid: Reassigning the pointer
int another_value = 100;
ptr = &another_value;
```

In this example, `ptr` is a constant pointer to an integer. The value it points to (`value`) cannot be modified, but the pointer itself can be changed to point to a different integer.

