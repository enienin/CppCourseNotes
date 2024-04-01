# Inheritance

## Construction & Destruction Order
When an object of a derived class ```Der1``` is created, the constructors are called in a **base-to-derived** order. 

Destructors are called in the reverse order of constructors, meaning **from derived back to base**. 

### Example 1
```cpp
class Base1 {
public:
	Base1()
	{
		std::cout << "Base1 default ctor\n";
	}

	~Base1()
	{
		std::cout << "Base1 dtor\n";
	}
};

class Der1 :public Base1 {
public:
	Der1()
	{
		std::cout << "Der1 default ctor\n";

	}
	~Der1()
	{
		std::cout << "Der1 dtor\n";

	}
};

int main()
{
	Der1 myder;
}
```
Output:
```
Base1 default ctor
Der1 default ctor
Der1 dtor
Base1 dtor
```

### Example 2
```cpp
class Member {
public:
	Member()
	{
		std::cout << "Member default ctor\n";
	}

	~Member()
	{
		std::cout << "Member dtor\n";
	}
};

class Base2 {
public:
	Base2()
	{
		std::cout << "Base2 default ctor\n";
	}

	~Base2()
	{
		std::cout << "Base2 dtor\n";
	}

};

class Der2 :public Base2 {
public:
	Der2()
	{
		// at this point, base class Base2 and elements of Der2 have come to life
		std::cout << "Der2 default ctor\n";
	}
	~Der2()
	{
		std::cout << "Der2 dtor\n";
	}

	Member mx;
};

int main()
{
	Der2 myder;
}
```
Output:
```
Base2 default ctor
Member default ctor
Der2 default ctor
Der2 dtor
Member dtor
Base2 dtor
```

### Example 3
```cpp
class Member3 {
public:
	Member3()
	{
		std::cout << "Member3 default ctor\n";
	}

	~Member3()
	{
		std::cout << "Member3 dtor\n";
	}
};

class Base3 {
public:
	Base3()
	{  // at this point, mx class has been instantiated
		std::cout << "Base3 default ctor\n";
	}

	~Base3()
	{
		std::cout << "Base3 dtor\n";
	}

	Member3 mx;
};

class Der3 :public Base3 {
public:
	Der3()
	{
		std::cout << "Der3 default ctor\n";
	}
	~Der3()
	{
		std::cout << "Der3 dtor\n";
	}

};

int main()
{
	Der3 myder;
}
```
Output:
```
Member3 default ctor
Base3 default ctor
Der3 default ctor
Der3 dtor
Base3 dtor
Member3 dtor
```
---

### Example

The default constructor of ```Der4``` is implicitly deleted because its base class ```Base4``` lacks a **default constructor**. 
When a class is derived from another class, the derived class's default constructor attempts to call the base class's default constructor.

```cpp
class Base4 {
public:
	Base4(int);
};

class Der4 :public Base4 {
public:
};

int main()
{
	Der4 myder; // the default constructor of "Der4" cannot be referenced -- it is a deleted function
}
```

### Explicitly calling Base class constructor
### Example 1
```cpp
class Base5 {
public:
	Base5(int x)
	{
		std::cout << "Base5(int x) x = " << x << "\n";
	}
};

class Der5 :public Base5 {
public:
	Der5() : Base5(0) {		// member initializer list
	}
};

int main()
{
	Der5 myder;
}
```
Output:
```
Base5(int x) x = 0
```

### Example 2

```cpp
class Base6 {
public:
	Base6(int x)
	{
		std::cout << "Base6(int x) x = " << x << "\n";
	}
};

class Der6 :public Base6 {
public:
	Der6(int a,int b) : Base6(a),mx(b) {}
private:
	int mx;
};

int main()
{
	Der6 myder(5,10);
}
```
Output:
```
Base6(int x) x = 5
```

---

### Example

```cpp
class Base7 {
public:
	void foo(){}	// has a hidden parameter foo(Base7* p)
};

class Der7 :public Base7 {
public:
};

int main()
{
	Der7 myder;
	myder.foo();
}
```
---

## Copy Constructor

### Example 1

```cpp
class Base8 {
public:
	Base8()
	{
		std::cout << "Base8 default ctor\n";
	}
	
	Base8(const Base8&)
	{
		std::cout << "Base8 copy ctor\n";
	}
};

class Der8 :public Base8 {
public:

};

int main()
{
	Der8 der_x;
	auto der_y = der_x;
}
```
Output:
```
Base8 default ctor
Base8 copy ctor
```

