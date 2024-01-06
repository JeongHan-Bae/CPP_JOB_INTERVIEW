tags: feature
---
# New Features in Modern C++

## **C++ 11**

---

### Initialize Container with `{}`

In C++11, you can initialize containers using curly braces `{}`. This provides a concise and readable way to initialize containers with values.

#### Example:
```cpp
struct Obj {
    std::string name;
    int value;
    bool used;
};
Obj myObj{"My Object", 0, false};

std::vector<int> myVect {1, 2};
std::vector<char> myOtherVect = {'a', 'b', 'c'};
```

---

### Keyword `auto`

The `auto` keyword in C++11 allows for automatic type inference during variable declaration, making code more concise and readable.

#### Example:
```cpp
auto a = 2; // infers int type

std::vector<int> myVect {1, 2, 3, 4};
auto ite = myVect.begin(); // infers iterator type
```

**Rules for using `auto`:**
- Must initialize when using `auto`.
- Cannot be used for function arguments.
- Cannot define arrays using `auto`.
  
  For example, `auto arr[] = "Hello World!"` is not allowed.
  
  But `auto ptr = "Hello World!"` can infer a pointer `char*`.
- Cannot be used for non-static attributes in classes.

---

### Keyword `decltype`

The `decltype` keyword in C++11 is used to deduce the type of an expression at compile time without necessarily evaluating it. This is particularly useful in situations where you want to declare a variable with the same type as another existing variable or expression.

#### Example:
```cpp
int x = 5;
decltype(x) y; // 'y' has the same type as 'x'

std::vector<double> values = {1.0, 2.0, 3.0};
decltype(values)::value_type element = 4.0; // 'element' has the same type as the elements in 'values'
```

**Rules for using `decltype`:**
- The expression inside `decltype` is not evaluated during compilation.
- It can be used with variables, expressions, or even functions.
- It is particularly useful when the type is complex or depends on template parameters.
- It can be combined with other modifiers like `const` or `&` to create references or const references.

#### Example with `const`:
```cpp
const int z = 10;
decltype(z) w = 20; // 'w' has the type 'const int'
```

#### Example with reference:
```cpp
int a = 42;
decltype(a)& b = a; // 'b' is a reference to 'a'
```

Using `decltype` provides a powerful way to create flexible and generic code, especially when dealing with templates or situations where the type is not explicitly known.

---

### Range-Based For Loop

C++11 introduces a concise way to iterate over elements of a container using a range-based for loop.

#### Example:
```cpp
std::vector<int> nums = {1, 2, 3, 4};
// 'auto' here infers int
for (auto num : nums) {
    std::cout << num << std::endl;
}
```

---

### Keyword `nullptr`

The `nullptr` keyword in C++11 is used as a replacement for `NULL` to represent a null pointer, providing better type safety.

#### Example with ListNode:
```cpp
struct ListNode {
    int val;          // Data stored in the node
    ListNode* next;   // Pointer to the next node in the list

    // Constructor to initialize the node with a given value
    ListNode(int value) : val(value), next(nullptr) {}
};
ListNode* p = nullptr;
```

---

### Lambda Expressions

Lambda expressions, introduced in C++11, provide a concise way to define anonymous functions or function objects. They are especially useful when you need a small function for a short period and don't want to write a separate function declaration.

#### Syntax:
```cpp
[ captures ] ( parameters ) -> return_type {
    // function body
}
```

- **Captures:** Specifies variables from the surrounding scope that the lambda function can access. It can capture variables by value or by reference.
  
  Example:
  ```cpp
  int x = 10;
  auto lambda = [x](int y) { return x + y; };
  std::cout << lambda(5) << std::endl; // Outputs 15
  ```

- **Parameters:** Specifies the parameters of the lambda function, similar to regular function parameters.

- **Return Type:** Specifies the return type of the lambda function. If not specified, the return type is inferred.

#### Example:

```cpp
#include <iostream>

int main() {
    // Lambda expression without captures
    auto greet = []() {
        std::cout << "Hello, World!" << std::endl;
    };
    greet(); // Outputs "Hello, World!"

    // Lambda expression with captures and parameters
    int base = 10;
    auto addValue = [base](int x) -> int {
        return base + x;
    };
    std::cout << addValue(5) << std::endl; // Outputs 15

    // Lambda expression with captures by reference
    auto increment = [&base]() {
        base++;
    };
    increment();
    std::cout << base << std::endl; // Outputs 11

    return 0;
}
```

Lambda expressions are versatile and widely used in modern C++ for their ability to create concise and readable code, especially in situations where a small, local function is needed. They are commonly used in algorithms, event handlers, and other scenarios where defining a named function is unnecessary.
