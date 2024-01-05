tags: include, format
---

# Difference between `< >` and `""` in C++

In C++, the use of `< >` and `""` has distinct purposes.

## Angle Brackets `< >`

Angle brackets are typically used for including header files in C++.

Example:
```cpp
#include <iostream>
```

This syntax is commonly used for standard C++ library headers.

## Double Quotes `""`

Double quotes are used for including your own header files or files in the same directory.

Example:
```cpp
#include "my_header.h"
```

Using double quotes is appropriate when including your project-specific headers.

Understanding the difference in usage is crucial for proper inclusion of files in C++ programs.
