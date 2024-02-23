## Name Mangling
C++ supports function overloading, i.e., there can be more than one function with the same name but, different parameters. 
How does the C++ compiler distinguish between different functions when it generates object code – it changes names by adding information about arguments. 
This technique of adding additional information to function names is called **Name Mangling**.

## Linkage Error  in C++

```cpp
int foo(int);

int main() 
{
	int x = 6;
	auto y = foo(x);
	std::cout << "y = " << y << '/n';
}
```

```Error	LNK2019	unresolved external symbol "int __cdecl foo(int)" (? foo@@YAHH@Z)```
The compiler can compile this code without errors because it sees the declaration  of **foo(int)** and assumes you will link the definition of **foo(int)** provided elsewhere. 
However, during the linking phase, the linker cannot find a definition for **foo(int)**, resulting in a **linkage error**.

## Conditional Compiling

```cpp
#ifdef __cplusplus
int foo(int);
#endif

int main() {
	auto x = foo(3);
}
```

```cpp
#ifdef __cplusplus
	extern "C" {
#endif

	int f1(int);
	int f2(int);
	int f3(int);
	int f4(int);
	int f5(int);

#ifdef __cplusplus
	}
#endif
```

## Function Deletion

Example :
```cpp
int foo(int) = delete;

int main()
{
	foo(3);		// error : attempting to reference a deleted function
}
```
It is a `compile-time operation` that tells the compiler there is a function that returns int and takes one int parameter but calling this function causes syntax error.


Example :
```cpp
int foo2(int) = delete;
int foo2(unsigned);

int main()
{
	foo2(2.5);		// ambiguity
}
```

Example :
```cpp
int foo(float) = delete;
int foo(double) = delete;
int foo(long double) = delete;
int foo(int);

int main()
{
	foo(2.3);	// Error	C2280	'int foo(double)': attempting to reference a deleted function

}
```

Example :
```cpp
int foo(int);

int main()
{
	foo(2.3);	// legal but there will be narrowing conversion
}
```

Example :
```cpp
class Nec {
public:
	void func(int) = delete;
	void func(double);
};

int main()
{
	Nec mynec;
	mynec.func(23.45);	// valid
	mynec.func(23);		// invalid
}
```
---
## Constructors

* The primary purpose of a constructor is to initialize objects of a class. This initialization may involve setting initial values for member variables, allocating resources, or setting up any other state necessary for the newly created object to be used.

Example :
```cpp
class Fighter {

private:
	std::string m_name;	// Constructors initialize the non-static member variables of a class
	int m_age;
	int m_power;
	//
};
```

* The Constructor of a class must have the same name as the class.
  
* The Constructor is a function that has no 'concept' of return value.

Example :
```cpp
class Eni {
public:
	void Eni(int);		// syntax error because ctor has no concept of return value
};
```
* The Constructor cannot be a free function (global function).
* The Constructor cannot be a static member function or const member function.
* 'this' can be used in its definition.
* Can be public, private, protected.

Example :
```cpp
class Eni2 {
private:
	Eni2(int);
};

int main() {
	Eni2 myeni(4);	// cannot access private member declared in class Eni2
}
```
  
* Constructor can be overloaded.

Example :
```cpp
// The constructor has 5 overloads.
class ex_class_2 {
private:
	ex_class_2();
	ex_class_2(int);
	ex_class_2(double);
	ex_class_2(int,int);
	ex_class_2(int,int,int);
};
```
* Cannot be called using . or -> operators

Example :
```cpp
class ex_class_12 {
public:
	ex_class_12();
	ex_class_12(int x);
	void foo();
};

int main()
{
	ex_class_12 myclass;
	myclass.foo();			// ok
	myclass.ex_class_12(12);	// error, cannot call ctor with . operator
}
```

Example :
```cpp
class ex_class_1 {
private:
	ex_class_1(int);

	void func()
	{
		ex_class_1 ex_class(12);	// valid
	}
};
```

## Default Constructor

A default constructor is a constructor that can be called with `no arguments` or `every parameter has a default value`.

Examples of Default Constructors :
```cpp
class ex_class_3 {
private:
	ex_class_3();	  // defalt constructor
};
```
```cpp
class ex_class_4 {
private:
	ex_class_4(int x);	// not a defalt constructor
};
```
```cpp
class ex_class_5 {
private:
	ex_class_5(int x = 10);	// defalt constructor
};

```

