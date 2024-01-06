tags: format, include
---

# Difference between `<>` and `""` in C++

In C++, the use of `< >` and `" "` has distinct purposes, especially when dealing with standard libraries and project-specific libraries.

## Angle Brackets `< >` for Standard Libraries

The `< >` angle brackets are typically used for including standard libraries. When you use `< >`, the compiler searches for the header files in the system's default paths or paths specified in the environment variable. These headers are part of the standard C++ library and are usually provided by the compiler or the operating system.

Example:
```cpp
#include <iostream>
#include <vector>
```

## Double Quotes `" "` for Project Libraries

On the other hand, double quotes `" "` are used for including project-specific libraries. When you use `" "`, the compiler first searches for the header files in the same directory as the source file. If the headers are not found, it looks in the directories specified in the build settings or makefile. This allows you to organize your project-specific headers in a way that suits your project structure.

Example:
```cpp
#include "my_project_header.h"
#include "folder/another_header.h"
```

## Third-Party Dependencies

For third-party dependencies, the choice between `< >` and `" "` depends on how the library is designed and where its header files are located. Some third-party libraries may follow the standard convention and use `< >`, while others might require you to use `" "`.

It's essential to refer to the documentation of the third-party library you are using to determine the correct inclusion method.

## Compilation Speed and Safety

Choosing correctly between Angle Brackets and Double Quotes can lead to a faster and safer compilation. When using `< >` for standard libraries, the compiler can take advantage of precompiled headers and optimizations provided by the system. On the other hand, using `" "` for project-specific headers allows for a more controlled and predictable inclusion process, reducing the risk of conflicts and improving build times.

It's essential to refer to the documentation of the third-party library you are using and follow best practices to optimize your code's compilation performance.

