# Read from File (I forgot)
To use the `fstream` library, include both the `<iostream>` AND `<fstream>` header file:
```cpp
#include <iostream>
#include <fstream>
```

- Declare an input file stream object using `std::ifstream`
- Open the file using `open()` method, specifying the file name as an argument.
- Read data from the file using the extraction operator >> or the `getline()` function
- After reading, close the file using `close()` method to release resources

```cpp
#include <iostream>
#include <fstream>
#inclue <string>
using namespace std;

int main() {
  ifstream inputFile("example.txt");

  if (inputFile.is_open()) {
    string line;
    while (getline(inputFile, line)) {
      cout << line << '\n';
    }
    inputFile.close();
  }  else {
    cerr << "Unable to open file" << '\n';
    return 1;
  }

  return 0;
}
```
