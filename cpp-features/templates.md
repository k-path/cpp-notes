# Templates
A way to achieve generic programming in C++, where types in an algorithm are written in a to-be-specificied-later style. Allows us to write code that works for different data type without having to duplicate the code for each type.

## Use Case
```cpp
int add(int a, int b) {
  return a + b;
}
// add function cannot be used for, say for example, 
// float result = add(3.4, 2.6);  because the numbers would be implicitly converted to int before being passed to our function, thereby giving us an undesired result

// int result2 = add(3,3); would obviously work as intended though

// we would need to duplicate the function with a different type specification
float add(float a, float b) {
  return a + b;
}
```
This is where templates come in

## Syntax
```cpp
// function templates
template <class Type>  // can also use typename interchangeably with class
Type add(Type a, Type b) {
  return a + b;
}

int main () {
  int int_result = add<int>(3, 3);
  float float_result = add<float>(3.4, 2.6);
  return 0;
}
```

```cpp
// variable templates
template <typename Type>  
constexpr Type speed_of_light = Type(299'792'458); // the ' between the words are digit separators (just makes the number more readable)
// constexpr indicates that the value, or return value, is constant and, where possible, is computed at compile time. 
// A constexpr integral value can be used wherever a const integer is required, such as in template arguments and array declarations.

int main () {
  std::cout << speed_of_light<double> << '\n';
  std::cout << speed_of_light<float> << '\n';
  std::cout << speed_of_light<int> << '\n';
  std::cout << speed_of_light<long long> << '\n';
  return 0;
}
```

```cpp
// class templates (for classes and structs)
template <class Type>  
struct Pair {
  Type first, second;
};

int main () {
    auto numbers = Pair<float>(4.2,6.9);

    return 0;
}
```

```cpp
// alias templates
template <class Type, int Size>
struct FixedArray {
    Type m_data[Size];
};

//alias template to create an array
// of size 3
template <class Type>
using Vector3 = FixedArray<Type, 3>;

template <class Type>
using Vector2 = FixedArray<Type, 2>;
 
int main () {
   return 0;
}
```
