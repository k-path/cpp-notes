# std::upper_bound
Used to find the first element in the given range that is greater than the given value. Not necessarily the opposite of lower_bound, also O(logn). 
\nLower bound: Returns an iterator pointing to the first element in the range that is not less than (i.e., greater than or equal to) the given value.
\nUpper bound: Returns an iterator pointing to the first element in the range that is greater than the given value.
### Key differences:
Lower bound: Includes elements equal to the given value.
\nUpper bound: Excludes elements equal to the given value.

## Syntax
```cpp
std::upper_bound (first, last,  val, comp);
```
### Parameters
- first: Iterator to the first element in the given range.
- last: Iterator to the element that is just after the last element in the range.
- val: Value of the upper bound to search for in the range. 
- comp (optional): Binary function that accepts two arguments and returns a boolean value. By default, it is set to find the first value just greater than the val.

### Return value
- Returns an iterator to the first element in a sorted range [first, last) that is just greater than a given value.
- If all the elements are less than or equal to the given value, then the iterator to the end of the range.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    vector<int> v{10, 20, 30, 40, 50};

    // Finding upper bound for 35 and 45
    auto upper1 = upper_bound(v.begin(), v.end(), 35);
    auto upper2 = upper_bound(v.begin(), v.end(), 45);

    cout << "45's upper_bound position: "
      << (upper1 - v.begin()) << endl;
    cout << "45's upper_bound position: "
      << (upper2 - v.begin());

    return 0;
}
```
Output
```
upper_bound for element 35 is at position : 3
upper_bound for element 45 is at position : 4
```
