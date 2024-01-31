tags: std, stl, container
---

# Overview of C++ Standard Library Containers

In addition to the traditional built-in array accessed with square brackets (`[]`), C++ offers a rich collection of containers within the Standard Template Library (**STL**). 
These containers, which are part of the broader C++ Standard Library, provide various data structures with efficient implementations, catering to diverse needs in modern software development. This document explores some of the most commonly used containers available in the C++ STL, highlighting their features, usage, and scenarios where they excel. Let's dive in!

-----

# `<array>`

## `std::array` Container

The `std::array` container is a fixed-size array-like container that encapsulates arrays with a known size at compile time. It provides features similar to built-in arrays but with additional functionalities, such as bounds checking and compatibility with standard algorithms.

### Initialization Behavior:

- **C++98:** In C++98, the values of elements inside a `std::array` are undefined upon creation. There's no automatic initialization mechanism provided, and the elements will contain whatever values happen to be at their memory locations.
  
  If initialization is required, it must be done manually using techniques like looping through elements or using `memset` to set all elements to a specific value.

- **C++11 and later:** In C++11 and later versions, elements of a `std::array` are automatically initialized with default values for their types upon creation. For primitive types like `int`, the default value is `0`.
- For user-defined types like structs, the default constructor (constructor with no parameters) is used to initialize each element.
  
  Additionally, uniform initialization syntax using `{}` can be used to initialize all elements of a `std::array` with the same value. If only n values is provided inside the braces, first n values will be initialized with these values.

### Example (C++98):

```cpp
#include <array>
#include <cstring>

// Create a std::array of integers with size 5 (values are undefined)
std::array<int, 5> intArray;

// Manual initialization using memset to set all elements to 0
std::memset(&intArray[0], 0, intArray.size() * sizeof(int));
```

### Example (C++11 and later):

```cpp
#include <array>

// Create a std::array of integers with size 5 (all elements initialized to 0)
std::array<int, 5> intArray{}; // All elements initialized to 0

// Create a std::array of integers with size 5 (first three elements initialized to -1, -2, -3, rest to 0)
std::array<int, 5> intArrayNegative = {-1, -2, -3}; // Initialized to {-1, -2, -3, 0, 0}

// Create a std::array of user-defined struct (default constructor used for initialization)
struct MyStruct {
    int value;
    // More members if needed
};
std::array<MyStruct, 3> structArray = {}; // All elements initialized with default constructor values
```

## Insertion in `std::array`

Inserting elements into a `std::array` involves creating a new array with a larger size, copying elements from the original array, and inserting the new element at the desired index. 
Here's how you can use the `insertElement` function to insert an element into a `std::array`:

```cpp
std::array<int, 5> originalArray = {1, 2, 3, 4, 5};
int index = 2; // Index to insert the new element
int toInsert = 10; // Value of the new element to insert

// Insert the new element into the original array
auto newArray = insertElement(originalArray, index, toInsert);
```

### Time Complexity Analysis:
- The time complexity of this insertion algorithm is `O(N)` where `N` is the size of the original array. 
- Copying elements before the insertion point and after the insertion point each require `O(N)` time due to the use of `std::swap_ranges`.
- Additionally, inserting the new element at the specified index takes constant time `O(1)`.

### Space Complexity Analysis:
- The space complexity of this insertion algorithm is also `O(N)` where `N` is the size of the original array.
- A new array with a size one larger than the original is created, resulting in additional memory usage proportional to the size of the original array.

### Example of `insertElement` Function:
The provided function `insertElement` takes three parameters: the original `std::array`, the index where the new element should be inserted, and the value of the new element to be inserted.

It creates a new `std::array` with a size one larger than the original, copies elements from the original array before and after the insertion point, and inserts the new element at the specified index.

```cpp
#include <algorithm>
#include <array>
#include <stdexcept>

template <typename T, std::size_t N>
std::array<T, N + 1> insertElement(const std::array<T, N>& originalArray,
                                   std::size_t index, const T& toInsert) {
    // Check if the index is valid, size_t is possitive
    if (index > N) {
        throw std::out_of_range("Index out of bounds");
    }

    // Create a new array with a size larger than the original
    std::array<T, N + 1> newArray{};

    // Copy the elements before the insertion point
    std::swap_ranges(const_cast<T*>(originalArray.begin()),
                     const_cast<T*>(originalArray.begin()) + index,
                     newArray.begin());

    // Insert the new element
    newArray[index] = toInsert;

    // Copy the elements after the insertion point
    std::swap_ranges(const_cast<T*>(originalArray.begin()) + index,
                     const_cast<T*>(originalArray.end()),
                     newArray.begin() + index + 1);

    // Return the new array
    return newArray;
}
```

## Deletion in `std::array`

Deleting elements from a `std::array` involves creating a new array with a smaller size, copying elements from the original array while excluding the element to be deleted. Here's how you can perform deletion using the `deleteElement` function:

```cpp
#include <array>

std::array<int, 5> originalArray = {1, 2, 3, 4, 5};
int index = 2; // Index of the element to delete

// Delete the element at the specified index from the original array
auto newArray = deleteElement(originalArray, index);
```

### Time Complexity Analysis:
- The time complexity of this deletion algorithm is `O(N)` where `N` is the size of the original array. 
- Copying elements before the deletion point and after the deletion point each require `O(N)` time due to the use of `std::swap_ranges`.
- Excluding the element to be deleted takes constant time `O(1)`.

### Space Complexity Analysis:
- The space complexity of this deletion algorithm is also `O(N)` where `N` is the size of the original array.
- A new array with a size one smaller than the original is created, resulting in additional memory usage proportional to the size of the original array.

### Example of `deleteElement` Function:
The provided function `deleteElement` takes two parameters: the original `std::array` and the index of the element to be deleted.

It creates a new `std::array` with a size one smaller than the original, copies elements from the original array before and after the deletion point while excluding the element to be deleted.

```cpp
#include <algorithm>
#include <array>
#include <stdexcept>

template <typename T, std::size_t N>
std::array<T, N - 1> deleteElement(const std::array<T, N>& originalArray,
                                   std::size_t index) {
    // Check if the index is valid
    if (index >= N) {
        throw std::out_of_range("Index out of bounds");
    }

    // Create a new array with a size smaller than the original
    std::array<T, N - 1> newArray{};

    // Copy the elements before the deletion point
    std::swap_ranges(const_cast<T*>(originalArray.begin()),
                     const_cast<T*>(originalArray.begin()) + index,
                     newArray.begin());

    /* Copy the elements after the deletion point, excluding the element to be
     * deleted */
    std::swap_ranges(const_cast<T*>(originalArray.begin()) + index + 1,
                     const_cast<T*>(originalArray.end()),
                     newArray.begin() + index);

    // Return the new array
    return newArray;
}
```

## Modification in `std::array`

Modifying elements in a `std::array` involves updating the value of an existing element at a specified index. This operation can be performed efficiently with a time complexity of `O(1)` by directly assigning the new value to the element or using `std::swap` to exchange values. Here's how you can modify an element in a `std::array`:

#### Direct Assignment:
```cpp
#include <array>

std::array<int, 5> dataArray = {1, 2, 3, 4, 5};
int index = 2; // Index of the element to modify
int newValue = 10; // New value to assign

// Modify the element at the specified index by direct assignment
dataArray[index] = newValue;
```

#### Swap Values:
```cpp
#include <array>
#include <algorithm>

std::array<int, 5> dataArray = {1, 2, 3, 4, 5};
int index = 2; // Index of the element to modify
int newValue = 10; // New value to swap

// Modify the element at the specified index by swapping values
std::swap(dataArray[index], newValue);
```

### Time Complexity Analysis:
- Both methods of modifying elements in a `std::array` have a time complexity of `O(1)`, meaning the time taken to modify an element remains constant regardless of the array size.

### Space Complexity Analysis:
- Modifying elements in a `std::array` does not incur any additional space overhead. The space complexity remains constant, denoted as `O(1)`.

## Searching in `std::array`

Searching for elements in a `std::array` can be efficiently performed using standard algorithms provided by the C++ Standard Library, such as `std::find` and `std::find_end`. These algorithms offer a convenient way to locate elements within the array without resorting to manual iteration or loops.

