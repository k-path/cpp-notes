# std::lower_bound
The lower_bound() function is used to find an element in a sorted range that has a value not less than the given value. It is defined inside the <algorithm> header file.
It uses a binary search, so it's O(logn).

## Syntax
```cpp
std::lower_bound (first, last, val, comp);
```
first: Iterator to the first element in the range.
last: Iterator to the element one past the last element in the range.
val: Value of the lower bound to be searched for in the range.
comp(optional): Binary function that accepts and compares the val with the current element in the range. By default, it returns true if the element in the range is smaller than val.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  
    // Input vector containing sorted integers
    vector<int> vec = {10, 20, 30, 40, 50};

    // Finding the lower bound for value 30
    auto lb = lower_bound(vec.begin(), vec.end(), 35); 

    // Checking if lower bound found and printing the position
      if (lb != vec.end())
      cout << "Position Found: " << distance(vec.begin(), lb);
    else
      cout << "Lower bound not found";

    return 0; 
}
```
Output
```
Position Found: 3
```