Example :
```cpp
// example.h
class ex_class_9 {
private:
	ex_class_9(int);		// constructor declaration
};

// example.cpp
// #include <example.h>
ex_class_9::ex_class_9(int x)		// constructor definition
{
	//..
}
```

Example :
```cpp
// example.h
class ex_class_9 {
private:
	ex_class_9(int);		          // constructor declaration
};

// example.cpp
// #include <example.h>
ex_class_9::ex_class_9(int x)		// constructor definition
{
	//..
}
```

## Special Member Functions
The codes of these functions (when certain conditions are met) can be written by the compiler.
* default ctor
* destructor
* copy ctor
* move ctor			C++11
* copy assignment
* move assignment		C++11

## Revision on ODR

Example 1 :
```cpp
// example.h
class ex_class_7 {
private:
	void func(int x);
};

void ex_class_7::func(int)
{
	//
}
```
If i include this header in 2 files, `ODR is violated`.


Example 2 :
```cpp
// example.h
class ex_class_6 {
private:
	void func(int x)
	{
		//
	}
};
```
If i include this header in 10 files, `ODR is not violated`.

Example 3 :
```cpp
// example.h
class ex_class_8 {
private:
	inline void func(int x);
};

void ex_class_8::func(int)	// or inline void ex_class_8::func(int)
{
	//
}

```
If i include this header in 2 files, `ODR is not violated`

## Destructor

* Used to end the life of the object of a class.
* The Destructor is a non-static member function.
* The Destructor of a class must have the same name as the class but ~ is put in front of the function.

Example :
```cpp
class ex_class_10 {
private:
	ex_class_10(int x);	// constructor
	~ex_class_10();		// destructor
};
```

* The Destructor is a function that has no 'concept' of return value.
* The Destructor cannot be a free function (global function).
* The Destructor cannot be a static member function or const member function.
* 'this' can be used in its definition.
* Can be public, private, protected.
* Destructor CANNOT be overloaded.
* Destructor must have a 'void' parameter list
* Destructor can be called using . or -> operators

Example :
```cpp
class ex_class_12 {
public:
	ex_class_12();
	ex_class_12(int x);
	void foo();
};

int main()
{
	ex_class_12 myclass;
	myclass.foo();	// ok
	myclass.ex_class_12(12);	// error, cannot call ctor with . operator
	myclass.~ex_class_12();		// ok, can call dtor with . operator but it is not very common 
}
```

## Storage Classes 
* static storage class
* automatic storage class
* dynamic storage class
* thread-local storage class

The following classes have **static life-span**:
- global variables
- static local variables
- static data members of a class

Example :
```cpp
class ex_class_13{
public:
	ex_class_13()
	{
		std::cout << "ex_class_13() this = " << this << '\n';
	}

	~ex_class_13()
	{
		std::cout << "ex_class_13 destructor this = " << this << '\n';
	}
};

ex_class_13 gex_class;

int main()
{
	std::cout << "main starting\n";
	std::cout << "&gex_class = " << &gex_class << "\n";
	std::cout << "main continuing\n";
	std::cout << "main ending\n";
}
```
Output:
```
ex_class_13() this = 00007FF6267A1180
main starting
&gex_class = 00007FF6267A1180
main continuing
main ending
ex_class_13 destructor this = 00007FF6267A1180
```
---

Example :
```cpp
class ex_class_14 {
public:
	ex_class_14()
	{
		std::cout << "ex_class_14() this = " << this << '\n';
	}

	~ex_class_14()
	{
		std::cout << "ex_class_14 destructor this = " << this << '\n';
	}
	void foo()
	{
		//..
	}

	void bar()
	{
		//..
	}

int main()
{
	std::cout << &ex_class_14::foo << '\n';		// these functions also have an address in memory
	std::cout << &ex_class_14::bar << '\n';
}

```
---

When a class has multiple constructors, the specific constructor called during object creation depends on the arguments provided when the object is instantiated.

Example :
```cpp
class ex_class_15 {
public:
	ex_class_15()
	{
		std::cout << "ex_class_15() this = " << this << '\n';
	}

	ex_class_15(int)
	{
		std::cout << "ex_class_15(int) this = " << this << '\n';
	}

	~ex_class_15()
	{
		std::cout << "ex_class_15 destructor this = " << this << '\n';
	}
};

ex_class_15 gex_class;

int main()
{
	std::cout << "main starting\n";
	std::cout << "&gex_class = " << &gex_class << "\n";
	std::cout << "main continuing\n";
	std::cout << "main ending\n";
}
```
Output:
```
ex_class_15() this = 00007FF792701200
main starting
&gex_class = 00007FF792701200
main continuing
main ending
ex_class_15 destructor this = 00007FF792701200
```
---