#### Using `std::find` for Finding First Instance:
```cpp
#include <iostream>
#include <array>
#include <algorithm>

std::array<int, 5> dataArray = {1, 2, 3, 4, 5};
int target = 3; // Element to search for

// Find the first occurrence of the target element in the array
auto iter = std::find(dataArray.begin(), dataArray.end(), target);

// Check if the element was found
if (iter != dataArray.end()) {
    std::cout << "Element found at index: " << std::distance(dataArray.begin(), iter) << std::endl;
} else {
    std::cout << "Element not found" << std::endl;
}
```

#### Using `std::find_end` for Finding Last Instance:
```cpp
#include <iostream>
#include <array>
#include <algorithm>

std::array<int, 5> dataArray = {1, 2, 3, 4, 3};
int target = 3; // Element to search for

// Find the last occurrence of the target element in the array
auto result = std::find_end(dataArray.begin(), dataArray.end(), &target, &target + 1);

// Check if the element was found
if (result != dataArray.end()) {
    std::cout << "Last occurrence found at index: " << std::distance(dataArray.begin(), result) << std::endl;
} else {
    std::cout << "Element not found" << std::endl;
}
```

#### Note:
- `std::find_end` can also find the last occurrence for a sequence.

### Time Complexity Analysis:
- Both `std::find` and `std::find_end` algorithms have a time complexity of `O(N)` where `N` is the size of the `std::array`. 
- This is because they traverse the entire array to search for the element, checking each element sequentially until a match is found or the end of the array is reached.

### Space Complexity Analysis:
- Searching in a `std::array` does not incur any additional space overhead beyond the array itself. The space complexity remains constant, denoted as `O(1)`.

  
## Sorting in `std::array`

By default, elements in a `std::array` are not sorted. However, you can easily sort the elements using the `std::sort` algorithm provided by the C++ Standard Library.
`std::sort` offers an efficient way to arrange the elements of the array in ascending order, with a time complexity of `O(N log N)`, where `N` is the size of the array.

```cpp
#include <array>
#include <algorithm>

std::array<int, 5> dataArray = {5, 2, 8, 1, 9};

// Sort the array in ascending order
std::sort(dataArray.begin(), dataArray.end());

// Now, the elements in dataArray are sorted: {1, 2, 5, 8, 9}
```

- You can apply various sorting algorithms available in the C++ Standard Library to a `std::array`, such as `std::stable_sort` for stable sorting or `std::partial_sort` for partial sorting.

## Comparing `std::array` and Built-in Array (`[]`)

When deciding between `std::array` and the traditional built-in array (`[]`), it's essential to consider factors like initialization, flexibility, safety, usability, and performance. Let's compare them across various aspects:

### Initialization:
- **`std::array`:** Offers convenient initialization methods, such as using `{}` with optional values to initialize all elements or allowing automatic initialization with default values based on the element type.
- **Built-in Array (`[]`):** Initialization can be less intuitive and often requires manual assignment or the use of loop constructs to set initial values.

### Flexibility:
- **`std::array`:** Provides additional functionalities like bounds checking and compatibility with standard algorithms, enhancing the usability and safety of the container.
- **Built-in Array (`[]`):** Offers raw access to memory, providing more flexibility but sacrificing safety features like bounds checking.

### Safety:
- **`std::array`:** Ensures safer code with built-in bounds checking, preventing common errors like buffer overflows or accessing out-of-bounds elements.
- **Built-in Array (`[]`):** Requires manual bounds checking, increasing the risk of errors if not implemented correctly.

### Usability:
- **`std::array`:** Offers a more user-friendly interface with standard library support, making it easier to integrate into modern C++ codebases. Compatibility with standard library algorithms simplifies common tasks.
- **Built-in Array (`[]`):** Requires more manual management and lacks the convenience of standard library features, potentially leading to more verbose code.

### Performance:
- **`std::array`:** Typically offers similar performance to built-in arrays while providing additional safety features and standard library functionalities. The overhead introduced by safety features is usually negligible in most scenarios.
- **Built-in Array (`[]`):** Offers raw performance but lacks safety features and standard library support, requiring more manual optimizations and error handling.

### Conclusion:
- **Use `std::array` when:** Safety, ease of use, and compatibility with standard library algorithms are essential. This is particularly useful in modern C++ codebases where readability, maintainability, and safety are prioritized.
- **Use built-in array (`[]`) when:** Performance is critical, and low-level memory management or compatibility with legacy code is required. However, be prepared to handle manual bounds checking and potential safety issues.

Understanding the specific requirements and trade-offs involved in your project will help you choose the most suitable array type for your needs.

-----

## `std::vector` Container

The `std::vector` container is a dynamic array-like container that provides resizable arrays with automatic memory management. It offers similar functionalities to built-in arrays and `std::array`, but with the added capability of dynamic resizing, making it suitable for scenarios where the size of the array may change frequently.

## Initialization Behavior:

- **Default Initialization:** When a `std::vector` is default-initialized, it contains zero elements.

- **Initialization with Size:** You can initialize a `std::vector` with a specified size, and all elements will be value-initialized, meaning numeric types will be initialized to zero, and class types will be initialized using their default constructor.

- **Initialization with Values:** You can initialize a `std::vector` with a specified size and initial values.

- **Initialization with Size and Value:** You can initialize a `std::vector` with a specified size and initial value for all elements.

### Example:

```cpp
#include <vector>

// Create an empty std::vector
std::vector<int> vec1; // vec1 is empty

// Create a std::vector with a size of 5 (all elements initialized to 0)
std::vector<int> vec2(5); // vec2: {0, 0, 0, 0, 0}

// Create a std::vector with a size of 5 and initial values
std::vector<int> vec3 = {1, 2, 3, 4, 5}; // vec3: {1, 2, 3, 4, 5}

// Create a std::vector with a size of 5 and initial value for all elements
std::vector<int> vec4(5, -1); // vec4: {-1, -1, -1, -1, -1}
```

## Insertion in `std::vector`

Inserting elements into a `std::vector` can be done efficiently at the end of the vector or at a specified position. The vector automatically handles resizing as needed to accommodate new elements.

#### Insert at the End:

```cpp
#include <vector>

std::vector<int> vec = {1, 2, 3};

// Inserting elements at the end
vec.emplace_back(4);
vec.emplace_back(5);
// vec: {1, 2, 3, 4, 5}
```

#### Insert at a Specified Position:

```cpp
std::vector<int> vec = {1, 2, 5};

// Inserting elements at a specified position
vec.insert(vec.begin() + 2, 3); // Insert integer 3 at index 2
vec.insert(vec.begin() + 3, 4); // Insert integer 4 at index 3
// vec: {1, 2, 3, 4, 5}
```

### Time Complexity Analysis:
- Inserting an element at the end of a `std::vector` using `emplace_back()` has an average time complexity of `O(1)`. However, in cases where the vector needs to be resized, the complexity can become `O(n)`, where `n` is the current number of elements in the vector.
- Inserting an element at a specified position using `insert()` has an average time complexity of `O(n)`, where `n` is the number of elements moved due to the insertion.

### Space Complexity Analysis:
- The space complexity of inserting elements into a `std::vector` depends on whether the vector needs to be resized. In the worst case, where resizing is required, the space complexity is `O(n)`.

## Deletion in `std::vector`

Deleting elements from a `std::vector` can be done efficiently using various methods such as `pop_back()` to remove elements from the end or `erase()` to remove elements at a specified position.

#### Delete from the End:

```cpp
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};

// Removing elements from the end
vec.pop_back(); // Removes the last element
// vec: {1, 2, 3, 4}
```

#### Delete from a Specified Position:

```cpp
std::vector<int> vec = {1, 2, 3, 4, 5};

// Removing elements from a specified position
vec.erase(vec.begin() + 2); // Removes the element at index 2
// vec: {1, 2, 4, 5}
```

### Time Complexity Analysis:
- Deleting an element from the end of a `std::vector` using `pop_back()` has a time complexity of `O(1)`.
- Deleting an element from a specified position using `erase()` has a time complexity of `O(n)`, where `n` is the number of elements moved due to shifting.

### Space Complexity Analysis:
- The space complexity of deleting elements from a `std::vector` is `O(1)` as it does not involve resizing the vector.

## Modification in `std::vector`

Modifying elements in a `std::vector` involves updating the value of an existing element at a specified index. This operation can be performed efficiently with a time complexity of `O(1)`.

#### Direct Assignment:

```cpp
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
int index = 2; // Index of the element to modify
int newValue = 10; // New value to assign

// Modify the element at the specified index by direct assignment
vec[index] = newValue;
```

