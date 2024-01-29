tags: stl, std, loop
---

# Additional STL Algorithms for Data Processing in C++

In addition to normal loops such as `for` or `while`, C++ provides a wide range of powerful algorithms in the Standard Template Library (STL) for various data processing tasks. Let's explore some more useful algorithms along with their different usages.

-----

# `<algorithm>`

---

## `std::copy` (Introduced in C++98)

The `std::copy` algorithm, found in the `<algorithm>` header, copies elements from a source range to a destination range.

### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> source = {1, 2, 3, 4, 5};
std::vector<int> destination(source.size());

// Copy elements from source
std::copy(source.begin(), source.end(), destination.begin());
```
---


## `std::vector::erase` (Introduced in C++11)

The `std::vector::erase` member function, available for `std::vector` containers, removes elements from the vector that satisfy a specific criterion.

### Details:

In C++11, `std::vector::erase` provides a way to remove elements from a vector based on a given value or range.

### Example Usage (C++11) with `std::remove`:

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};

// Erase all occurrences of the value 3 from the vector
vec.erase(std::remove(vec.begin(), vec.end(), 3), vec.end());
```

### Note:

This introduction highlights the usage of `std::remove` in combination with `std::vector::erase` for removing elements based on a specific value, along with details about `std::vector::erase` and its availability in C++11.

---


## `std::for_each` (Introduced in C++98)

The `std::for_each` algorithm, found in the `<algorithm>` header, applies a given function to each element in a range.

### Different Usages of `std::for_each`

1. **Using with Function Pointer:**
   - Available in C++98 and later versions.

   ```cpp
   #include <algorithm>
   #include <vector>

   void process(int n) {
       // Do something with n
   }

   std::vector<int> vec = {1, 2, 3, 4, 5};
   std::for_each(vec.begin(), vec.end(), process);
   ```

2. **Using with Lambda Expression:**
   - Requires C++11 or later versions.

   ```cpp
   #include <algorithm>
   #include <vector>

   std::vector<int> vec = {1, 2, 3, 4, 5};
   std::for_each(vec.begin(), vec.end(), [](int n) {
       // Do something with n
   });
   ```

3. **Using with Functor (Function Object):**
   - Available in C++98 and later versions.

   ```cpp
   #include <algorithm>
   #include <vector>

   struct Process {
       void operator()(int n) const {
           // Do something with n
       }
   };

   std::vector<int> vec = {1, 2, 3, 4, 5};
   std::for_each(vec.begin(), vec.end(), Process());
   ```

4. **Using for Side Effects:**
   - Requires C++11 or later versions.

   ```cpp
   #include <algorithm>
   #include <vector>

   std::vector<int> vec = {1, 2, 3, 4, 5};
   int sum = 0;
   std::for_each(vec.begin(), vec.end(), [&sum](int n) {
       sum += n;
   });
   ```

5. **Using for Transformation:**
   - Requires C++11 or later versions.

   ```cpp
   #include <algorithm>
   #include <vector>

   std::vector<int> vec = {1, 2, 3, 4, 5};
   std::vector<int> squared;
   std::for_each(vec.begin(), vec.end(), [&squared](int n) {
       squared.emplace_back(n * n);
   });
   ```

### Note:

The various usages of `std::for_each` demonstrate its versatility in applying functions to elements within a range. While the function pointer approach provides a traditional and straightforward mechanism, the introduction of lambda expressions and functors in later versions of C++ adds powerful tools for more expressive and concise code.

- **Using Function Pointer:** This classic approach, available since C++98, offers a simple way to apply a function to each element in a range.

- **Using Lambda Expression:** Introduced in C++11, lambda expressions allow for inline function definitions, enhancing code readability and conciseness.

- **Using Functor (Function Object):** Also available since C++98, functors provide a means to encapsulate state and behavior, offering flexibility and reusability in algorithmic operations.

- **Using for Side Effects and Transformation:** These advanced techniques, requiring C++11 or later, leverage lambda expressions or functors to achieve specific tasks such as accumulating values or transforming elements within a range.

Overall, the different realizations of `std::for_each` cater to a wide range of programming needs, from basic function application to more sophisticated operations, contributing to the expressiveness and efficiency of C++ codebases.

