# Memory Management
Memory management is required to avoid wastage of memory and to make sure allocation takes place efficently. During the declaration of arrays, sometimes the exact memory is not known to us, and that is why we declare an array of max size, which results in memory wastage.
To avoid such a case, we use memory allocation. 

## Raw pointers
A raw pointer is a variable that stores the memory address of another variable. It directly holds the memory location and provides direct access to the data stored at that address. Raw pointers are declared using an asterisk (*) before the variable name. 
They require manual memory allocation using `new` and deallocation using `delete` or `delete[]` to prevent memory leaks.
```cpp
int* ptr; // Declares a raw pointer to an integer
int num = 10;
ptr = &num; // Assigns the address of 'num' to 'ptr'

cout << "ptr: " << ptr << '\n'; // will hold the address of num
cout << "&num: " << &num << '\n';
cout << "*ptr: " << *ptr << '\n'; // will dereference to the value at num's address
cout << "&ptr: " << &ptr << '\n'; // NOTHING will change about ptr's address, it still has its own address
```
```
ptr: 0x7ffcad598054
&num: 0x7ffcad598054
*ptr: 10
&ptr: 0x7ffcad598058
```
## Operator new and operator delete
The `new` oeprator is used to allocate memory and the `delete` operator is use for the deallocation memory.

The `new` operator is used for dynamic memory allocation. The new operator is responsible for the creation of the object.

Syntax:
`PointerVariable = new datatype;`
Syntax for arrays:
`PointerVariable = new datatype[size];`
For arrays, the new keyword returns the address of first element

Example:
```cpp
Int *ptr'
ptr = new int;
*ptr = 75;
```
The delete operator is used for deallocation of memory or for releasing of memory space. Once the memory is no longer required, then we have to deallocate the memory using the `delete` operator.

Syntax:
`delete PointerVariable;`

Syntax for arrays:
`delete [] PointerVariable;

Example:
```cpp
Int *ptr;
ptr = new int;
*ptr = 75;
// after printing
delete ptr;
// We could also set ptr to nullptr; afterwards
```

Advantages of `new`:
- `new` operator can be overloaded
- The return type of new is of the type for which the memory was allocated (don't need to type cast unlike `malloc`)
- It automatically computes the size of the data object (don't need size of unlike `malloc`)
- It can initialize object while creating memory for it (unlike `malloc`)

Class example:
```cpp
#include <iostream>
using namespace std;

class Student {
public:
  string name;
  int* age;

  void print() {
    cout << name << '\n';
  }

  void print_age() {
    cout << *age << '\n';
  }

  Student(string name, int studentAge) : name(name) {
    age = new int(studentAge);
  }

  // destructor
  ~Student() {
    delete age;
  }
  
};

int main() {
  Student *student = new Student();
  student->name = "John"; // (*student).name = "John";
  student->print(); // (*student).print();

  // with defined constructor not default constructor
  Student *student = new Student("Mary"); // 
  student->print(); 
  

  delete student;

  // handle failed dynamic mem alloc with try-catch block
  try {
    double *big_array = new double[999999999999999];
  } catch {
    cout << "bad_alloc caught: " << exp.what() << '\n';
  }
  
  // handle failed dynamic mem alloc with nothrow (when nothrow is used as argument for new, it returns a null pointer instead.)
  double *big_again = new(nothrow) double[999999999999999];

  if (big_again == NULL) {
    cout << "Failed to allocate!" << '\n';
  }

  // placement new, (place in existing memory location)
  double *mydouble = new double(12.2);
  double *specific = new(mydouble) double(20.5); // puts the value 20.5 at the memory position mydouble is at

  cout << *mydouble << " and " << *specific << '\n'; // 20.5 and 20.5

  // destructor example, if you paste this you'd probably get an error didnt feel like editing the above instantiation
  Student *student2 = new Student("John", 20);
  student2->print_age();
  
  delete student2; // system will then call the destructor of Student to free internally allocated memory, then also deallocates the memory that the instance points to

}
```

## Smart Pointers (No Manual Management Necessary)
Smart pointer is a class that behaves like a regular (raw) pointer but automatically manages the lifetime of the object it points to, preventing memory leaks. Smart pointers don't need destructors like raw pointers.

### std::unique_ptr
As the name implies, makes sure that only exactly one owner of an object pointer exists. Represents exclusive ownership. When the `unique_ptr`goes out of scope, the object is automatically deleted. Introduced in C++11.

It's a class template that wraps a raw pointer in it. -> and * can be used on the object because they are overloaded in the `unique_ptr` class.

Operations: `std::release`, `std::reset`, `std::swap, `std::get`, `std::get_deleter`

Example with a class:
```cpp
#include <iostream>
#include <memory> // necessary include for using smart ptrs
using namespace std;

class Foo {
    int x; // private by default since outside of public
  public:
    explicit Foo(int x) : x{x} {}
    int getX() { return x; }
  ~Foo() { cout << "Foo Destructor" << '\n'; }
};

int main() {
  // Foo *f = new Foo(10);
  // cout << f->getX() << '\n';

  unique_ptr<Foo> p(new Foo(10)); // explicit constructor with raw pointer. In this case, p is on the stack, but the Foo object is on the heap
  cout << p->getX() << '\n';

  // other ways to create unique_ptr
  // default constructor, 
  // unique_ptr<int> ptr; 

  // make_unique
  // auto ptr = make_unique<int>(42); // safer and more efficient
  unique_ptr<Foo> p2 = make_unique<Foo>(20);

  // p = p2; This will FAIL, because you can not copy ownership
  unique_ptr<Foo> p3 = move(p); // PASS because MOVING ownership is allowed
}
```

### std::shared_ptr
Enables shared ownership. Multi `shared_ptr` instances can point to the same object, and a reference count tracks how many are doing so. The object is deleted only when the last shared_ptr pointing to it goes out of scope and is destroyed.


### std::weak_ptr
It is a special type of `shared_ptr` which doesn't count the reference. Provides non-owning access to an object managed by a `shared_ptr`. The main purpose of `weak_ptr` is to solve circular reference problems that can occur with `shared_ptr`. If two objects hold `shared_ptr`s to each other, they'll never be deleted. If you make one of them a `weak_ptr` they both can be deleted.