### Time Complexity Analysis:
- Modifying an element in a `std::vector` using direct assignment has a time complexity of `O(1)` as it directly accesses the element at the specified index.

### Space Complexity Analysis:
- Modifying elements in a `std::vector` does not incur any additional space overhead. The space complexity remains constant, denoted as `O(1)`.

## Searching in `std::vector`

Searching for elements in a `std::vector` can be efficiently performed using standard algorithms provided by the C++ Standard Library, such as `std::find` and `std::find_end`.

#### Using `std::find` for Finding First Instance:

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

std::vector<int> vec = {1, 2, 3, 4, 5};
int target = 3; // Element to search for

// Find the first occurrence of the target element in the vector
auto iter = std::find(vec.begin(), vec.end(), target);

// Check if the element was found
if (iter != vec.end()) {
    std::cout << "Element found at index: " << std::distance(vec.begin(), iter) << std::endl;
} else {
    std::cout << "Element not found" << std::endl;
}
```

#### Using `std::find_end` for Finding Last Instance:

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

std::vector<int> vec = {1, 2, 3, 4, 3};
int target = 3; // Element to search for

// Find the last occurrence of the target element in the vector
auto iter = std::find_end(vec.begin(), vec.end(), &target, &target + 1);

// Check if the element was found
if (iter != vec.end()) {
    std::cout << "Last occurrence found at index: " << std::distance(vec.begin(), iter) << std::endl;
} else {
    std::cout << "Element not found" << std::endl;
}
```

### Time Complexity Analysis:
- Searching in a `std::vector` using `std::find` or `std::find_end` has a time complexity of `O(n)` where `n` is the number of elements in the vector. 
- Both algorithms traverse the entire vector to search for the element or condition.

### Space Complexity Analysis:
- Searching in a `std::vector` does not incur any additional space overhead beyond the vector itself. The space complexity remains constant, denoted as `O(1)`.

## Sorting in `std::vector`

Sorting elements in a `std::vector` can be efficiently performed using the `std::sort` algorithm provided by the C++ Standard Library.

#### Example:

```cpp
#include <vector>
#include <algorithm>

std::vector<int> vec = {5, 2, 8, 1, 9};

// Sort the vector in ascending order
std::sort(vec.begin(), vec.end());

// Now, the elements in vec are sorted in ascending order: {1, 2, 5, 8, 9}
```

### Time Complexity Analysis:
- Sorting a `std::vector` using `std::sort` has an average time complexity of `O(n log n)`, where `n` is the number of elements in the vector.

### Note:
- You can choose different sorting orders, such as descending, or provide custom comparison functions to `std::sort` for more specialized sorting requirements.

By utilizing `std::sort`, you can easily organize the elements within a `std::vector` according to your desired criteria.

## Comparing `std::vector` and `std::array`

### Initialization:
- **`std::vector`:** Provides dynamic resizing and automatic memory management, allowing flexible initialization with variable sizes and values.
- **`std::array`:** Offers fixed-size arrays with compile-time known sizes, requiring explicit size specifications during initialization.

### Flexibility:
- **`std::vector`:** Offers dynamic resizing, making it suitable for scenarios where the size of the container may change frequently.
- **`std::array`:** Has a fixed size determined at compile time, providing better performance and memory locality for applications with known size requirements.

### Safety:
- **`std::vector`:** Provides automatic bounds checking during access and resizing, reducing the risk of buffer overflows or memory corruption.
- **`std::array`:** Does not offer bounds checking, relying on the programmer to ensure correct array access within specified bounds.

### Usability:
- **`std::vector`:** Offers a more user-friendly interface with dynamic resizing and standard library support for common operations like insertion, deletion, and sorting.
- **`std::array`:** Requires manual management of array sizes and lacks dynamic resizing, leading to more verbose code in scenarios requiring flexibility.

### Performance:
- **`std::vector`:** Provides efficient dynamic resizing with amortized constant time complexity for adding or removing elements at the end (`emplace_back`, `pop_back`). However, resizing can incur occasional performance overhead.
- **`std::array`:** Offers fixed-size arrays with constant time complexity for element access and modification, providing better performance for scenarios with known size requirements.

### Conclusion:
- **Use `std::vector` when:** Dynamic resizing, automatic memory management, or flexibility in container size are required. This is suitable for scenarios where the size of the container may change during runtime or when dealing with collections of variable size.
- **Use `std::array` when:** Fixed-size arrays with known compile-time sizes, better performance, and memory locality are essential. This is suitable for scenarios where the size of the container is known in advance and does not change during runtime, or when optimal performance is critical.

Understanding the specific requirements and trade-offs involved in your project will help you choose the most suitable container type (`std::vector` or `std::array`) for your needs.

-----

## `std::stack` Container

The `std::stack` container in C++ provides a LIFO (Last-In-First-Out) data structure, where elements are inserted and removed from the same end known as the top. It is implemented as an adapter over other standard container classes, such as `std::deque`, `std::list`, or `std::vector`, with `std::deque` being the default.

### Single-Ended Stack:
`std::stack` operates as a single-ended stack, meaning that elements can only be inserted or removed from one end, which is the top of the stack.

### Initialization:
- **Default Initialization:** When a `std::stack` is default-initialized, it is empty, containing no elements.

### Example:

```cpp
#include <stack>

// Default initialization of a std::stack of integers
std::stack<int> intStack; // Creates an empty stack
```

With this initialization, `intStack` is created as an empty stack ready to hold integers.

The `std::stack` container is particularly useful in scenarios where you need a last-in-first-out (LIFO) data structure, such as managing function call stacks or undo/redo functionality.

## Insertion in `std::stack`

Inserting an element into a specific position in a `std::stack` is not directly supported by its interface.
However, it can be achieved using a custom function, `insertElement`, which creates a temporary stack to hold elements, inserts the new element at the desired position, and then rebuilds the original stack.

### Using Built-In Functions for Insertion at the Top:
When inserting elements into a `std::stack` at the top, the `emplace` function provided by the standard library can be directly used.
This function constructs the element in place at the top of the stack, increasing its size by one.

#### Example:
```cpp
#include <stack>

std::stack<int> intStack;

// Insert elements at the top of the the stack using the emplace function
intStack.emplace(10);
intStack.emplace(20);
```

### Time Complexity Analysis:
- Time Complexity: `O(1)`
  - The time complexity of inserting an element at the top of a `std::stack` using the `emplace` function is constant, regardless of the number of elements in the stack. Since the insertion happens at the top, the time complexity remains constant.

### Space Complexity Analysis:
- Space Complexity: `O(1)`
  - Insertion at the top of a `std::stack` using the `emplace` function does not incur any additional space overhead beyond the stack itself. The space complexity remains constant.

### Custom Function for Insertion at Any Position:
If insertion at a specific position other than the top is required, a custom function `insertElement` can be used. This function rearranges the elements in the stack to insert the new element at the desired position. The time complexity of inserting an element using this custom function is linear with respect to the distance between the insertion position and the top of the stack. At the top, insertion with `emplace` is surely `O(1)`.

### Time Complexity Analysis:
- Time Complexity: `O(n)` for custom insertion, `O(1)` for insertion at the top
  - When using the `insertElement` function for custom insertion, the time complexity is linear with respect to the distance between the insertion position and the top of the stack. However, when inserting at the top with `emplace`, the time complexity remains constant.

### Space Complexity Analysis:
- Space Complexity: `O(n)`
  - The space complexity of the `insertElement` function is linear, as it creates a temporary stack to hold elements while rearranging them. However, insertion at the top with `emplace` does not incur any additional space overhead beyond the stack itself.

### Example of `insertElement` Function:

```cpp
#include <stack>

template <typename T>
void insertElement(std::stack<T>& stack, size_t position, const T& element) {
    if (position > stack.size()) {
        throw std::out_of_range("Index out of bounds");
    }

    std::stack<T> tempStack;
    while (stack.size() > position) {
        tempStack.emplace(stack.top());
        stack.pop();
    }

    stack.emplace(element);

    while (!tempStack.empty()) {
        stack.emplace(tempStack.top());
        tempStack.pop();
    }
}
```

The `insertElement` function takes a `std::stack` reference, the position at which the new element should be inserted, and the value of the new element to be inserted. It rearranges the elements in the stack to insert the new element at the specified position.

