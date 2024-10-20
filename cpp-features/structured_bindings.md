# Structured Bindings

Structured bindings, introduced in C++17, allow you to unpack elements of an array, tuple, or struct into separate variables.

## Syntax
Hashmap example
```cpp
void loop_map_items() {
  std::unordered_map<std::string, std::string> colors = {
      {"RED", "#FF0000"},
      {"GREEN", "#00FF00"},
      {"BLUE", "#0000FF"},
  };
  
  for (const auto& [name, hex] : colors) {
      std::cout << "name: " << name << ", hex: " << hex << '\n';
  }
}
```