Example :
```cpp
class ex_class_16 {
public:
	ex_class_16()
	{
		std::cout << "ex_class_16() this = " << this << '\n';
	}

	ex_class_16(int)
	{
		std::cout << "ex_class_16(int) this = " << this << '\n';
	}

	~ex_class_16()
	{
		std::cout << "ex_class_16 destructor this = " << this << '\n';
	}
};

ex_class_16 gex_class(5);

int main()
{
	std::cout << "main starting\n";
	std::cout << "&gex_class = " << &gex_class << "\n";
	std::cout << "main continuing\n";
	std::cout << "main ending\n";
}
```
Output:
```
ex_class_16(int) this = 00007FF6BFC11200
main starting
&gex_class = 00007FF6BFC11200
main continuing
main ending
ex_class_16 destructor this = 00007FF6BFC11200
```
---

Example :
```cpp
class ex_class_17 {
public:
	ex_class_17()
	{
		std::cout << "ex_class_17() this = " << this << '\n';
	}

	~ex_class_17()
	{
		std::cout << "ex_class_17 destructor this = " << this << '\n';
	}
};

void foo()
{
	static ex_class_17 x;

}

int main()
{
	std::cout << "main starting\n";
	foo();
	std::cout << "main ending\n";
}

```
Output:
```
main starting
ex_class_17() this = 00007FF7746D0200
main ending
ex_class_17 destructor this = 00007FF7746D0200
```
---

Example :
```cpp
class ex_class_18 {
public:
	ex_class_18()
	{
		std::cout << "ex_class_18() this = " << this << '\n';
	}

	~ex_class_18()
	{
		std::cout << "ex_class_18 destructor this = " << this << '\n';
	}
};

void foo()
{
	std::cout << "foo called\n";
	static ex_class_18 x;
}

int main()
{
	std::cout << "main starting\n";
	foo();		// constructor is called only once
	foo();
	foo();
	foo();
	foo();
	std::cout << "main ending\n";
}
```
Output:
```
main starting
foo called
ex_class_18() this = 00007FF65B530200
foo called
foo called
foo called
foo called
main ending
ex_class_18 destructor this = 00007FF65B530200
```
---
Example :
```cpp
class ex_class_19 {
public:
	ex_class_19()
	{
		std::cout << "ex_class_19() this = " << this << '\n';
	}

	~ex_class_19()
	{
		std::cout << "ex_class_19 destructor this = " << this << '\n';
	}
	char buf[256]{};
};

ex_class_19 ar[16];

int main()
{
	std::cout << "main starting\n";
	std::cout << "main ending\n";
}
```
Output:
```
ex_class_19() this = 00007FF6660C1200
ex_class_19() this = 00007FF6660C1300
ex_class_19() this = 00007FF6660C1400
ex_class_19() this = 00007FF6660C1500
ex_class_19() this = 00007FF6660C1600
ex_class_19() this = 00007FF6660C1700
ex_class_19() this = 00007FF6660C1800
ex_class_19() this = 00007FF6660C1900
ex_class_19() this = 00007FF6660C1A00
ex_class_19() this = 00007FF6660C1B00
ex_class_19() this = 00007FF6660C1C00
ex_class_19() this = 00007FF6660C1D00
ex_class_19() this = 00007FF6660C1E00
ex_class_19() this = 00007FF6660C1F00
ex_class_19() this = 00007FF6660C2000
ex_class_19() this = 00007FF6660C2100
main starting
main ending
ex_class_19 destructor this = 00007FF6660C2100
ex_class_19 destructor this = 00007FF6660C2000
ex_class_19 destructor this = 00007FF6660C1F00
ex_class_19 destructor this = 00007FF6660C1E00
ex_class_19 destructor this = 00007FF6660C1D00
ex_class_19 destructor this = 00007FF6660C1C00
ex_class_19 destructor this = 00007FF6660C1B00
ex_class_19 destructor this = 00007FF6660C1A00
ex_class_19 destructor this = 00007FF6660C1900
ex_class_19 destructor this = 00007FF6660C1800
ex_class_19 destructor this = 00007FF6660C1700
ex_class_19 destructor this = 00007FF6660C1600
ex_class_19 destructor this = 00007FF6660C1500
ex_class_19 destructor this = 00007FF6660C1400
ex_class_19 destructor this = 00007FF6660C1300
ex_class_19 destructor this = 00007FF6660C1200
```
---