## Deletion in `std::stack`

Deleting an element from a specific position in a `std::stack` is not directly supported by its interface.
However, it can be achieved using a custom function, `deleteElement`, which creates a temporary stack to hold elements, removes the element at the desired position, and then rebuilds the original stack.

### Using Built-In Functions for Deletion at the Top:
When deleting elements from a `std::stack` at the top, the `pop` function provided by the standard library can be directly used.
This function removes the top element from the stack, reducing its size by one.

#### Example:
```cpp
#include <stack>

std::stack<int> intStack;

intStack.emplace(10);
intStack.emplace(20);

// Delete elements at the top of the the stack using the pop function
intStack.pop();
```

### Time Complexity Analysis:
- Time Complexity: `O(n)` for custom deletion, `O(1)` for deletion at the top
  - When using the `deleteElement` function for custom deletion, the time complexity is linear, directly proportional to the number of elements in the stack. However, when deleting at the top with `pop`, the time complexity remains constant.

### Space Complexity Analysis:
- Space Complexity: `O(n)`
  - The space complexity of the `deleteElement` function is linear, as it creates a temporary stack to hold elements while rearranging them. However, deletion at the top with `pop` does not incur any additional space overhead beyond the stack itself.
### Example of `deleteElement` Function:

```cpp
#include <stack>
#include <vector>

template <typename T>
void deleteElement(std::stack<T>& stack, size_t position) {
    if (position >= stack.size()) {
        throw std::out_of_range("Index out of bounds");
    }

    std::stack<T> tempStack;
    size_t count = 0;

    while (stack.size() > position) {
        tempStack.emplace(stack.top());
        stack.pop();
    }

    stack.pop();

    while (!tempStack.empty()) {
        stack.emplace(tempStack.top());
        tempStack.pop();
    }
}
```

The `deleteElement` function takes a `std::stack` reference and the position of the element to be deleted. It rearranges the elements in the stack to delete the element at the specified position.

## Modification in `std::stack`

Modifying an element at a specific position in a `std::stack` is not directly supported by its interface.
However, it can be achieved using a custom function, `ModifyElement`, which creates a temporary stack to hold elements, replaces the element at the desired position with a new element, and then rebuilds the original stack.

### Using Built-In Functions for Modification at the Top:
When modifying elements in a `std::stack` at the top, the `pop` and `emplace` functions provided by the standard library can be directly used.
This sequence of operations first removes the top element from the stack and then inserts a new element at the top.

#### Example:
```cpp
#include <stack>

std::stack<int> intStack;
intStack.emplace(10);
intStack.emplace(20);

// Modify elements at the top of the the stack using the pop and emplace functions
intStack.pop();
intStack.emplace(50);
```

### Time Complexity Analysis:
- Time Complexity: `O(n)` for custom modification, `O(1)` for modification at the top
  - When using the `ModifyElement` function for custom modification, the time complexity is linear, directly proportional to the number of elements in the stack. However, modification at the top with `pop` and `emplace` has a constant time complexity.

### Space Complexity Analysis:
- Space Complexity: `O(n)`
  - The space complexity of the `ModifyElement` function is linear, as it creates a temporary stack to hold elements while rearranging them. However, modification at the top with `pop` and `emplace` does not incur any additional space overhead beyond the stack itself.

### Example of `ModifyElement` Function:

```cpp
#include <stack>

template <typename T>
void ModifyElement(std::stack<T>& stack, size_t position, const T& element) {
    if (position >= stack.size()) {
        throw std::out_of_range("Index out of bounds");
    }

    std::stack<T> tempStack;

    // Move elements from the original stack to tempStack until the target position
    while (stack.size() > position) {
        tempStack.emplace(stack.top()); // Emplace the top element of stack into tempStack
        stack.pop(); // Remove the top element from stack
    }

    stack.pop(); // Delete the element at the target position
    stack.emplace(element); // Replace the element at the target position with the new value

    // Move elements back from tempStack to the original stack
    while (!tempStack.empty()) {
        stack.emplace(tempStack.top()); // Emplace the top element of tempStack into stack
        tempStack.pop(); // Remove the top element from tempStack
    }
}
```

This function effectively modifies the element at the specified position within the `std::stack`, ensuring that the stack's LIFO (Last-In-First-Out) property is maintained.

## Searching in `std::stack`

Searching for an element at a specific position in a `std::stack` is not directly supported by its interface. However, you can create a custom function, `searchElement`, which creates a temporary stack to hold elements while searching for the desired element at the specified position.

### Time Complexity Analysis:
- Time Complexity: `O(n)`
  - The time complexity of searching for an element at a specific position in a `std::stack` using the `searchElement` function is linear, directly proportional to the number of elements in the stack.

### Space Complexity Analysis:
- Space Complexity: `O(n)`
  - The space complexity of this function is linear, as it creates a temporary stack to hold elements while searching for the desired element.

### Example of `searchElement` Function:

```cpp
#include <stack>

template <typename T>
T searchElement(const std::stack<T>& stack, size_t position, bool& found) {
    if (position >= stack.size()) {
        throw std::out_of_range("Index out of bounds");
    }

    found = false;
    std::stack<T> tempStack = stack;
    T result{}; // initialize
    size_t count = 0;
    while (count != position) {
        tempStack.pop();
        ++count;
    }
    result = tempStack.top();

    found = true;
    return result;
}
```

## Sorting in `std::stack`

Sorting elements in a `std::stack` involves transferring elements to another container, such as a `std::vector`, sorting them, and then pushing them back onto the stack.

### Time Complexity Analysis:
- Time Complexity: O(n log n)
  - The time complexity of sorting elements in a `std::stack` involves sorting a vector of elements, which has a time complexity of O(n log n), where n is the number of elements in the stack.

### Space Complexity Analysis:
- Space Complexity: `O(n)`
  - The space complexity of this operation is linear, as it creates a temporary vector to hold elements while sorting.

### Example of Sorting:

```cpp
#include <algorithm>
#include <stack>
#include <vector>

template <typename T> void sortStack(std::stack<T>& stack) {
    std::vector<T> tempVector;
    while (!stack.empty()) {
        tempVector.push_back(stack.top());
        stack.pop();
    }

    std::sort(tempVector.begin(), tempVector.end());

    for (const auto& element : tempVector) {
        stack.push(element);
    }
}
```

To apply sorting on a `std::stack`, you can use the `sortStack` function as follows:

```cpp
std::stack<int> intStack;
// Assume intStack contains elements

sortStack(intStack); // Sort the elements in the stack

```

This function first transfers elements from the stack to a temporary vector, sorts the vector, and then pushes the sorted elements back onto the stack.


## Comparing `std::stack` and `ListStack`

When comparing `std::stack` from the C++ Standard Library with the hand-made `ListStack` implementation, it's essential to evaluate their features, performance, and suitability for various use cases. Let's examine both implementations across different aspects:

### Implementation:
- **`std::stack`:** Provided by the C++ Standard Library as a container adapter, `std::stack` internally uses other standard containers (e.g., `std::deque`, `std::list`, `std::vector`) to manage elements in a LIFO (Last-In-First-Out) structure.
- **`ListStack`:** A custom implementation using a singly linked list, with each node containing a value and a pointer to the next node. It directly manages memory allocation and deallocation for each element.

### Usability:
- **`std::stack`:** Offers a simple and intuitive interface for stack operations, with methods like `emplace`, `pop`, and `top`. It's part of the standard library and seamlessly integrates with other container types and algorithms.
- **`ListStack`:** Requires manual memory management and linked list operations, making it more complex to use compared to `std::stack`. Users need to handle memory allocation, deallocation, and traversal explicitly.

### Performance:
- **`std::stack`:** Provides efficient performance for stack operations, leveraging optimizations from the standard library implementation and underlying container types.
- **`ListStack`:** Performance might vary depending on the efficiency of linked list operations and memory management. Directly managing memory can introduce overhead compared to the optimized implementations in standard library containers.

### Safety:
- **`std::stack`:** Offers safety features such as bounds checking and exception handling, ensuring robustness and preventing common errors.
- **`ListStack`:** Requires careful handling of memory allocation and deallocation to avoid memory leaks and undefined behavior. It lacks built-in safety features provided by standard library containers.

### Flexibility:
- **`std::stack`:** Provides flexibility in choosing the underlying container type, allowing users to tailor the implementation based on performance and memory requirements.
- **`ListStack`:** Offers flexibility in implementation details and customization but requires more manual effort to achieve desired functionalities compared to `std::stack`.

