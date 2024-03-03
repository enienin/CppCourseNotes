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

### Example 1 :

In C++, a ```constant member variable``` must be initialized when it is **declared**, or through a **Constructor Initializer List**.

```cpp
// default constructor is deleted
// implicitly declared deleted
class ex_class8 {

	const int x;
};

// there will be syntax error when this deleted function is called
int main() {
	ex_class8 ex;
}
```
Output:
```
Error : the default ctor of "ex_class8" cannot be referenced - it is a deleted function
```


### Example 2 :

Similar to const members, ```reference members``` must be initialized when an object is created. 
This requirement exists because references in C++ must refer to an object, they cannot be null and they cannot be 
re-assigned to refer to a different object after initialization.

In this example, the default constructor is deleted. 
The default constructor is ```implicitly declared deleted```.

```cpp
class ex_class9 {

	int& r;
};

int main() {
	ex_class9 ex;
}
```
Output:
```
Error : the default ctor of "ex_class9" cannot be referenced - it is a deleted function
```

### Example 3 :

The error in the provided code stems from the inability to implicitly instantiate the ```ex_class10``` member ```mc``` within ```ex_class11``` due to the lack of a **default constructor** in ```ex_class10```.
The ```ex_class10``` class only defines a constructor that takes an int parameter, and since C++ requires an object to be initialized upon creation, the compiler looks for a default constructor when attempting to instantiate ```mc``` within ```ex_class11```.
Since no such constructor exists for ```ex_class10```, and ```ex_class11``` does not explicitly initialize ```mc``` in its initialization list, the compiler will generate an error.

```cpp
class ex_class10 {
public:
	ex_class10(int);	// ex_class10::ex_class10(void) function definition not found
				// no default constructor
};

class ex_class11 {

	ex_class10 mc;
};

int main()
{
	ex_class11 m;	
}
```
Output:
```
Error : the default constructor of "ex_class11" cannot be referenced -- it is a deleted function
```

### Example 4 :

There is no problem in this code.

```cpp
class ex_class12 {
public:
	ex_class12();
};

class ex_class13 {

	ex_class12 mc;
};

int main()
{
	ex_class13 m;
}
```

### Example 5 :

The issue in the provided code arises because ```ex_class14``` has a ```private default constructor```, which is not accessible from ```ex_class15```. 
Consequently, ```ex_class15``` cannot implicitly use the default constructor of ```ex_class14``` to initialize its member ```mc```. 

C++ class members must be initialized when an instance of the class is created, and since ```ex_class15```'s default constructor implicitly attempts to call the default constructor of ```ex_class14``` for ```mc```, it fails due to the ```access level```.

```cpp
class ex_class14 {
	ex_class14();
};

class ex_class15 {

	ex_class14 mc;
};

int main()
{
	ex_class15 m;	// the default constructor of "ex_class15" cannot be referenced -- it is a deleted function
}
```

### Example 6 :

```cpp
class ex_class16 {
public:
	ex_class16() = delete;
};

class ex_class17 {

	ex_class16 mc;
};

int main()
{
	ex_class17 m;	// the default constructor of "ex_class17" cannot be referenced -- it is a deleted function
}
```

---

## IMPORTANT RULE

**If you declare any constructor to the class, the compiler does not declare a default constructor.**

If you do not explicitly declare any constructor in a class, the compiler will implicitly declare a default constructor for you. However, as soon as you declare a constructor that takes one or more parameters, the compiler does not implicitly declare a default constructor anymore.

Example:
```cpp
class ex_class18 {
public:
	ex_class18(int);	// default constructor not declared
};
```

Example:
```cpp
class A {
public:
	A(int);		// doesn't have default constructor, default constructor not declared
};
```

Example:
```cpp
class B {
public:
	B(const B&);	// copy constructor
			// this class has no default ctor, it is NOT implicitly declared
};
```
---

### Is there a difference between writing "C() = default;" and not declaring the ctor at all ?

Yes, there is. Not, on the code written by the compiler, but on the status of the ctor.

```cpp
class C {
public:
	C() = default;	// ctor is user declared 
};
```
```cpp
class D {
public:
	// ctor is implicitly declared
};
```

---

Example:

```cpp
class ex_class18 {
public:
	ex_class18();	// user declared ctor and the definition should be written by us
};

int main()
{
	ex_class18 ex;
}
```

Output:

```
Linkage Error : function definition for "ex_class18" not found.
```