Example :
```cpp
class ex_class_20{
public:
	ex_class_20()
	{
		std::cout << "ex_class_20() this = " << this << '\n';
	}

	~ex_class_20()
	{
		std::cout << "ex_class_20 destructor this = " << this << '\n';
	}
	char buf[256]{};
};

void foo()
{
	ex_class_20 x;
}		// because x  is automatic storage class, the life of the object ends here so destructor is called

int main()
{
	std::cout << "main starting\n";
	foo();
	std::cout << "main ending\n";
}
```
Output:
```
main starting
ex_class_20() this = 0000001DC90FF6B0
ex_class_20 destructor this = 0000001DC90FF6B0
main ending
```
---
Example :
```cpp
class ex_class_21 {
public:
	ex_class_21()
	{
		std::cout << "ex_class_21() this = " << this << '\n';
	}

	~ex_class_21()
	{
		std::cout << "ex_class_21 destructor this = " << this << '\n';
	}
	char buf[256]{};
};

void foo()
{
	ex_class_21 x;
}		// because x  is automatic storage class, the life of the object ends here so destructor is called

int main()
{
	std::cout << "main starting\n";
	foo();
	foo();
	foo();
	std::cout << "main ending\n";
}
```
Output:
```
main starting
ex_class_21() this = 000000B26155F6A0
ex_class_21 destructor this = 000000B26155F6A0
ex_class_21() this = 000000B26155F6A0
ex_class_21 destructor this = 000000B26155F6A0
ex_class_21() this = 000000B26155F6A0
ex_class_21 destructor this = 000000B26155F6A0
main ending
```
---

Example :
```cpp
class ex_class_22 {
public:
	ex_class_22()
	{
		std::cout << "ex_class_22() this = " << this << '\n';
	}

	~ex_class_22()
	{
		std::cout << "ex_class_22 destructor this = " << this << '\n';
	}
};

int main()
{
	std::cout << "main starting\n";
	for (int i = 0; i < 10; ++i)
	{	
		ex_class_22 myclass;	// ctor and dtor will be called 10 times
	}
	std::cout << "main ending\n";
}
```
Output:
```
main starting
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
ex_class_22() this = 000000F1045BF934
ex_class_22 destructor this = 000000F1045BF934
main ending
```
---

Example :
```cpp
class ex_class_23 {
public:
	ex_class_23()
	{
		std::cout << "ex_class_23() this = " << this << '\n';
	}

	~ex_class_23()
	{
		std::cout << "ex_class_23 destructor this = " << this << '\n';
	}
};

int main()
{
	std::cout << "main starting\n";
	for (int i = 0; i < 10; ++i)
	{
		std::cout << "i = " << i << '\n';

		static ex_class_23 myclass;		// ctor and dtor will be called once
	}
	std::cout << "main ending\n";
}
```
Output:
```
main starting
i = 0
ex_class_23() this = 00007FF6A16C0200
i = 1
i = 2
i = 3
i = 4
i = 5
i = 6
i = 7
i = 8
i = 9
main ending
ex_class_23 destructor this = 00007FF6A16C0200
```
---

Example :
```cpp
class ex_class_24 {
public:
	ex_class_24()
	{
		std::cout << "ex_class_24() this = " << this << '\n';
	}

	~ex_class_24()
	{
		std::cout << "ex_class_24 destructor this = " << this << '\n';
	}
};

int main()
{
	ex_class_24 myclass;		// constructor called only once
	ex_class_24* ptr = &myclass;
}
```
Output:
```
ex_class_24() this = 0000005C64CFF9B4
ex_class_24 destructor this = 0000005C64CFF9B4
```
---
Example :
```cpp
class ex_class_25 {
public:
	ex_class_25()
	{
		std::cout << "ex_class_25() this = " << this << '\n';
	}

	~ex_class_25()
	{
		std::cout << "ex_class_25 destructor this = " << this << '\n';
	}
};

int main()
{
	ex_class_25 myclass;		// only one object created
	ex_class_25& r1 = myclass;	// this doesn't create an object
	ex_class_25& r2 = myclass;
	ex_class_25& r3 = myclass;
	ex_class_25& r4 = myclass;
}
```
Output:
```
ex_class_25() this = 00000037DBEFF804
ex_class_25 destructor this = 00000037DBEFF804
```
---