### Conclusion:
- **Use `std::stack` when:** 
  - Standard stack operations are sufficient.
  - Safety, ease of use, and compatibility with other standard library components are priorities.
  - Performance is critical and optimized standard library implementations are preferred.

- **Consider `ListStack` when:** 
  - Custom memory management or linked list operations are required.
  - Specific customization beyond what `std::stack` provides is necessary.
  - Performance characteristics of a custom implementation are acceptable compared to standard library alternatives.

Understanding the trade-offs between simplicity, performance, and safety will help in selecting the appropriate stack implementation for your project.

### `ListStack` Implementation:
```cpp
#include <iostream>
#include <stdexcept>

template<typename T>
struct ListNode {
    T val;
    ListNode* next;

    explicit ListNode(const T& value) : val(value), next(nullptr) {}
};

template<typename T>
struct ListStack {
    ListNode<T>* top_ele;
    size_t length;

    ListStack() : top_ele(nullptr), length(0) {}

    void emplace(const T& value) {
        auto* new_node = new ListNode<T>(value);
        new_node->next = top_ele;
        top_ele = new_node;
        length++;
    }

    [[nodiscard]] T top() const {
        if (empty()) {
            throw std::runtime_error("Stack is empty");
        }
        return top_ele->val;
    }

    [[nodiscard]] bool empty() const {
        return length == 0;
    }

    void pop() {
        if (empty()) {
            throw std::runtime_error("Stack is empty");
        }
        ListNode<T>* dummy = top_ele;
        top_ele = top_ele->next;
        delete dummy;
        length--;
    }

    ~ListStack() {
        while (top_ele != nullptr) {
            auto next_node = top_ele->next;
            delete top_ele;
            top_ele = next_node;
        }
    }
};
```

This `ListStack` implementation offers basic stack operations using a singly linked list structure. Users should handle memory management and linked list traversal manually when using this implementation.


## Comparing `std::stack` and `std::vector` for LIFO Behavior

When considering a LIFO (Last-In-First-Out) data structure in C++, both `std::stack` and `std::vector` can serve the purpose. 
However, they differ in their implementations, features, and performance characteristics. Let's compare them across various aspects:

### Implementation:
- **`std::stack`:** It's a container adapter provided by the C++ Standard Library. It uses other standard containers (e.g., `std::deque`, `std::list`, `std::vector`) internally to implement the stack behavior. The default underlying container is `std::deque`.
- **`std::vector`:** It's a dynamic array that can resize itself automatically when needed. While it's not specifically designed as a stack, it can be used to implement LIFO behavior by pushing elements to the back and popping them from the back.

### Usability:
- **`std::stack`:** Offers a specialized interface tailored for stack operations, including methods like `push`/`emplace`, `pop`, and `top`. It abstracts away the underlying container details, providing a clean and intuitive stack interface.
- **`std::vector`:** Provides a more general-purpose interface for dynamic arrays. While it offers similar operations like `push_back`/`emplace_back` and `pop_back`, users need to manage the resizing and reallocation of memory explicitly.

### Performance:
- **`std::stack`:** Offers efficient performance for stack operations, leveraging optimizations from the underlying container implementations. It's designed specifically for LIFO behavior, resulting in optimized performance for stack-related operations.
- **`std::vector`:** Provides good performance for LIFO behavior but might incur overhead during resizing operations when the capacity is exceeded. While it's versatile and can be used for various purposes beyond a stack, its performance might not be as specialized as `std::stack`.

### Safety:
- **`std::stack`:** Ensures safety features such as bounds checking and exception handling, providing robustness and preventing common errors.
- **`std::vector`:** Offers safety features like bounds checking during element access. However, users need to ensure proper memory management and avoid out-of-bounds access, especially when resizing the vector.

### Flexibility:
- **`std::stack`:** Provides flexibility in choosing the underlying container type, allowing users to tailor the implementation based on performance and memory requirements.
- **`std::vector`:** Offers flexibility as a dynamic array that can be used for various data storage needs beyond LIFO behavior. Users can control the capacity and growth strategy to optimize performance for specific use cases.

### Conclusion:
- **Use `std::stack` when:** 
  - Specialized stack behavior with a clean and intuitive interface is required.
  - Safety, ease of use, and compatibility with other standard library components are priorities.
  - Performance optimizations specific to stack operations are necessary.

- **Consider `std::vector` when:** 
  - Versatility and flexibility beyond stack behavior are desired.
  - Performance characteristics of a dynamic array suit the application's requirements.
  - Users are comfortable managing memory and resizing operations explicitly.

Both `std::stack` and `std::vector` offer LIFO behavior, but the choice depends on the specific requirements, performance considerations, and ease of use within the context of the application.

-----

# `<deque>`

## `std::deque` Container

The `<deque>` (double-ended queue) container in C++ provides a dynamic array-like data structure that supports efficient insertion and deletion at both ends. It allows for constant time insertion and deletion at either end of the sequence, making it suitable for scenarios where elements need to be efficiently added or removed from the front or back of the container.

### Initialization:

`std::deque` can be initialized in several ways:

- **Default Initialization:** A default-initialized `std::deque` is empty, containing no elements.

- **Initialization with Size:** You can initialize a `std::deque` with a specified size, creating a container with a certain number of default-initialized elements.

- **Initialization with Range:** You can initialize a `std::deque` with elements from a specified range, such as another container or an initializer list.

### Example:

```cpp
#include <deque>

// Default initialization of a std::deque
std::deque<int> deque1; // Creates an empty deque

// Initialization with size
std::deque<int> deque2(5); // Creates a deque with 5 default-initialized elements

// Initialization with range
std::deque<int> deque3 {1, 2, 3, 4, 5}; // Creates a deque with elements from the initializer list
```

## Insertion in `std::deque`

### Using Built-In Functions for Insertion at the Front and Back:
- `emplace_front` for insertion at the front with a time complexity of O(1):
  ```cpp
  myDeque.emplace_front(10);
  ```

- `emplace_back` for insertion at the back with a time complexity of O(1):
  ```cpp
  myDeque.emplace_back(50);
  ```

### Using `insert` for Custom Position Insertion:
- `insert` for insertion at a custom position with a time complexity of O(n):
  ```cpp
  myDeque.insert(myDeque.begin() + 2, 25);
  ```
  
### Time Complexity Analysis:
- Time Complexity: `O(1)` for insertion at the front and back, `O(n)` for insertion at a custom position.
  - Insertion at the front and back using `emplace_front` and `emplace_back` functions has a constant time complexity because these operations directly add elements to the respective ends of the deque in constant time.
  - However, insertion at a custom position using the `insert` function requires shifting elements to accommodate the new element, resulting in a linear time complexity proportional to the number of elements in the deque.

### Space Complexity Analysis:
- Space Complexity: `O(1)` for insertion at the front and back, `O(n)` for insertion at a custom position.
  - Insertion at the front and back using `emplace_front` and `emplace_back` functions does not incur any additional space overhead beyond the deque itself. It simply adds elements to the beginning or end of the deque.
  - Insertion at a custom position using the `insert` function may require temporary space to hold elements while rearranging them, resulting in a space complexity linearly proportional to the number of elements in the deque. Therefore, the space complexity for custom position insertion is O(n).

## Deletion in `std::deque`

### Using Built-In Functions for Deletion at the Front and Back:
- `pop_front` for deletion at the front with a time complexity of O(1):
  ```cpp
  myDeque.pop_front();
  ```

- `pop_back` for deletion at the back with a time complexity of O(1):
  ```cpp
  myDeque.pop_back();
  ```

### Using `erase` for Custom Position Deletion:
- `erase` for deletion at a custom position with a time complexity of O(n):
  ```cpp
  myDeque.erase(myDeque.begin() + 2);
  ```

### Time Complexity Analysis:
- Time Complexity: `O(n)` for custom deletion, `O(1)` for deletion at the front and back.
  - When using `erase` for custom deletion, the time complexity is linear, directly proportional to the number of elements in the deque. However, deletion at the front and back with `pop_front` and `pop_back` has a constant time complexity.

### Space Complexity Analysis:
- Space Complexity: `O(1)` for deletion at the front and back, `O(n)` for custom deletion.
  - The space complexity of `erase` for custom deletion is linear, as it may create temporary space to hold elements while rearranging them. However, deletion at the front and back with `pop_front` and `pop_back` does not incur additional space overhead beyond the deque itself.