---

## `std::find`, `std::find_if`, `std::find_if_not`, `std::find_end` (Introduced in C++98 or C++11)

### `std::find` (Introduced in C++98)

The `std::find` algorithm, found in the `<algorithm>` header, searches for the first occurrence of a specified value within a range.

#### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
auto it = std::find(vec.begin(), vec.end(), 3);
```

### `std::find_if` (Introduced in C++98)

The `std::find_if` algorithm, found in the `<algorithm>` header, searches for the first element in a range that satisfies a specified condition.

#### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
auto it = std::find_if(vec.begin(), vec.end(), [](int n) { return n % 2 == 0; });
```
### **Attention** ⚠:

lambda introduced in C++11, if you are using C++98, refer to the `for_each`, defined the function or functor outside the loop.

### `std::find_if_not` (Introduced in C++11)

The `std::find_if_not` algorithm, found in the `<algorithm>` header, searches for the first element in a range that does not satisfy a specified condition.

#### Example Usage (C++11):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
auto it = std::find_if_not(vec.begin(), vec.end(), [](int n) { return n % 2 != 0; });
```

## `std::find_end` (Introduced in C++98)

The `std::find_end` algorithm, found in the `<algorithm>` header, searches for the last occurrence of a subsequence within a range.

### Details:

In C++98, `std::find_end` searches for the last occurrence of the sequence defined by the iterators `[s_first, s_last)` within the range `[first, last)`.

### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> main_sequence = {1, 2, 3, 4, 5, 1, 2, 3, 4, 5};
std::vector<int> sub_sequence = {1, 2, 3};

// Search for the last occurrence of the subsequence within the main sequence
auto it = std::find_end(main_sequence.begin(), main_sequence.end(), sub_sequence.begin(), sub_sequence.end());

// Check if the subsequence was found
if (it != main_sequence.end()) {
    // Found the last occurrence of the subsequence
    // 'it' points to the beginning of the last occurrence
    // Perform further actions as needed
}
```

### Note:

`std::find_end` is particularly useful when you need to locate the last occurrence of a subsequence within a larger sequence. It provides flexibility in searching for patterns within data and can be handy in various applications such as text processing and pattern recognition.

---

## `std::reverse` (Introduced in C++98 & C++17)
### `std::reverse` (C++98):

The `std::reverse` algorithm, found in the `<algorithm>` header, reverses the order of elements in a range.

### Details:

In C++98, `std::reverse` takes two iterators, defining the range of elements to be reversed. It operates in a sequential manner.

### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
// Reverse the elements in the vector
std::reverse(vec.begin(), vec.end());

```
### `std::reverse` (C++17):

The `std::reverse` algorithm, found in the `<algorithm>` header, offers additional functionality compared to its C++98 counterpart.

#### Details:

In C++17, an additional overload of `std::reverse` was introduced, allowing the specification of an execution policy. This enables potential parallelization of the reversal operation for improved performance in certain scenarios. The syntax for this overload includes an execution policy parameter, providing options such as sequential, parallel, or vectorized execution.

- `std::execution::seq`: Specifies sequential execution.
- `std::execution::par`: Specifies parallel execution.
- `std::execution::par_unseq`: Specifies parallel and vectorized execution.

Compared to the C++98 version, this version with execution policies has better potential for performance improvements, especially for large ranges and when executed on multi-core processors.

#### Example Usage (C++17):

```cpp
#include <algorithm>
#include <vector>
#include <execution> // for std::execution
// ...
std::vector<int> vec = {1, 2, 3, 4, 5};
// Reverse the elements in the vector using parallel execution
std::reverse(std::execution::par, vec.begin(), vec.end());

```


## `std::swap_ranges` (Introduced in C++98)

The `std::swap_ranges` algorithm, found in the `<algorithm>` header, swaps the elements of two ranges.

### Example Usage (C++98):

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vect0 = {1, 2, 3, 4, 5};
std::vector<int> vect1 = {6, 7, 8, 9, 10};

// Swap elements from vect0 to vect1
std::swap_ranges(vect0.begin(), vect0.end(), vect1.begin());
```

### Note:

Sometimes, when dealing with two vectors, such as `vect0` and `vect1`, where we need to assign values from `vect0` to `vect1`, and we will not be using the values in `vect0` anymore, we can simply swap them. Alternatively, if we still need the values of `vect1`, we can swap them as well.

This approach is more flexible than copying, as it exchanges the pointers directly. For some complex structs, this might be much faster, although for primitive types, the performance difference is almost negligible.

---

## `std::transform` (Introduced in C++98)

The `std::transform` algorithm, found in the `<algorithm>` header, applies a specified operation to each element in one or two ranges and stores the result in another range.

### Details:

In C++98, `std::transform` performs an operation specified by a provided binary function on corresponding elements from two ranges, storing the result in a third range. Alternatively, it can apply a unary operation to each element in a single range.

### Example Usage (C++98) with Binary Function:

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec1 = {1, 2, 3, 4, 5};
std::vector<int> vec2 = {10, 20, 30, 40, 50};
std::vector<int> result(vec1.size());

// Add corresponding elements from vec1 and vec2 and store the result in 'result'
std::transform(vec1.begin(), vec1.end(), vec2.begin(), result.begin(), std::plus<int>());
```

### Example Usage (C++11) with Unary Function using Lambda:

```cpp
#include <algorithm>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
std::vector<double> result(vec.size());

// Apply a lambda function (square root) to each element in 'vec' and store the result in 'result'
std::transform(vec.begin(), vec.end(), result.begin(), [](int x) { return std::sqrt(x); });
```

### Note:

`std::transform` provides a flexible way to perform element-wise operations on ranges, allowing for various transformations and computations to be applied efficiently.

-----

# `<numeric>`

---

## `std::accumulate` (Introduced in C++98)

The `std::accumulate` algorithm, found in the `<numeric>` header, computes the sum of elements in a range, or performs a binary operation sequentially on all elements in the range.

### Example Usage (C++98) for Summation:

```cpp
#include <numeric>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};

// Compute the sum of elements in the vector
int sum = std::accumulate(vec.begin(), vec.end(), 0);
```

### Example Usage (C++11) for Binary Operation with Lambda:

```cpp
#include <numeric>
#include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};

// Compute the product of elements in the vector using a lambda function
int product = std::accumulate(vec.begin(), vec.end(), 1, [](int acc, int curr) {
    // 'acc' represents the accumulated product so far
    // 'curr' represents the current value of the element being processed
    return acc * curr; // Multiply the accumulated product with the current value
});
```

### Note:

`std::accumulate` with a lambda function as the binary operation is only available starting from C++11. It provides a concise and efficient way to compute the sum or perform other binary operations on elements in a range.

---

## `std::adjacent_difference` (Introduced in C++98)

The `std::adjacent_difference` algorithm, found in the `<numeric>` header, computes the differences between adjacent elements in a range and stores the result in another range.

### Details:

In C++98, `std::adjacent_difference` computes the differences between adjacent elements in the range defined by the iterators `[first, last)` and stores the result in the range beginning at the iterator `result`.

### Example Usage (C++98):

```cpp
#include <numeric>
#include <vector>

std::vector<int> vec = {1, 4, 7, 10, 13};
std::vector<int> differences(vec.size());

// Compute the differences between adjacent elements in 'vec' and store the result in 'differences'
std::adjacent_difference(vec.begin(), vec.end(), differences.begin());
```

### Note:

`std::adjacent_difference` is a useful algorithm for computing differences between adjacent elements, often used in applications such as computing velocity from position data or finding the rate of change in a series of values.

---

## `std::iota` (Introduced in C++98)

The `std::iota` algorithm, found in the `<numeric>` header, assigns sequential values to a range, starting from a specified initial value.

### Details:

In C++98, `std::iota` assigns sequential values to the elements in the range defined by the iterators `[first, last)`. It starts with the initial value provided and increments it for each successive element in the range.

### Example Usage (C++98):

```cpp
#include <numeric>
#include <vector>

std::vector<int> vec(5);

// Assign sequential values starting from 1 to the elements in 'vec'
std::iota(vec.begin(), vec.end(), 1);
```

### Note:

`std::iota` is particularly useful when initializing containers with sequential values or creating sequences for algorithms like `std::generate`.