Example: Print numbers from 1 to 100 without using loops.
```cpp
class ex_class_26 {
public:
	ex_class_26()
	{
		static int x = 0;
		std::cout << "x = " << ++x << '\n';
	}
};

int main()
{
	ex_class_26 arr[100];	// creates 100 objects of ex_class_26 class
}
```
Output:
```
x = 1
x = 2
x = 3
x = 4
x = 5
x = 6
x = 7
x = 8
x = 9
x = 10
x = 11
x = 12
...
x = 94
x = 95
x = 96
x = 97
x = 98
x = 99
x = 100
```

Example :
```cpp
class Logger {
public:
	Logger()
	{
		mf = std::fopen("logger.txt", "w");
		///...
	}
	~Logger()
	{
		fclose(mf);
	}
	void foo()
	{
		std::fprintf(mf, "foo called\n");
	}

	void bar()
	{
		std::fprintf(mf, "bar called\n");
	}
private:
	FILE* mf;
};

void foo()
{
	Logger lg;	// file opens when the object is created

	lg.foo();
	lg.bar();
}				// life of object finishes here

int main()
{
	foo();
	// at this point, file is closed
}
```

Example :
```cpp
struct ex_class_27 {
	ex_class_27()
	{
		std::cout << "default ctor\n";
	}

	void print()const
	{
		std::cout << "mx = " << mx << "\n";
	}
	int mx;
};

int main()
{
	ex_class_27 myclass1;	// default initialization
	// default ctor is called for default initialized objects


	ex_class_27 myclass2{};	// value initialization
	// default ctor is called for value initialized objects

	
	myclass1.print();		// undefined behavior because mx has garbage value
	myclass2.print();		// zero-initialized, no undefined behavior here, mx = 0

}
```

Example :
```cpp
struct ex_class_28 {
	void print()const
	{
		std::cout << "mx = " << mx << "\n";
	}
	int mx;
};

ex_class_28 myclass;		// default initialization
				// no undefined-behavior because the object has static life-span
				//! static objects, before initialization, get zero-initialized
int main()
{
	myclass.print();
}
```

Example :
```cpp
struct ex_class_29 {
public:
	ex_class_29(int x)
	{
		std::cout << "ex_class_29(int x) x = " << x << "this = " << this << "\n";
	}
};

int main()
{
	ex_class_29 class1(45);		// direct initialization
	std::cout << "&class1 = " << &class1 << '\n';	// class1 and this have same address
}
```

Example :
```cpp
struct ex_class_30 {
public:
	ex_class_30(int x)
	{
		std::cout << "ex_class_30(int x) x = " << x << "this = " << this << "\n";
	}
};

int main()
{
	ex_class_30 class1{67};		// direct list initialization
	std::cout << "&class1 = " << &class1 << '\n';
}
```

Example :
```cpp
struct ex_class_31 {
public:
	ex_class_31(int x)
	{
		std::cout << "ex_class_31(int x) x = " << x << "this = " << this << "\n";
	}
};

int main()
{
	ex_class_31 class1 = 10;	// copy initialization
}
```

Example :
```cpp
struct ex_class_32 {
public:
	ex_class_32()
	{
		std::cout << "ex_class_32 this = " << this << "\n";
	}

	ex_class_32(int x)
	{
		std::cout << "ex_class_32(int x) x = " << x << "this = " << this << "\n";
	}
};

int main()
{
	ex_class_32 class1;		// default initialization
	ex_class_32 class2{};		// value initialization
	ex_class_32 class3(12);		// direct initialization
	ex_class_32 class4{36};		// direct list initialization
	ex_class_32 class5 = 65;	// copy initialization
}
```

Example :
```cpp
struct ex_class_33 {
public:
	ex_class_33()
	{
		std::cout << "ex_class_33 this = " << this << "\n";
	}

	ex_class_33(int x)
	{
		std::cout << "ex_class_33(int x) x = " << x << "this = " << this << "\n";
	}

	ex_class_33(double x)
	{
		std::cout << "ex_class_33(double x) x = " << x << "this = " << this << "\n";
	}

	ex_class_33(int x, int y)
	{
		std::cout << "ex_class_33(int x, int y) x = " << x << "y = " << y << "this = " << this << "\n";
	}
};

int main()
{
	ex_class_33 class1;		// default ctor called
	ex_class_33 class2(45);		// ex_class_33(int x) called
	ex_class_33 class3(4.5f);	// ex_class_33(double x) called
	ex_class_33 class4(4.5L);	// syntax error, ambiguity
	
	ex_class_33 class5 = { 3,5 };	// ex_class_33(int x, int y) called
}
```