## Performance Characteristics

`std::deque` provides efficient performance for insertion and deletion at both ends of the sequence, typically with constant time complexity (`O(1)`). This makes it suitable for scenarios where frequent insertion and deletion operations are required at the front and back of the container.

### Modification in `std::deque`

Modifying an element in a `std::deque` can be efficiently done using the subscript operator `[]` or the `at()` function to access and modify elements directly.

#### Using Built-In Functions for Modification at the Front and Back:
- Modifying elements at the front and back of a `std::deque` can be achieved using `emplace_front()` and `emplace_back()` functions, respectively. These operations have a time complexity of `O(1)`.

#### Example:
```cpp
std::deque<int> myDeque = {10, 20, 30, 40, 50};

// Modify the first and last elements
myDeque.front() = 5;
myDeque.back() = 55;
```

#### Using Built-In Functions for Modification at Custom Position:
- Modification at a custom position is done using the subscript operator `[]` or the `at()` function to access the element at the desired position and then assigning the new value. This operation has a time complexity of `O(1)`.

#### Example:
```cpp
std::deque<int> myDeque = {10, 20, 30, 40, 50};

// Modify the element at index 2
myDeque[2] = 25;
```

### Time Complexity Analysis:
- Time Complexity: `O(1)` for modification at the front and back, `O(1)` for modification at a custom position.
  - Modifying elements at the front and back using `emplace_front()` and `emplace_back()` functions has a constant time complexity because these operations directly access and modify the respective ends of the deque in constant time.
  - Similarly, modifying an element at a custom position using the subscript operator `[]` or the `at()` function also has a constant time complexity, as it directly accesses and modifies the element at the specified position.

### Space Complexity Analysis:
- Space Complexity: `O(1)`
  - Modification operations in a `std::deque` do not incur any additional space overhead beyond the deque itself. Therefore, the space complexity remains constant.
 
### Searching in `std::deque`

Similar to `std::vector`, searching for elements in a `std::deque` can also be efficiently performed using standard algorithms provided by the C++ Standard Library, such as `std::find` and `std::find_end`.

#### Using `std::find` for Finding First Instance:

```cpp
#include <deque>
#include <algorithm>

std::deque<int> myDeque = {10, 20, 30, 40, 50};
int target = 30; // Element to search for

// Find the first occurrence of the target element in the deque
auto iter = std::find(myDeque.begin(), myDeque.end(), target);

// Check if the element was found
if (iter != myDeque.end()) {
    // Element found
} else {
    // Element not found
}
```

### Time Complexity Analysis:
- The time complexity of searching in a `std::deque` using `std::find` is `O(n)`, where `n` is the number of elements in the deque. 
- The `std::find` algorithm linearly searches through the elements of the `std::deque` until it finds the desired value or reaches the end.

### Space Complexity Analysis:
- Searching in a `std::deque` using `std::find` does not incur any additional space overhead beyond the iterator used for traversal. Therefore, the space complexity remains constant, denoted as `O(1)`.


### Sorting in `std::deque`

Sorting elements in a `std::deque` can also be efficiently performed using the `std::sort` algorithm provided by the C++ Standard Library.

```cpp
#include <deque>
#include <algorithm>

std::deque<int> myDeque = {5, 2, 8, 1, 9};

// Sort the deque in ascending order
std::sort(myDeque.begin(), myDeque.end());

// Now, the elements in myDeque are sorted: {1, 2, 5, 8, 9}
```

- Similar to `std::vector`, you can apply various sorting algorithms available in the C++ Standard Library to a `std::deque`, providing flexibility and ease of use.

### Comparing `std::deque` and `std::stack` for LIFO Behavior

When considering a LIFO (Last-In-First-Out) data structure in C++, both `std::deque` (double-ended queue) and `std::stack` can serve the purpose. However, they differ in their implementations, features, and performance characteristics. Let's compare them across various aspects:

### Implementation:
- **`std::deque`:** It's a versatile container that provides efficient insertion and deletion at both ends. It's implemented as a sequence of individual blocks, allowing fast insertions and deletions.
- **`std::stack`:** It's a container adapter provided by the C++ Standard Library, typically using other standard containers like `std::deque`, `std::list`, or `std::vector` internally. The default underlying container is `std::deque`, but users can choose other containers based on their requirements.

### Usability:
- **`std::deque`:** Offers a general-purpose interface for double-ended queue operations, including methods like `push_front`/`emplace_front`, `push_back`/`emplace_back`, `pop_front`, and `pop_back`. It provides flexibility for various data storage needs beyond LIFO behavior.
- **`std::stack`:** Provides a specialized interface tailored specifically for stack operations, abstracting away the underlying container details. It offers methods like `push`/`emplace`, `pop`, and `top`, making it easier to work with LIFO behavior.

### Performance:
- **`std::deque`:** Offers efficient performance for LIFO behavior due to its optimized implementation for insertion and deletion at both ends. It provides constant-time complexity for `push_front`/`emplace_front`, `push_back`/`emplace_back`, `pop_front`, and `pop_back` operations.
- **`std::stack`:** Provides efficient performance for stack operations, leveraging optimizations from the underlying container implementation (e.g., `std::deque`). However, the choice of the underlying container can impact performance.

### Safety:
- **`std::deque`:** Ensures safety features such as bounds checking and exception handling during element access and modification, providing robustness and preventing common errors.
- **`std::stack`:** Offers safety features inherited from the underlying container type (e.g., `std::deque`), ensuring safe stack operations. Users need to handle exceptions and ensure proper error handling.

### Flexibility:
- **`std::deque`:** Provides flexibility for various data storage needs beyond LIFO behavior, supporting random access and efficient insertion/deletion at both ends. Users can choose different container adapters based on performance and memory requirements.
- **`std::stack`:** Offers a specialized interface for stack operations, limiting flexibility to LIFO behavior. However, users can choose the underlying container type (e.g., `std::deque`, `std::list`, `std::vector`) to tailor the implementation based on specific requirements.

### Conclusion:
- **Use `std::deque` when:** 
  - Versatility and efficiency for double-ended queue operations are required.
  - Constant-time complexity for insertion and deletion at both ends is crucial.
  - Random access and flexibility for various data storage needs are desired.

- **Use `std::stack` when:** 
  - Specialized stack behavior with a clean and intuitive interface is needed.
  - Performance optimizations specific to stack operations are necessary.
  - Compatibility with other standard library components and ease of use are priorities.

Both `std::deque` and `std::stack` offer LIFO behavior, but the choice depends on the specific requirements, performance considerations, and flexibility within the context of the application.

### Comparing `std::deque` and `std::vector`

When comparing `std::deque` (double-ended queue) and `std::vector` in C++, they both serve as sequence containers, but they differ in their characteristics, features, and usage scenarios. Let's compare them across various aspects:

### Initialization:
- **`std::deque`:** Offers dynamic resizing and efficient insertion and deletion at both ends, allowing flexible initialization with variable sizes and values.
- **`std::vector`:** Provides dynamic resizing with automatic memory management, enabling flexible initialization with variable sizes and values, similar to `std::deque`.

### Flexibility:
- **`std::deque`:** Provides efficient insertion and deletion at both ends, making it suitable for scenarios where elements are frequently added or removed from the front or back of the container.
- **`std::vector`:** Offers dynamic resizing and efficient insertion and deletion at the end (`emplace_back`, `pop_back`), but less efficient for operations involving elements at the front due to potential reallocation of memory.

### Safety:
- **`std::deque`:** Ensures safety features such as bounds checking during access and modification, reducing the risk of buffer overflows or memory corruption.
- **`std::vector`:** Provides automatic bounds checking during access and resizing, enhancing safety by preventing out-of-bounds access or memory corruption.

### Usability:
- **`std::deque`:** Offers a general-purpose interface for double-ended queue operations, including methods like `push_front`/`emplace_front`, `push_back`/`emplace_back`, `pop_front`, and `pop_back`.
- **`std::vector`:** Provides a more user-friendly interface with standard library support for common operations like insertion, deletion, sorting, and dynamic resizing (`emplace_back`, `pop_back`).

