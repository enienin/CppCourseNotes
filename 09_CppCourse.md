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

* The primary purpose of a constructor is to initialize objects of a class.
* This initialization may involve setting initial values for member variables, allocating resources, or setting up any other state necessary
* for the newly created object to be used.

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




Example :
```cpp

```
