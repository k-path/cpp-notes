# `std::map` is more flexible when it comes acceptable keys than `std::unordered_map` a.k.a hashmap 

## Some data types are not hashable by default in C++
For example, you can't use a `pair<int,int>` as a key like: `unordered_map<pair<int,int>, int>`
..that is unless you define your own custom hash function. By default, the standard library doesn't provide a hash function for pairs; only provides it for primitive/basic types.

`unordered_map` uses a hash function and the == operator for equality.

## For a `map<pair<int,int>, int>`, you don't need a hash function because map is ordered using the < operator
This is defined for pairs (they're compared lexicographically). So this works right out of the box:
```cpp
map<pair<int, int>, int> myMap;
myMap[{1, 2}] = 42;
```
Any type can be a key in a map as long as it has a defined less than operator (<). The standard library provides this for: All basic types (int, double, etc.), string, pair, array, tuple. 
For containers like vector, set, etc., the comparison operators are already defined, so these can also be keys in a map.
## Bring your own hash function
However, for an `unordered_map<pair<int,int>, int>`, you would need to define a hash function:
```cpp
struct PairHash { //  struct is designed to be a functor (function object) that can be used as a custom hash function.
                  // so can be passed as a template argument to containers like unordered_map
        template<class T1, class T2>
        size_t operator () (const pair<T1,T2>& p) const { // overloads the function call operator operator(). size_t is type used for hash values
            auto h1 = hash<T1>{}(p.first); // creates hash values for both elements of the pair using their respective hash specializations
            auto h2 = hash<T2>{}(p.second);
            return h1^h2; // combines these hash values using the XOR operation
        }
    };
```
Although this one is slightly better:
```cpp
struct PairHash {
    template <class T1, class T2>
    size_t operator() (const pair<T1, T2>& p) const {
        auto h1 = hash<T1>{}(p.first);
        auto h2 = hash<T2>{}(p.second);
        return h1 ^ (h2 << 1); // shifts bit before XOR
    }
};
```
Because:
If two pairs like (a,b) and (b,a) have the same values but in different positions, a simple XOR would produce the same hash (because XOR is commutative: a^b = b^a). The bit shift breaks this symmetry.

Both of these lines are combining two hash values, but they do it slightly differently.
- `return h1 ^ (h2 << 1);` performs a shift on the second hash value before XORing it with the first. By shifting h2 left by 1 bit, it changes the bit pattern before combining with h1. This creates a different hash result than if you just XORed them directly.
- `return h1 ^ h2;` simply XORs the two hash values without any bit shifting.

## Trivia
`map` uses a binary search tree (usually red-black tree) that just needs an ordering relation.

`unordered_map` uses a hash table that needs a way to distribute keys uniformly across buckets.