Example :
```cpp
class ex_class_34{

public:
	~ex_class_34()
	{
		std::cout << "destructor\n";
	}
};

int main()
{
	ex_class_34 myclass;	// destructor gets called
				// what about constructor? :o
				// default constructor called which was written by compiler
}
```

Example :
```cpp
class ex_class_35 {

public:
	ex_class_35() = default;		// i ask the compiler to write the constructor
	~ex_class_35()
	{
		std::cout << "destructor\n";
	}
};

int main()
{
	ex_class_35 myclass;
}
```

Example :
```cpp
class ex_class_36 {

public:
	ex_class_36(int) {};

};

int main() {
	ex_class_36 myclass;	// syntax error : no appropriate default constructor available
}
```
Output:
> syntax error : no appropriate default constructor available

---

## Special Member Functions
Special member functions can be:

**1. User Declared**
   
   **- defined**
```cpp
class ex_class_37 {

public:
	ex_class_37();				// user declared defined

};
```

   **- defaulted**
```cpp
class ex_class_38 {

public:
	ex_class_38() = default;	// user declared defaulted
	// i am declaring it, but the compiler will define it
};
```
   **- deleted**
```cpp
class ex_class_39 {

public:
	ex_class_39() = delete;	// user declared
	// i am declaring it, but calling this function is a syntax error
};
```

**2. implicitly declared**
 
   **- defaulted**

```cpp
class ex_class_40 {
public:
	// this class has default constructor
	// has destructor
	// none is user declared
	// both are implicitly declared
};
```
```cpp
class ex_class_41 {
public:
	// the compiler declares and defines constructor and destructor
	// implicitly declared defaulted
};

int main()
{
	ex_class_41 m;
}
```


  **- deleted**

```cpp
class ex_class_42 {

	const int x;

	// this class has default constructor, it is implicitly declared
};

int main()
{
	ex_class_42 m;		// syntax error, the default constructor cannon be referenced - it is a deleted function
	// implicitly declared deleted
}
```
Output:
> syntax error, the default constructor cannon be referenced - it is a deleted function

**3. not declared**

```cpp
struct ex_struct_1 {
	ex_struct_1(int);	// default constructor is NOT 'user declared'
	// default constructor is NOT 'implicitly declared'
	// default constructor is 'not declared'
};

int main()
{
	ex_struct_1 struct_1;	// error : no default constructor exists
}
```
Output:
> error : no default constructor exists

---

Example :
```cpp
class ex_class_43 {
public:
	ex_class_43()
	{
		mx = 0;		// assignment, not initialization
		my = 0;		// when the program enters the constructor block, member variables have already been initialized
	}

private:
	int mx, my;
};
```

Example :
```cpp
class ex_class_44 {
public:
	ex_class_44()
	{

	}

private:
	int& r;			// syntax error, references do not get default initialized
				// references must be initialized

	const int x;	// const objects do not get default initialized
			// an object of const-qualified type must be initialized
};
```
Output:
> syntax error, references do not get default initialized

---

## Constructor (Member) Initializer List
This is a syntax that only constructors can use.

Example :
```cpp
class ex_class_45 {
public:
	ex_class_45();		// constructor : user declared declaration
private:
	int mx;
	double my;
	//
};

```
```cpp
ex_class_45::ex_class_45() : mx(5), my(7.5)		// valid syntax
{
	//
}
```
```cpp
ex_class_45::ex_class_45() : mx{ 5 }, my{ 7.5 }
{
	// also valid syntax, but narrowing conversion will cause syntax error
}
```
---
Example :

In C++, const members must be initialized when an object is created. Since mx is not given a value in the constructor initializer list, the compiler will generate an error.

```cpp
class ex_class_46 {
public:
	ex_class_46();
private:
	const int mx;
};

ex_class_46::ex_class_46()
{
	// syntax error : an object of const - qualified type must be initialized	
}
```
Output:
> syntax error : an object of const - qualified type must be initialized
---

Example :

In C++, reference members must be initialized when an object is created. This is because references must always refer to an object; they cannot exist without being bound to an object. The constructor in ```ex_class_47``` does not initialize the reference member r, leading to a ```compilation error```.

```cpp
class ex_class_47 {
public:
	ex_class_47();
private:
	int& r;
};

ex_class_47::ex_class_47()
{
	// syntax error : references do not get default initialized
}
```
Output:
> syntax error : references do not get default initialized



Example :
```cpp

```
Output:
```

```