### Performance:
- **`std::deque`:** Provides efficient performance for insertion and deletion at both ends with constant time complexity (`O(1)`), making it suitable for scenarios where elements are frequently added or removed from the front or back.
- **`std::vector`:** Offers efficient performance for dynamic resizing with amortized constant time complexity (`O(1)`) for adding or removing elements at the end (`emplace_back`, `pop_back`). However, reallocation of memory during resizing can incur occasional performance overhead.

### Conclusion:
- **Use `std::deque` when:** Efficient insertion and deletion at both ends (`push_front`/`emplace_front`, `pop_front`, `push_back`/`emplace_back`, `pop_back`) are required, or when flexibility in container size and efficient dynamic resizing are essential.
- **Use `std::vector` when:** Dynamic resizing, automatic memory management, or a more user-friendly interface with support for common operations like insertion, deletion, sorting, and dynamic resizing (`emplace_back`, `pop_back`) are needed, and performance is optimized for operations at the end of the container.

Understanding the specific requirements and performance characteristics of your application will help you choose the most suitable container type (`std::deque` or `std::vector`) for your needs.

-----

# `<unordered_map>` and `<unordered_set>`

## `std::unordered_map` and `std::unordered_set` Containers

The `std::unordered_map` and `std::unordered_set` containers are part of the C++ Standard Library and provide implementations of hash tables for efficient storage and retrieval of elements. They are commonly used when rapid access to elements based on keys or values is required. While `std::unordered_map` stores key-value pairs with unique keys, `std::unordered_set` stores unique elements without any associated values.

### Initialization Behavior:

- **Default Initialization:** When default-initialized, both `std::unordered_map` and `std::unordered_set` are empty containers with no elements.
  
- **Initialization with Buckets:** You can initialize `std::unordered_map` and `std::unordered_set` with a specific number of initial buckets to optimize memory usage and reduce the number of rehashes.

### Example:

```cpp
#include <unordered_map>
#include <unordered_set>

// Default initialization
std::unordered_map<int, std::string> myMap; // Empty unordered_map
std::unordered_set<int> mySet; // Empty unordered_set

// Initialization with specific number of buckets
std::unordered_map<int, std::string> myMapWithBuckets(10); // unordered_map with 10 initial buckets
std::unordered_set<int> mySetWithBuckets(20); // unordered_set with 20 initial buckets
```

### Insertion in `std::unordered_map` and `std::unordered_set`

Inserting elements into `std::unordered_map` and `std::unordered_set` can be done efficiently using the `insert()` function or the `emplace()` function for individual elements.

#### Inserting into `std::unordered_map`:

```cpp
std::unordered_map<int, std::string> myMap;

// Inserting key-value pairs into unordered_map
myMap.insert({1, "first"});
myMap.emplace(2, "second");
```

#### Inserting into `std::unordered_set`:

```cpp
std::unordered_set<std::string> mySet;

// Inserting elements into unordered_set
mySet.insert("first");
mySet.emplace("second");
```

### Time Complexity Analysis:
- Both `insert()` and `emplace()` operations have an average time complexity of `O(1)` for insertion into `std::unordered_map` and `std::unordered_set`.
- However, in cases where rehashing is required due to increased load factor, the complexity can become `O(n)`, where `n` is the number of elements in the container.

### Space Complexity Analysis:
- The space complexity of inserting elements into `std::unordered_map` and `std::unordered_set` is proportional to the number of elements inserted, with no additional overhead beyond the container itself.

### Deletion in `std::unordered_map` and `std::unordered_set`

Deleting elements from `std::unordered_map` and `std::unordered_set` can be efficiently done using the `erase()` function.

#### Deleting from `std::unordered_map`:

```cpp
std::unordered_map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Deleting elements from unordered_map
myMap.erase(1); // Deletes the element with key 1
```

#### Deleting from `std::unordered_set`:

```cpp
std::unordered_set<std::string> mySet = {"first", "second"};

// Deleting elements from unordered_set
mySet.erase("first"); // Deletes the element "first"
```

### Time Complexity Analysis:
- The `erase()` operation for both `std::unordered_map` and `std::unordered_set` has an average time complexity of `O(1)` for deletion.
- In cases where rehashing is required due to decreased load factor, the complexity can become `O(n)`, where `n` is the number of elements in the container.

### Space Complexity Analysis:
- The space complexity of deleting elements from `std::unordered_map` and `std::unordered_set` is `O(1)` as it does not involve additional memory overhead beyond the container itself.

### Modification in `std::unordered_map` and `std::unordered_set`

Modifying elements in `std::unordered_map` involves updating the value associated with a specific key, while in `std::unordered_set`, it involves removing the existing element and inserting a new one with the updated value.

#### Modifying `std::unordered_map`:

```cpp
std::unordered_map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Modifying value associated with key 2
myMap[2] = "new value"; // Update value associated with key 2
```

#### Modifying `std::unordered_set`:

```cpp
std::unordered_set<std::string> mySet = {"first", "second"};

// Modifying element in unordered_set
mySet.erase("first"); // Remove existing element
mySet.insert("new value"); // Insert new element with updated value
```

### Time Complexity Analysis:
- Modifying elements in `std::unordered_map` and `std::unordered_set` has an average time complexity of `O(1)`.

### Space Complexity Analysis:
- The space complexity of modifying elements in `std::unordered_map` and `std::unordered_set` is `O(1)` as it does not involve additional memory overhead beyond the container itself.

### Searching in `std::unordered_map` and `std::unordered_set`

Searching for elements in `std::unordered_map` and `std::unordered_set` can be efficiently performed using the `find()` function.

#### Searching in `std::unordered_map`:

```cpp
std::unordered_map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Searching for key 2 in unordered_map
auto iter = myMap.find(2);
if (iter != myMap.end()) {
    std::cout << "Key 2 found, associated value: " << iter->second << std::endl;
} else {
    std::cout << "Key 2 not found" << std::endl;
}
```

#### Searching in `std::unordered_set`:

```cpp
std::unordered_set<std::string> mySet = {"first", "second"};

// Searching for element "second" in unordered_set
auto iter = mySet.find("second");
if (iter != mySet.end()) {
    std::cout << "Element 'second' found" << std::endl;
} else {
    std::cout << "Element 'second' not found" << std::endl;
}
```

### Time Complexity Analysis:
- The `find()` operation for both `std::unordered_map` and `std::unordered_set` has an average time complexity of `O(1)` for searching.

### Space Complexity Analysis:
- Searching in `std::unordered_map` and `std::unordered_set` does not incur any additional space overhead beyond the container itself. The space complexity remains constant, denoted as `O(1)`.

### Conclusion:

- **Use `std::unordered_map` when:** Storing key-value pairs where rapid access to elements based on keys is required, and the order of elements is not significant.
- **Use `std::unordered_set` when:** Storing unique elements where rapid access and membership testing are essential, and the order of elements is not significant.
- Both containers offer efficient insertion, deletion, modification, and searching operations with average constant-time complexity, making them suitable for various applications where hash-based data structures are needed.
- However, keep in mind that hash-based containers may have slightly higher memory overhead compared to ordered containers like `std::map` or `std::set`.

## Comparing `std::unordered_map`/ `std::unordered_set` and `std::vector<std::pair<T1, T2>>` / `std::vector<T>`

When deciding between `std::unordered_map`/`std::unordered_set` and `std::vector<std::pair<T1, T2>>`/`std::vector<T>`, it's essential to consider factors like efficiency, memory usage, and the nature of the data being stored.

### Lookup and Insertion Time:

- **`std::unordered_map` and `std::unordered_set`:** Offer constant-time complexity `O(1)` for average-case lookup and insertion operations, leveraging hash functions for efficient access.
- **`std::vector<std::pair<T1, T2>>` and `std::vector<T>`:** Require linear-time complexity `O(n)` for searching and insertion, with potential resizing overhead.

### Memory Overhead:

- **`std::unordered_map` and `std::unordered_set`:** May incur more memory overhead due to hash tables and buckets, but offer efficient access and management of elements.
- **`std::vector<std::pair<T1, T2>>` and `std::vector<T>`:** Generally have lower memory overhead, but may become inefficient for large datasets due to linear search.

### Ordering:

- **`std::unordered_map` and `std::unordered_set`:** Do not maintain any specific ordering of elements, organizing them based on hash functions for rapid access.
- **`std::vector<std::pair<T1, T2>>` and `std::vector<T>`:** Maintain the order of elements as they are inserted, providing sequential access.

### Use Cases:

