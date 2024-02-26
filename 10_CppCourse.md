# Special Member Functions
---
## Copy Constructor

A class object comes to life by taking its value from another class object of the same type.

### Scenario 1

```cpp
class MyClass 
{
public:
    MyClass(const MyClass& other)     // const lvalue reference, MyClass& reference to x, 'other' is to y.
    {
        
    }
};

MyClass y;
MyClass x = y;   // copy constructor gets called for this
```

### Scenario 2
```cpp
class MyClass 
{
public:
    MyClass(const MyClass& other)
    {
        
    }
};

MyClass x2(y);   // these two come to life through copy constructor
MyClass x3{y};
```

### Scenario 3
```cpp
class MyClass 
{
public:
    MyClass(const MyClass& other)
    {
        
    }
};

auto x = y;    // copy constructor is called
auto x2(y);   
auto x3{y};
```

### Scenario 4
```cpp
class ex_class1
{
    // ...
};

void foo(ex_class1 x)   // the parameter is of type 'ex_class1' , copy constructor will be called 
{
    //...
}

int main()
{
    ex_class1 ex;
    foo(ex);
}
```

### Scenario 5

```cpp
class ex_class2
{
    // ...
};

ex_class2 bar()   // the parameter is of type 'ex_class1' , copy constructor will be called 
{   
    //..
    return ex;
}

int main()
{
    ex_class2 myclass = bar();  // copy constructor will be called 

}
```
---
> Code of Copy ctor can be written by the compiler, because it is a special member function.
---

Example:
```cpp
class ex_class3 {
public: 
    ex_class3()
    {
        std::cout << "ex_class3 default ctor this : " << this << '\n';
    }

    ~ex_class3()
    {
        std::cout << "ex_class3 dtor this : " << this << '\n';
    }
};

int main()
{
    ex_class3 ex;
}
```
Output:
```
ex_class3 default ctor this : 000000B6E7B9FB24
ex_class3 dtor this : 000000B6E7B9FB24          (address of ctor and dtor is the same)
```
---


Example:
```cpp

```
Output:
```

```




Example:
```cpp

```
Output:
```

```
