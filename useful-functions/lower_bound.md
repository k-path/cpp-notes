# std::lower_bound
The lower_bound() function is used to find an element in a sorted range that has a value not less than the given value (either this value is equal to `val` or it's the next value greater than it). It is defined inside the <algorithm> header file.
It uses a binary search, so it's O(logn).

## Syntax
```cpp
std::lower_bound (first, last, val, comp);
```
### Parameters
- first: Iterator to the first element in the range.
- last: Iterator to the element one past the last element in the range.
- val: Value of the lower bound to be searched for in the range.
- comp(optional): Binary function that accepts and compares the val with the current element in the range. By default, it returns true if the element in the range is smaller than val.

### Return value
- This function returns an iterator pointing to the next smallest number just greater than or equal to val.
- If all the elements in the range are less than the given value, the function returns an iterator to the end of the range.
- If all the elements in the range are larger than given value, the function returns a pointer to the first element.

```cpp
#include <algorithm>
#include <vector>
#include <iostream>
using namespace std;

int main() {
  
    // Input vector containing sorted integers
    vector<int> vec = {10, 20, 30, 32, 34, 35, 40, 50};
    // if we erase 35 from this vector, the lower bound would be the iterator pointing at 40

    // Finding the lower bound for value 30
    auto lb = lower_bound(vec.begin(), vec.end(), 35); 

    // Checking if lower bound found and printing the position
    if (lb != vec.end()) {
      cout << "Position Found: " << distance(vec.begin(), lb) << '\n';
      cout << "The number is " << *lb << '\n';
    } else {
      cout << "Lower bound not found";
    }
    return 0; 
} 
```
Output
```
Position Found: 5
The number is 35
```
### Using `lower_bound`/`upper_bound` on a container of pairs
The easiest way is to pass a pair using `std::make_pair` into the third argument, like so:
```cpp
#include <iostream>
#include <utility>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    // Initialize with altitude -> thrust mappings
    // Format: {altitude in meters, thrust in newtons}
    vector<pair<double, double>> altitudeThrustTable = {
          {0.0, 750000.0},      // Sea level
          {1000.0, 800000.0},   // 1km
          {5000.0, 850000.0},   // 5km
          {10000.0, 900000.0},  // 10km
          {20000.0, 950000.0},  // 20km
          {40000.0, 980000.0},  // 40km
          {60000.0, 750000.0},  // 60km
          {80000.0, 500000.0},  // 80km
          {100000.0, 250000.0}  // 100km
    };

    double currentAltitude = 2500.0;
    auto lowerMapping = lower_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), make_pair(currentAltitude, 0.0)); 
    auto upperMapping = upper_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), make_pair(currentAltitude, 0.0));
    
}
```
We only care about the pair first values, we're only doing the comparison there so it doesn't matter what our temporary pair's second value is.

We could also use a custom comparator function (here we use lambda written directly into the argument). This one works if we wanted to search based on the second element, but the `make_pair` approach wouldn't work as easily because the binary search algorithms would compare pairs lexicographically (first element, then second), which doesn't match what you want when searching by the second element:
```cpp
#include <iostream>#include <utility>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
  vector<pair<double, double>> altitudeThrustTable = {
          {0.0, 750000.0},      // Sea level
          {1000.0, 800000.0},   // 1km
          {5000.0, 850000.0},   // 5km
          {10000.0, 900000.0},  // 10km
          {20000.0, 950000.0},  // 20km
          {40000.0, 980000.0},  // 40km
          {60000.0, 750000.0},  // 60km
          {80000.0, 500000.0},  // 80km
          {100000.0, 250000.0}  // 100km
    };

  double currentAltitude = 2500.0;
  // custom comparator based on FIRST element
  auto lowerMapping = lower_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), currentAltitude, [](const pair<double,double>& entry, double value) { return entry.first <= value; }); 
  auto upperMapping = upper_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), currentAltitude, [](double value, const pair<double,double>& entry) { return value < entry.first; }); 

  // custom comparator based on SECOND element
  auto lowerMapping = lower_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), currentAltitude, [](const pair<double,double>& entry, double value) { return entry.second <= value; }); 
  auto upperMapping = upper_bound(altitudeThrustTable.begin(), altitudeThrustTable.end(), currentAltitude, [](double value, const pair<double,double>& entry) { return value < entry.second; });

}
```
Note that for this to work properly, your container should be sorted by the second element. If your container is sorted by the first element (altitude) instead of the second element (thrust), then binary search functions won't give correct results when searching by thrust values. So in this case our binary search based on the second element wouldn't make any sense.
