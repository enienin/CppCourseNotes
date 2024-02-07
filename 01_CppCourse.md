## _Undefined Behavior in C and C++_

#### 1. Division by zero
```sh
    int val = 5;
    return val / 0;			// undefined behavior
```

#### 2. Memory accesses outside of array bounds
```sh
    int arr[4] = { 1,2,3,4 };
    return arr[5];			// undefined behavior
```

#### 3. Signed integer overflow
```sh
    int x = INT_MAX;		// INT_MAX is the maximum possible integer
    printf("%d", x + 1);	// undefined behavior
```

#### 4. Null pointer dereference
```sh
    val = 0;
    int ptr = *val;		// undefined behavior
```

#### 5. Modification of a string literal
```sh
    char* s = "eni";		// stored in read-only section of memory
    s[0] = 'e';         // undefined behavior
```

#### 6. Accessing a NULL pointer
```sh
    int* ptr;
    printf("%d", *ptr);		// undefined behavior
```

#### 7. Misuse of the delete operator with an array allocated using new[]
When you **allocate** an array with new[], you must **deallocate** it with delete[]. 
Using delete on an array can lead to partial deallocation of the array, potentially causing a **memory leak**.
```sh
    auto p = new int[n];	  // dynamically allocates memory for an array of 'n' integers
    delete p;				     // undefined behavior
```

#### 8. Undefined behavior in bitwise shifting
- Shifting more bits than size of type.
 ```sh
    unsigned int a = 1;
    unsigned int result = a << 32;
```
- Shifting a 32-bit integer by 32 or more bits is undefined behavior.
```sh
    int a = 1;
    int shift_amount = -1;
    int result = a << shift_amount;
```
- Shifting by a Negative amount is undefined behavior.
```sh
    int a = 1;
    int shift_amount = -1;
    int result = a << shift_amount;
```
- Left-shifting a negative value is undefined behavior.
```sh
    int a = -1;
    int result = a << 1;
```
- Shifting into the sign bit is undefined behavior.
```sh
    int a = 1 << 30;
    int result = a << 1;
```