---

Example:

```cpp
class ex_class19 {
public:
	ex_class19() = default;	 // user declared defaulted
};

int main()
{
	ex_class19 ex;
}
```

---

Example:

```cpp
class ex_class20 {
public:
	ex_class20() {};	 // user declared defined 
};

int main()
{
	ex_class20 ex;
}
```

---

Example:

```cpp
class ex_class21 {
public:
	ex_class21() = delete;	// user declared deleted
};

int main()
{
	ex_class21 ex;	// compile time error
}
```
Output:
```
Error : the default constructor of "ex_class21" cannot be referenced -- it is a deleted function
```

---

## How the compiler writes copy ctor?
```
class ex_class22 {
public:
	ex_class22(const ex_class22& other) : tx(other.tx), ux(other.ux), wx(other.wx) {}
private:
	T tx;
	U ux;
	W wx;
};
```

---

## Rule of Zero

If a class does not manage resources directly (i.e., it does not perform custom actions in its destructor, copy constructor, move constructor, copy assignment operator, or move assignment operator), it should not explicitly define these special member functions.

```cpp
class Student {
private:
	std::string name;
	std::vector<int> grades;
};
```
> compiler writes the ctor
> compiler default initializes name and grades as empty str and empty vector

## Value Type

Every object has its own value.
```T x = y;``` there is no connection between 'x' and 'y' after initialization (if T is value type), later if i change 'x' , 'y' won't change and vice versa

---

Example:
```cpp
class ex_class23 {
public:
	ex_class23(int day, int mon, int year) : day_(day), month_(mon), year_(year) {}
	//
	void print()const
	{
		std::cout << day_ << '-' << month_ << '-' << year_ << '\n';
	}
private:
	int day_;
	int month_;
	int year_;
};

int main()
{
	ex_class23 dx{ 11,2,2024 };
	ex_class23 dy = dx;		// copy ctor is called for 'dy'
	dx.print();
	dy.print();
}
```
Output:
```
11-2-2024
11-2-2024
```
> it is not necessary to write the copy constructor ourselves in this case, there is nothing wrong with the copy ctor the compiler writes

---

Example:
```cpp
class ex_class24 {
public:
	ex_class24(int day, int mon, int year) : day_(day), month_(mon), year_(year) {}

	// copy ctor
	ex_class24(const ex_class24& other) : day_(other.day_), month_(other.month_), year_(other.year_) {};

	void print()const
	{
		std::cout << day_ << '-' << month_ << '-' << year_ << '\n';
	}
private:
	int day_;
	int month_;
	int year_;
};

int main()
{
	ex_class24 dx{ 11,2,2024 };
	ex_class24 dy = dx;		// copy ctor is called for 'dy'
	dx.print();
	dy.print();
}
```

---

```Shallow copy``` : both the original and the copied object will point to the same memory locations for their pointer members.

```Deep copy``` : any dynamically allocated memory or resources held by the original object are also allocated a new and copied over for the new object.

In this case, the compiler can write the copy ctor:
```cpp
class Person {
private:
	std::string name;
	std::string surname;
	std::string address;
};
```

In this case, the compiler shouldn't write the copy ctor :
```cpp
class Person2 {
private:
	char* pname;
	char* psurname;
	char* paddress;
};
```
> The compiler-generated versions simply copy the values of each member,including pointer values.
> This means that after a copy, both the original and the copied Person2 objects will have pointers (pname, psurname, paddress)
> pointing to the same memory locations.
---

## Example where the programmer should write the copy ctor

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include<string>
#include<cstdlib>
#include<cstring>

class Sentence {
public:
	Sentence(const char* p) : mlen(std::strlen(p)),
		mp(static_cast<char*>(std::malloc(mlen + 1)))
	{
		if (!mp) {
			std::cerr << "not enough memory\n";
			std::exit(EXIT_FAILURE);
		}
		std::strcpy(mp, p);
	}

	~Sentence()
	{
		if (mp)
			std::free(mp);
	}

	void print()const
	{
		std::cout << '[' << mp << ']' << '\n';
	}

	std::size_t length()const
	{
		return mlen;
	}

private:
	std::size_t mlen;	// length of sentence
	char* mp;	// will hold the address of the sentence
};

int main()
{
	Sentence s1{ "Today is 1st of February." };
	s1.print();
	auto len = s1.length();

	std::cout << "length = " << len << '\n';
	//...
}
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