### Example 2

For **copy constructors**, specifically, if you want the base part of an object to be copied using the base class's copy constructor, 
you must **explicitly** call it in the initializer list of the derived class's copy constructor. Otherwise, the base class's **default constructor** is called instead (if available).

```cpp
class Base9 {
public:
	Base9()
	{
		std::cout << "Base9 default ctor\n";
	}

	Base9(const Base9&)
	{
		std::cout << "Base9 copy ctor\n";
	}
};

class Der9 :public Base9 {
public:
	Der9() = default;
	Der9(const Der9&)
	{
		std::cout << "Der9 copy ctor\n";
	}
};

int main()
{
	Der9 der_x;
	auto der_y = der_x;
}
```
Output:
```
Base9 default ctor
Base9 default ctor
Der9 copy ctor
```

### Example 3

```cpp
class Base10 {
public:
	Base10()
	{
		std::cout << "Base10 default ctor\n";
	}

	Base10(const Base10&)
	{
		std::cout << "Base10 copy ctor\n";
	}
};

class Der10 :public Base10 {
public:
	Der10() = default;
	Der10(const Der10& dr) : Base10(dr)
	{
		std::cout << "Der10 copy ctor\n";
	}
};

int main()
{
	Der10 der_x;
	auto der_y = der_x;
}
```
Output:
```
Base10 default ctor
Base10 copy ctor
Der10 copy ctor
```

---

## Move Constructor

### Example 1

When an attempt is made to move an object and no **move constructor** is defined for that object's class (but a copy constructor is available), the copy constructor is called as a ```fallback mechanism```. 

```cpp
class Base11 {
public:
	Base11()
	{
		std::cout << "Base11 default ctor\n";
	}

	Base11(const Base11&)
	{
		std::cout << "Base11 copy ctor\n";
	}
};

class Der11 :public Base11 {
public:
	Der11() = default;
	Der11(const Der11& dr) : Base11(dr)
	{
		std::cout << "Der11 copy ctor\n";
	}
};

int main()
{
	Der11 der_x;
	Der11 der_y = std::move(der_x);	// fallback to copy ctor
}
```
Output:
```
Base11 default ctor
Base11 copy ctor
Der11 copy ctor
```

### Example 2

```cpp
class Base12 {
public:
	Base12()
	{
		std::cout << "Base12 default ctor\n";
	}

	Base12(const Base12&)
	{
		std::cout << "Base12 copy ctor\n";
	}

	Base12(Base12&&)
	{
		std::cout << "Base12 move ctor\n";
	}
};

class Der12 :public Base12 {
public:
	Der12() = default;
};

int main()
{
	Der12 der_x;
	Der12 der_y = std::move(der_x);
}
```
Output:
```
Base12 default ctor
Base12 move ctor
```

### Syntax 

To call the ```move constructor``` of a base class from the ```move constructor``` of a derived class, you use the ```member initializer list``` syntax, specifically by using ```std::move``` 
on the base class portion of the derived class's constructor argument to ensure it is treated as an **rvalue**. 

```cpp
#include <utility> // For std::move

class Base {
public:
    // Base class move constructor
    Base(Base&& other) {
        // Implementation...
    }
    // Other constructors...
};

class Derived : public Base {
public:
    // Derived class move constructor
    Derived(Derived&& other) : Base(std::move(other)) {
        // Implementation...
    }
    // Other constructors...
};
```

### Example 3

```cpp
class Base13 {
public:
	Base13()
	{
		std::cout << "Base13 default ctor\n";
	}

	Base13(const Base13&)
	{
		std::cout << "Base13 copy ctor\n";
	}

	Base13(Base13&&)
	{
		std::cout << "Base13 move ctor\n";
	}
};

class Der13 :public Base13 {
public:
	Der13() = default;
	
	//! if we write the move ctor ourselves
	Der13(Der13&& dr):Base13(std::move(dr)){}
};

int main()
{
	Der13 der_x;
	Der13 der_y = std::move(der_x);
}
```
Output:
```
Base13 default ctor
Base13 move ctor
```

---

## Copy Assignment





```cpp

```
Output:
```

```