- **`std::unordered_map` and `std::unordered_set`:** Ideal for scenarios requiring rapid access to elements based on keys or values, without the need for maintaining order.
- **`std::vector<std::pair<T1, T2>>` and `std::vector<T>`:** Suitable when sequential access to elements or minimizing memory overhead is important, albeit at the expense of lookup performance.

### Conclusion:

- **Use `std::unordered_map` and `std::unordered_set` when:** Swift access to elements based on keys or values is essential, and the order of elements is not significant.
- **Use `std::vector<std::pair<T1, T2>>` and `std::vector<T>` when:** Maintaining the order of elements or minimizing memory overhead is important, at the expense of lookup performance.

-----

# `<map>` and `<set>`

## `std::map` and `std::set` Containers

The `std::map` and `std::set` containers are part of the C++ Standard Library and provide implementations of balanced binary search trees (usually red-black trees) for efficient storage and retrieval of elements. They are commonly used when elements need to be ordered and when rapid access to elements based on keys or values is required. While `std::map` stores key-value pairs with unique keys sorted by keys, `std::set` stores unique elements sorted by values without any associated values.

### Initialization Behavior:

- **Default Initialization:** When default-initialized, both `std::map` and `std::set` are empty containers with no elements.
  
- **Initialization with Comparator:** You can initialize `std::map` and `std::set` with a specific comparison function or object to customize the ordering of elements.

### Example:

```cpp
#include <map>
#include <set>

// Default initialization
std::map<int, std::string> myMap; // Empty map
std::set<int> mySet; // Empty set

// Initialization with custom comparison function
auto cmp = [](int a, int b) { return a > b; }; // Custom comparison function
std::map<int, std::string, decltype(cmp)> myCustomMap(cmp); // Map with custom comparison
```

### Insertion in `std::map` and `std::set`

Inserting elements into `std::map` and `std::set` can be done efficiently using the `insert()` function or the `emplace()` function for individual elements.

#### Inserting into `std::map`:

```cpp
std::map<int, std::string> myMap;

// Inserting key-value pairs into map
myMap.insert({1, "first"});
myMap.emplace(2, "second");
```

#### Inserting into `std::set`:

```cpp
std::set<int> mySet;

// Inserting elements into set
mySet.insert(1);
mySet.emplace(2);
```

### Time Complexity Analysis:
- Both `insert()` and `emplace()` operations have an average time complexity of `O(log n)` for insertion into `std::map` and `std::set`.
- The logarithmic complexity arises from the balanced binary search tree structure, ensuring efficient insertion while maintaining order.

### Space Complexity Analysis:
- The space complexity of inserting elements into `std::map` and `std::set` is proportional to the number of elements inserted, with no additional overhead beyond the container itself.

### Deletion in `std::map` and `std::set`

Deleting elements from `std::map` and `std::set` can be efficiently done using the `erase()` function.

#### Deleting from `std::map`:

```cpp
std::map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Deleting elements from map
myMap.erase(1); // Deletes the element with key 1
```

#### Deleting from `std::set`:

```cpp
std::set<int> mySet = {1, 2, 3};

// Deleting elements from set
mySet.erase(1); // Deletes the element 1
```

### Time Complexity Analysis:
- The `erase()` operation for both `std::map` and `std::set` has an average time complexity of `O(log n)` for deletion.
- The logarithmic complexity arises from the balanced binary search tree structure, ensuring efficient deletion while maintaining order.

### Space Complexity Analysis:
- The space complexity of deleting elements from `std::map` and `std::set` is `O(1)` as it does not involve additional memory overhead beyond the container itself.

### Modification in `std::map` and `std::set`

Modifying elements in `std::map` involves updating the value associated with a specific key, while in `std::set`, it involves removing the existing element and inserting a new one.

#### Modifying `std::map`:

```cpp
std::map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Modifying value associated with key 2
myMap[2] = "new value"; // Update value associated with key 2
```

#### Modifying `std::set`:

```cpp
std::set<int> mySet = {1, 2, 3};

// Modifying element in set
mySet.erase(1); // Remove existing element
mySet.insert(4); // Insert new element
```

### Time Complexity Analysis:
- Modifying elements in `std::map` and `std::set` has an average time complexity of `O(log n)`.

### Space Complexity Analysis:
- The space complexity of modifying elements in `std::map` and `std::set` is `O(1)` as it does not involve additional memory overhead beyond the container itself.

### Searching in `std::map` and `std::set`

Searching for elements in `std::map` and `std::set` can be efficiently performed using the `find()` function.

#### Searching in `std::map`:

```cpp
std::map<int, std::string> myMap = {{1, "first"}, {2, "second"}};

// Searching for key 2 in map
auto iter = myMap.find(2);
if (iter != myMap.end()) {
    std::cout << "Key 2 found, associated value: " << iter->second << std::endl;
} else {
    std::cout << "Key 2 not found" << std::endl;
}
```

#### Searching in `std::set`:

```cpp
std::set<int> mySet = {1, 2, 3};

// Searching for element 2 in set
auto iter = mySet.find(2);
if (iter != mySet.end()) {
    std::cout << "Element 2 found" << std::endl;
} else {
    std::cout << "Element 2 not found" << std::endl;
}
```

### Time Complexity Analysis:
- The `find()` operation for both `std::map` and `std::set` has an average time complexity of `O(log n)` for searching.

### Space Complexity Analysis:
- Searching in `std::map` and `std::set` does not incur any additional space overhead beyond the container itself. The space complexity remains constant, denoted as `O(1)`.

### Conclusion:

- **Use `std::map` when:** Storing key-value pairs where elements need to be ordered by keys and rapid access to elements based on keys is required.
- **Use `std::set` when:** Storing unique elements where elements need to be ordered by values and rapid access and membership testing are essential.
- Both containers offer efficient insertion, deletion, modification, and searching operations with average logarithmic time complexity, making them suitable for various applications where ordered data structures are needed.

## Comparing `std::map`/ `std::set` and their Hash-Based Counterparts

### Performance and Complexity:

- **`std::map` and `std::set`:**
  - Offer average logarithmic time complexity `O(log n)` for insertion, deletion, and searching operations.
  - Provide ordered storage of elements, maintaining a balanced binary search tree structure.
  - Suitable for scenarios where elements need to be ordered and rapid access based on keys or values is required.

- **`std::unordered_map` and `std::unordered_set`:**
  - Offer average constant-time complexity `O(1)` for insertion, deletion, and searching operations.
  - Do not maintain any specific ordering of elements, organizing them based on hash functions for rapid access.
  - Ideal for scenarios where rapid access to elements based on keys or values is essential, and the order of elements is not significant.

### Memory Overhead:

- **`std::map` and `std::set`:**
  - May have lower memory overhead compared to hash-based counterparts for small to medium-sized datasets due to the balanced binary search tree structure.
  - Require additional memory for maintaining tree nodes and pointers.

- **`std::unordered_map` and `std::unordered_set`:**
  - May incur more memory overhead due to hash tables and buckets, especially for large datasets.
  - Offer efficient access and management of elements but may consume more memory than balanced trees.

### Ordering:

- **`std::map` and `std::set`:**
  - Maintain the order of elements as they are inserted, providing sequential access.
  - Suitable when ordered storage and retrieval of elements are required.

- **`std::unordered_map` and `std::unordered_set`:**
  - Do not maintain any specific ordering of elements, organizing them based on hash functions for rapid access.
  - Ideal for scenarios where the order of elements is not significant, and rapid access or membership testing is crucial.

### Use Cases:

- **`std::map` and `std::set`:**
  - Suitable for applications requiring ordered storage of elements and efficient access based on keys or values, such as associative arrays, dictionaries, or priority queues.

- **`std::unordered_map` and `std::unordered_set`:**
  - Ideal for scenarios where rapid access to elements based on keys or values is essential, and the order of elements is not significant, such as indexing, caching, or membership testing.

### Conclusion:

- **Use `std::map` and `std::set` when:** Ordered storage of elements and efficient access based on keys or values are required, and memory overhead is manageable.
- **Use `std::unordered_map` and `std::unordered_set` when:** Rapid access to elements based on keys or values is essential, and the order of elements is not significant, even at the cost of potentially higher memory overhead.

Understanding the specific requirements, performance characteristics, and trade-offs involved in your project will help you choose the most suitable container for your needs.
Understanding the specific requirements and trade-offs involved in your project will help you choose the most suitable container for your needs.
