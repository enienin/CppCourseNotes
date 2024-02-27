# Special Member Functions

## Copy Constructor
A copy constructor is a **member function** that initializes an object using another object of the same class. In simple terms, a constructor which creates an object by initializing it with an object of the same class, which has been created previously is known as a **copy constructor**.

**Copy constructor** is used to initialize the members of a newly created object by copying the members of an already existing object.

**Copy constructor** takes a reference to an object of the same class as an argument.

Syntax:
```cpp
ClassName(const ClassName &old_obj);
```

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
class ex_class4 {
public:
    ex_class4()
    {
        std::cout << "ex_class4 default ctor this : " << this << '\n';
    }

    ~ex_class4()
    {
        std::cout << "ex_class4 dtor this : " << this << '\n';
    }
};

int main()
{
    ex_class4 ex;
    
    {
        ex_class4 ey = ex;
    }   // destructor for 'ey' is called 

    std::cout << "Main continues..." << '\n';
    (void)getchar();
}       // destructor for 'ex' is called 
```
Output:
```
ex_class4 default ctor this : 0000007EA76FFC34  ( default ctor called for 'ex')
ex_class4 dtor this : 0000007EA76FFC54          ( notice how the address of ctor and dtor is different)
Main continues...

ex_class4 dtor this : 0000007EA76FFC34          ( dtor called for 'ex')
```
---
### Writing copy ctor ourselves

Example:
```cpp
class ex_class5 {
public:
    ex_class5()
    {
        std::cout << "ex_class5 default ctor this : " << this << '\n';
    }

    ex_class5(const ex_class5& other)
    {
        std::cout << "ex_class5 copy ctor this : " << this << '\n';
    }

    ~ex_class5()
    {
        std::cout << "ex_class5 dtor this : " << this << '\n';
    }
};

int main()
{
    ex_class5 ex;

    {
        ex_class5 ey = ex;
    }

    std::cout << "Main continues..." << '\n';
    (void)getchar();
}
```
Output:
```
ex_class5 default ctor this : 000000678F8FFC94
ex_class5 copy ctor this : 000000678F8FFCB4
ex_class5 dtor this : 000000678F8FFCB4
Main continues...
ex_class5 dtor this : 000000678F8FFC94
```
---
Example:
```cpp
class ex_class6 {
public:
	ex_class6()
	{
		std::cout << "ex_class6 default ctor this : " << this << '\n';
	}

	ex_class6(const ex_class6& other)
	{
		std::cout << "ex_class6 copy ctor this : " << this << '\n';
	}

	~ex_class6()
	{
		std::cout << "ex_class6 dtor this : " << this << '\n';
	}
};

int main()
{
	ex_class6 ex;
	std::cout << "ex = " << &ex << '\n';

	{
		ex_class6 ey = ex;
		std::cout << "ey = " << &ey << '\n';
	}

	std::cout << "Main continues..." << '\n';
	(void)getchar();
}
```
Output:
```
ex_class6 default ctor this : 000000757AD2F734
ex = 000000757AD2F734
ex_class6 copy ctor this : 000000757AD2F754
ey = 000000757AD2F754
ex_class6 dtor this : 000000757AD2F754
Main continues...

ex_class6 dtor this : 000000757AD2F734
```
---

Example:
```cpp
class ex_class7 {
public:
	ex_class7()
	{
		std::cout << "ex_class7 default ctor this : " << this << '\n';
	}

	ex_class7(const ex_class7& other)
	{
		std::cout << "ex_class7 copy ctor this : " << this << '\n';
		std::cout << "&other = " << &other << '\n';
	}

	~ex_class7()
	{
		std::cout << "ex_class7 dtor this : " << this << '\n';
	}
};

int main()
{
	ex_class7 ex;
	std::cout << "ex = " << &ex << '\n';

	{
		ex_class7 ey = ex;
		std::cout << "ey = " << &ey << '\n';
	}

	std::cout << "Main continues..." << '\n';
	(void)getchar();
}
```
Output:
```
ex_class7 default ctor this : 00000020182FF784
ex = 00000020182FF784
ex_class7 copy ctor this : 00000020182FF7A4
&other = 00000020182FF784
ey = 00000020182FF7A4
ex_class7 dtor this : 00000020182FF7A4
Main continues...

ex_class7 dtor this : 00000020182FF784
```
---
The default **copy ctor** that the compiler writes is a public, non-static, in-line member function.
```cpp
class MyClass {
public:
	MyClass() : tx(), ux(), wx() {}

private:
	T tx;
	U ux;
	W wx;
};
```
## Rule :
If a situation that violates the rules of the language (syntax error) occurs when the compiler attempts to implicitly declare and define a special member function of a class, the compiler will declare the default special member function as deleted.

### What syntax error can occur?
- elements cannot be default initialized
- one of the elements is const or reference
- one of the elements is initialized as default and there is no default constructor
- the default constructor of one of the elements is private
- the default constructor of one of the elements was deleted

### Examples of the Rule:

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
