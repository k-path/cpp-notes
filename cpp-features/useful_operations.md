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

## Get the last digit of a number/every digit of a number
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
