# Useful Operations
## Convert a single digit char (e.g. '2') to integer
```cpp
#include <iostream>
using namespace std;
int main() {
	char two_char = '2';
	int two = '2' - '0';
	cout << two << '\n'; // prints the number 2 as an integer type
	return 0;
}
```

## Get the last digit of a number/every digit of a number OR Get every prefix of a number
```cpp
#include <iostream>
using namespace std;
int main() {
	int num = 1337;
	while (num > 0) {
		cout << num % 10 << '\n'; // get last digit
		num /= 10; // cut off last digit
	}
	return 0;
}

```
```cpp
#include <iostream>
using namespace std;
int main() {
	int num = 1337;
	int stored_num = num;
	int num_digits = 0;
	while (num > 0) {
		num_digits++;
		num /= 10; // cut off last digit
	}

	int dividing_by = 1;
	while (num_digits > 0) {
		int curr_prefix = stored_num / dividing_by;
		cout << curr_prefix << '\n';
		dividing_by *= 10;
		num_digits--;
	}
	return 0;
}

```

```
7
3
3
1
```

## Convert letter to its position in the alphabet
```cpp
#include <iostream>
using namespace std;
int main() {
	cout << 't'-'a' << '\n'; // prints 19
	cout << 'z'-'a' << '\n'; // 25
	return 0;
}
```

## BFS 4-directionally on a graph using "direction arrays"
```cpp
vector<int> d_i = {1, -1, 0, 0};
vector<int> d_j = {0, 0, 1, -1};
while (!q.empty()) {
    int i = q.front().first;
    int j = q.front().second;
    q.pop();
// v Direction Array logic here v
    for (int k = 0; k < 4; ++k) { // move in all 4 directions
	int new_i = i + d_i[k];
	int new_j = j + d_j[k];
	
	if (new_i < m && new_i >= 0 && new_j < n && new_j >= 0) { // boundary check
	*Rest of BFS logic and checks*
	
```
