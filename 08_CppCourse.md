## Revision

Example:
```cpp
class MyClass {
public: 
	void func();		// it has a hidden parameter, void func(MyClass *)
	void foo()const;	// void func(const MyClass *)
};

int main()
{
	MyClass m;
	m.func();	// sends the address of 'm' object as a parameter to the func() function

	m.foo();

	const MyClass n;
	n.foo();	
}
```

Example:
```cpp
class MyClass2 {
public:
	void func();
	void foo()const;
	int mx;
};

void f(MyClass2* p)
{
	p->mx = 5;	// ok
}

void f2(const MyClass2* p)
{
	p->mx = 5;	// syntax error : expression must be a modifiable lvalue
}
```
---
## Mutable Keyword
Sometimes there is requirement to modify one or more data members of class / struct through const function even though you don’t want the 
function to update other members of class / struct. This task can be easily performed by using mutable keyword.

Example:
```cpp
class Fighter {
public:
	void print()const
	{
		m_call_count++;		// print() is a const function, syntax-error
		mtx.lock();

		mtx.unlock();
	}
	
private:
	int m_age;
	int m_power;
	//
	int m_call_count;

	// internal synchronization (using mutex) to make it thread-safe
	// std::mutex mtx;
	mutable std::mutex mtx;
};
```

Example:
```cpp
class Fighter {
public:
	void print()const
	{
		m_call_count++;  // no more a syntax error after adding 'mutable'
		mtx.lock();

		mtx.unlock();
	}
	
private:
	int m_age;
	int m_power;
	//
	mutable int m_call_count;

	// internal synchronization (using mutex) to make it thread-safe
	// std::mutex mtx;
	mutable std::mutex mtx;
};
```

Example:
```cpp
class Fighter2 {
public:
	
	void func()const;	// const Fighter2 *
	void func();		// function overloading
};
```
It is the same as this Function Overloading:
```cpp
void foo(Fighter2*);
void foo(const Fighter2*);
```

Example:
```cpp
class Eni {
public:
	void func()const;
	void func();
};

int main() {
	const Eni myen;
	myen.func();	// void func()const; will be called for const objects
}
```

Example:
```cpp
using namespace std;

int main()
{
	vector<int> vec{ 3,6,8,1 };
	vec.at(2) = 5;			// legal
	auto val = vec.at(1);	// legal

	const vector<int> cvec{ 3,6,8,1 };
	cvec.at(2) = 5;			// expression must be a modifiable lvalue
	auto val = cvec.at(1);	// legal
	// 'at' function was overloaded that is why
}
class Vector {
public:
	int& at(std::size_t n);
	const int& at(std::size_t n)const;
};
```

Example:
```cpp
class MyClass {
public: 
	void foo()const;	// accesser
	void bar();			  // mutator
};

int main() 
{
	MyClass myclass;
	
	myclass.foo();

	const MyClass myclass_c;
	myclass_c.bar();		// syntax-error
}
```
Output:
> Error 'void MyClass::bar(void)': cannot convert 'this' pointer from 'const MyClass' to 'MyClass &'

## this keyword
It is used only for non-static class members.

this : value category : PR Value Expression

Example:
```cpp
class MyClass4 {
public:
	void func();
	void foo();
private:
	int mx;
	int my;
};

void MyClass4::func()		// func(MyClass4 *this)
{
	// no difference in meaning between these three
	this->mx = 4;
	mx = 4;
	MyClass4::mx = 4;

	// no difference in meaning between these three
	foo();
	this->foo();
	MyClass4::foo();
}
```

Example:
```cpp
class MyClass5 {
public:
	void func();
	void foo();
private:
	int mx;
	int my;
};

void f(MyClass5*);
void g(MyClass5&);

void MyClass5::func()
{
	f(this);
	g(*this);
}
```
---
## FLUENT API

A Fluent API is a method for designing object oriented interfaces in a way that provides more readable code. 
It is achieved by using method chaining to relay the instruction context of a subsequent call. 
Typically, a Fluent API will have a series of methods in a class which return the object itself (*this) or a copy of the object 
so that the method calls can be chained together in a single statement.

```cpp
class MyClass6 {
public:
	MyClass6& foo();
	MyClass6& bar();
	MyClass6& baz();
private:
};

MyClass6& MyClass6::foo()
{

	return *this;
}

MyClass6& MyClass6::bar()
{

	return *this;
}

MyClass6& MyClass6::baz()
{

	return *this;
}

int main() {

	MyClass6 m;
	m.foo().bar().baz();
}
```
Explanation:
```m.foo()``` is called first. Whatever ```foo()``` does is performed, and then ```foo()``` returns a reference to ```m```.
Because ```foo()``` returned a reference to ```m```, calling ```.bar()``` immediately after is equivalent to calling ```m.bar()```. 
```bar()``` then does its operation and returns a reference to ```m``` again.
Finally, ```.baz()``` is called on the result of ```m.bar()```, which is still ```m```, so it is effectively calling ```m.baz()```. 
After ```baz()``` completes, the chain ends (though it could continue if there were more methods to chain).

---

Example:
```cpp
class MyClass5 {
public:
	void func();
	void foo();
private:
	int mx;
	int my;
};

void f(MyClass5*);
void g(MyClass5&);

void MyClass5::func()
{
	f(this);
	g(*this);
}
```

Example:
```cpp
class MyClass7 {
public:
	MyClass7* foo();
	MyClass7* bar();
	MyClass7* baz();
private:
};

MyClass7* MyClass7::foo()
{

	return this;
}

MyClass7* MyClass7::bar()
{

	return this;
}

MyClass7* MyClass7::baz()
{

	return this;
}

int main() {

	MyClass7 m;
	auto p = &m;

	p->foo()->bar()->baz();
}
```

Example:
```cpp
int main() {

	int x = 5;
	int y = 6;
	double z = 4.5;

	cout << x << y << z << endl;
	cout.operator<<(x).operator<<(y).operator<<(z).operator<<(endl);
}
```

Example:
```cpp
class MyClass8 {
public:
	void foo();
	void bar()const;
};

void MyClass8::bar()const {

	*this; 
}
```
## ODR - One Definition Rule

Example:
```cpp
// furkan.cpp :

	void foo(int x)
	{

	}

// bora.cpp :

	void foo(int x)
	{

	}
```
> Not a Syntax-Error but ILL-FORMED

Example:
```cpp

```




Example:
```cpp

```
