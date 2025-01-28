# Lambda Functions
A lambda function in C++ is an anonymous function that can be defined inline within your code. It's a convenient way to create small, one-off functions without having to explicitly name them.

```
[ capture clause ] (parameters) -> return-type  
{   
   definition of method   
}
```

## Explanation:
**capture clause**:
This is where you specify which variables from the surrounding scope you want to use inside the lambda function.

- []: Captures nothing. (You can't access anything defined outside of the lambda function)
- [=]: Captures all variables from the surrounding scope by value. (You can access all stuff within the same scope but they won't be modified)
- [&]: Captures all variables from the surrounding scope by reference. (You can access all stuff within the same scope and they can be modified)
- You can also capture specific variables by name, e.g., [x, &y].

**parameters**:
This is the list of parameters the lambda function takes, just like a regular function.

**return_type**:
This is the return type of the lambda function, which can be inferred in many cases (since C++14).

**function body**:
This is the code that the lambda function executes when called.

## Example
```cpp
#include <iostream>

int main() {
    int x = 10;

    auto lambda = [x]() { std::cout << x << std::endl; };
    // auto lambda = [&x]() { std::cout << x << std::endl; };

    x = 20; // Changing x outside the lambda

    lambda(); // Still prints 10, because x was captured by value
    // The commented out lambda definition would make this print 20
}
```

## Lambda can be passed into functions like ```std::sort()```
Let's say we are given ```events``` that is an array of an array of strings. Each inner array will have ```[{event_type}, {timestamp}, {details}]```. 

If we wanted to sort the array by timestamp, lowest timestamp -> highest, we could use the ```std::sort()``` function with a lambda function passed into it.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

struct Event {
    string event_type;
    string timestamp;
    string details;
};

int main() {
  vector<vector<string>> events = {
       {"MESSAGE", "10", "id1 id0"},
       {"OFFLINE", "10", "0"},
       {"MESSAGE", "12", "ALL"}
   };
  
  // convert the original array of arrays to an array of the Event object type which we created
  vector<Event> converted_events_array;
  for (const vector<string>& ev : events) {
      converted_events_array.push_back({ev[0], ev[1], ev[2]});
  }
  
  sort(converted_events_array.begin(), converted_events_array.end(), 
          [](const Event& e1, const Event& e2){
              // if timestamps are the same, OFFLINE event takes precedence
              if (e1.timestamp == e2.timestamp) {
                  if (e1.event_type == "OFFLINE") {
                      return true;
                  }
                  if (e2.event_type == "OFFLINE") {
                      return false;
                  }
              }
              return stoi(e1.timestamp) < stoi(e2.timestamp); // sort by timestamp, if timestamp on left (e1.timestamp) is less, it will come before timestamp on the right
          }
  );

  for (const Event& ev : events) {
    cout << ev.event_type << "\t" << ev.timestamp << '\n';
  }

  return 0;
```
