# String Tokenization (Splitting strings using delimiter, how to " .strip()")

## Stringstream
Stream class to operate on strings.

## Std::getline (string)
```istream& getline (istream& is, string& str, char delim);```
```istream& getline (istream& is, string& str);```
Extracts characters from is and stores them into str until the delimitation character delim is found (or the newline character, '\n', for (2)).

The extraction also stops if the end of file is reached in is or if some other error occurs during the input operation.

If the delimiter is found, it is extracted and discarded (i.e. it is not stored and the next input operation will begin after it). So str would be empty for that operation.

Note that any content in str before the call is replaced by the newly extracted sequence.

Each extracted character is appended to the string as if its member push_back was called.

## Code
```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
using namespace std;

int main() {
    string path = "/home/johndoe/leetcode";
    stringstream ss(path); // create stream from path, currently ss is the stream made from path
    string path_name; // each subpath, each component being tokenized
    vector<string> path_vector;
    while (getline(ss, path_name, '/')) { // ss what is what we reading from, path_name where we're storing, '/' is the delimiter
        if (!path_name.empty()) { // whenever we split on a "/" path_name is empty
            path_vector.push_back(path_name);
        }
    }
    
    for (const string& s : path_vector) {
        cout << '/' << s;
    }

    return 0;
}
```
