# Chapter 2 Variables and Basic Types

### Exercise 2.1

*Q: What are the differences between int, long, long long, and short? Between an unsigned and a signed type? Between a float and a double?*

C++ guarantees `short` and `int` is **at least** 16 bits, `long` **at least** 32 bits, `long long` **at least** 64 bits.

The `signed` can represent positive numbers, negative numbers and zero, while `unsigned` can only represent numbers no less than zero.

Typically, `float` s are represented in 32 bits, `double`s in 64 bits. 

**Usage**:

- Use `int` for integer arithmetic. `short` is usually too small and, in practice,`long` often has the same size as `int`. If your data values are larger than the minimum guaranteed size of an `int`, then use `long long`. (In a word: short <= int <= long <= long long)
- Use an unsigned type when you know that the values cannot be negative. 
- Use double for floating-point computations; float usually does not have enough precision, and the cost of double-precision calculations versus single-precision is negligible. In fact, on some machines, double-precision operations are faster than single. The precision offered by long double usually is unnecessary and often entails considerable run-time cost. (In a word: float <= double <= long double)



### Exercise 2.2

*Q: To calculate a mortgage payment, what types would you use for the rate, principal, and payment? Explain why you selected each type.*

double



### Exercise 2.3

*Q: What output will the following code produce?*

```c++
unsigned u = 10, u2 = 42;
std::cout << u2 - u << std::endl;
std::cout << u - u2 << std::endl;
int i = 10, i2 = 42;
std::cout << i2 - i << std::endl;
std::cout << i - i2 << std::endl;
std::cout << i - u << std::endl;
std::cout << u - i << std::endl;
```

`32 4294967264 32 -32 0 0`



### Exercise 2.4

*Q: Write a program to check whether your predictions were correct. If not, study this section until you understand what the problem is.*

See 2.3



### Exercise 2.5

*Q: Determine the type of each of the following literals. Explain the differences among the literals in each of the four examples:*

(a) 'a', L'a', "a", L"a"

char, wide character literal, string, **string wide character literal**

(b) 10, 10u, 10L, 10uL, 012, 0xC

decimal, unsigned decimal, long decimal, unsigned long decimal, octal, hexadecimal

(c) 3.14, 3.14f, 3.14L

double, float, long double

(d) 10, 10u, 10., 10e-2

decimal, unsigned decimal, double, double.



### Exercise 2.6

*Q: What, if any, are the differences between the following definitions:*

```c++
int month = 9, day = 7; //decimal
int month = 09, day = 07; // month is invalid octal
```



### Exercise 2.7

*Q: What values do these literals represent? What type does each have?*

(a) "Who goes with F\145rgus?\012"

"Who goes with Fergus\n" (string)

(b) 3.14e1L

31.4 (long double)

(c) 1024f

~~1024.0 (float)~~ ERROR: The suffix f is valid only with floating point literal.

(d) 3.14L

3.14 (long double)



### Exercise 2.8

*Q: Using escape sequences, write a program to print 2M followed by a newline. Modify the program to print 2, then a tab, then an M, followed by a newline.*

```c++
#include <iostream>

int main() {
    std::cout << "2\115\12";
    std::cout << "2\t\115\12";
    return 0;
}
```



### Exercise 2.9

*Q: Explain the following definitions. For those that are illegal, explain what’s wrong and how to correct it.*

```c++
(a) std::cin >> int input_value;c
// int input_value;
// std::cin >> input_value;
(b) int i = { 3.14 };
// (C++11)error: conversion from 'double' to 'int' requires a narrowing conversion
// double i = { 3.14 };
(c) double salary = wage = 9999.99;
// error: use of undeclared identifier 'wage'
// double wage;
// double salary = wage = 9999.99;
(d) int i = 3.14;
// ok, but the value will be truncated.
```



### Exercise 2.10

*Q: What are the initial values, if any, of each of the following variables?*

```c++
std::string global_str; // empty string
int global_int; // 0
int main()
{
    int local_int; // undefined value
    std::string local_str; // empty string (value is defined by the class)
}
```



### Exercise 2.11

*Q: Explain whether each of the following is a declaration or a definition:*

```c++
(a) extern int ix = 1024; // definition
(b) int iy; // definition
(c) extern int iz; // declaration
```



### Exercise 2.12

*Q: Which, if any, of the following names are invalid?*

```c++
(a) int double = 3.14; // invalid
(b) int _;
(c) int catch-22; // invalid
(d) int 1_or_2 = 1; // invalid
(e) double Double = 3.14;
```



### Exercise 2.13

*Q: What is the value of j in the following program?*

```c++
int i = 42;
int main()
{
    int i = 100;
    int j = i;
}
```

`100`



### Exercise 2.14

*Q: Is the following program legal? If so, what values are printed?*

```c++
    int i = 100, sum = 0;
    for (int i = 0; i != 10; ++i)
        sum += i;
    std::cout << i << " " << sum << std::endl;
```

Legal, `100 45`



### Exercise 2.15

*Q: Which of the following definitions, if any, are invalid? Why?*

```c++
(a) int ival = 1.01; //valid
(b) int &rval1 = 1.01; //invalid, initializer must be an object
(c) int &rval2 = ival; // valid
(d) int &rval3; // invalid, a reference must be initialized
```



### Exercise 2.16

*Q: Which, if any, of the following assignments are invalid? If they are valid, explain what they do.*

```c++
int i = 0, &r1 = i;  
double d = 0, &r2 = d; 

(a) r2 = 3.14159; // valid
(b) r2 = r1; // valid
(c) i = r2; // valid
(d) r1 = d; // valid
```



### Exercise 2.17

*Q: What does the following code print?*

```c++
int i, &ri = i;
i = 5; ri = 10;
std::cout << i << " " << ri << std::endl;
```

`10 10`



### Exercise 2.18

*Q: Write code to change the value of a pointer. Write code to change the value to which the pointer points.*

```c++
int i = 42;
int *p1 = &i; 
p1++; // change the value of a pointer
*p1 = *p1 * *p1; // cahnge the value to which the pointer points
```



### Exercise 2.19

*Q: Explain the key differences between pointers and references.*

**definition:**

the pointer is "points to" any other type.

the reference is "another name" of an **object**.

**key difference:**

1. a reference is another name of an **already existing** object. a pointer is an object in its **own right**.
2. Once initialized, a reference remains **bound to** its initial object. There is **no way** to rebind a reference to refer to a different object. a pointer can be **assigned** and **copied**.
3. a reference always get the object to which the reference was initially bound. a single pointer can point to **several different objects** over its lifetime.
4. a reference must be initialized. a pointer need **not be** initialized at the time it is defined.



### Exercise 2.20

*Q: What does the following program do?*

```c++
int i = 42;
int *p1 = &i; *p1 = *p1 * *p1;
```

change `i`'s value to (42*42)



### Exercise 2.21

*Q: Explain each of the following definitions. Indicate whether any are illegal and, if so, why.*

```c++
int i = 0;

(a) double* dp = &i; //illegal, error: 'initializing': cannot convert from 'int *' to 'double *'
(b) int *ip = i; // illegal, error: 'initializing': cannot convert from 'int' to 'int *'
(c) int *p = &i; // legal
```



### Exercise 2.22

*Q: Assuming p is a pointer to int, explain the following code:*

```c++
if (p) // whether p is a nullptr
if (*p) // whether the value pointed by p is zero
```



### Exercise 2.23

*Q: Given a pointer p, can you determine whether p points to a valid object? If so, how? If not, why not?*

No. Under most compilers. when we used uninitialized pointer, the bits in the memory in which the pointer resides are used as an address. There is no way to distinguish a valid address from an invalid one formed from the bits that happen to be in the memory in which the pointer was allocated.



### Exercise 2.24

*Q: Why is the initialization of p legal but that of lp illegal?*

```c++
int i = 42;
void *p = &i;
long *lp = &i;
```

`void*` can hold the address of any object. 

types of `*lp` and `i` differ

### Exercise 2.25

*Q: Determine the types and values of each of the following variables.*

```c++
(a) int* ip, i, &r = i; // ip pointer to int, i integer, r reference.
(b) int i, *ip = 0; // i integer, ip nullptr.
(c) int* ip, ip2; // ip pointer to int, ip2 integer.
```



### Exercise 2.26

*Q: Which of the following are legal? For those that are illegal, explain why.*

```c++
const int buf;      // illegal, buf is uninitialized const.
int cnt = 0;        // legal.
const int sz = cnt; // legal.
++cnt;              // legal.
++sz;               // illegal, attempt to write to const object(sz).
```



### Exercise 2.27

*Q: Which of the following initializations are legal? Explain why.*

```c++
int i = -1, &r = 0;         // illegal, r must refer to an object.
int *const p2 = &i2;        // legal.
const int i = -1, &r = 0;   // legal.
const int *const p3 = &i2;  // legal.
const int *p1 = &i2;        // legal
const int &const r2;        // illegal, r2 is a reference that cannot be const.
const int i2 = i, &r = i;   // legal.
```



### Exercise 2.28

*Q: Explain the following definitions. Identify any that are illegal.*

```c++
int i, *const cp;       // illegal, cp must initialize.
int *p1, *const p2;     // illegal, p2 must initialize.
const int ic, &r = ic;  // illegal, ic must initialize.
const int *const p3;    // illegal, p3 must initialize.
const int *p;           // legal. a pointer to const int.
```



### Exercise 2.29

*Q: Using the variables in the previous exercise, which of the following assignments are legal? Explain why.*

```c++
i = ic;     // legal.
p1 = p3;    // illegal. low-level const must match
p1 = &ic;   // illegal. (error: p1 is a plain pointer)
p3 = &ic;   // illegal. p3 is a const pointer. cannot be changed.
p2 = p1;    // illegal. p2 is a const pointer. cannot be changed.
ic = *p3;   // illegal. ic is a const int. cannot be changed.
```



### Exercise 2.30

*Q: For each of the following declarations indicate whether the object being declared has top-level or low-level const.*

```c++
const int v2 = 0; int v1 = v2; // v2: top-level const
int *p1 = &v1, &r1 = v1; // no const
const int *p2 = &v2, *const p3 = &i, &r2 = v2; // p2 low-level const; p3: both low-level and top-level const; r2: low-level const
```



### Exercise 2.31

*Q: Given the declarations in the previous exercise determine whether the following assignments are legal. Explain how the top-level or low-level const applies in each case.*

```c++
r1 = v2; // legal.
p1 = p2; // illegal, p2 has a low-level const but p1 doesn't.
p2 = p1; // legal.
p1 = p3; // illegal, p3 has a low-level const but p1 doesn't.
p2 = p3; // legal.
```



### Exercise 2.32

*Q: Is the following code legal or not? If not, how might you make it legal?*

```c++
int null = 0, *p = null; // *p = &null
```



### Exercise 2.33

*Q: Using the variable definitions from this section, determine what happens in each of these assignments:*

```c++
a=42; // set 42 to int a.
b=42; // set 42 to int b.
c=42; // set 42 to int c.
d=42; // ERROR, d is an int *.
e=42; // ERROR, e is an const int *.
g=42; // ERROR, g is a const int& that is bound to ci.
```



### Exercise 2.34

*Q: Write a program containing the variables and assignments from the previous exercise. Print the variables before and after the assignments to check whether your predictions in the previous exercise were correct. If not, study the examples until you can convince yourself you know ￼￼what led you to the wrong conclusion.*

```c++
#include <iostream>

int main()
{
    int i = 0, &r = i;
    auto a = r;   // a is an int (r is an alias for i, which has type int)

    const int ci = i, &cr = ci;
    auto b = ci; // b is an int (top-level const in ci is dropped)
    auto c = cr; // c is an int (cr is an alias for ci whose const is top-level)
    auto d = &i; // d is an int* (& ofan int objectis int*)
    auto e = &ci; // e is const int*(& of a const object is low-level const)

    const auto f = ci; // deduced type of ci is int; f has type const int
    auto &g = ci; // g is a const int& that is bound to ci

    a = 42; b = 42; c = 42; *d = 42; e = &c;

    return 0;
}
```



### Exercise 2.35

*Q: Determine the types deduced in each of the following definitions. Once you’ve figured out the types, write a program to see whether you were correct.*

```c++
const int i = 42;
auto j = i; // int
const auto &k = i;  // const int&
auto *p = &i;  // const int*
const auto j2 = i, &k2 = i; // j2: const int; k2: const int&
```



### Exercise 2.36

*Q: In the following code, determine the type of each variable and the value each variable has when the code finishes:*

```c++
int a = 3, b = 4;
decltype(a) c = a; // c: int
decltype((b)) d = a; // d: int&
++c; // c=4
++d; // d = a = 4;
```



### Exercise 2.37

*Q: Assignment is an example of an expression that yields a reference type. The type is a reference to the type of the left-hand operand. That is, if i is an int, then the type of the expression i = x is int&. Using that knowledge, determine the type and value of each variable in this code:*

```c++
int a = 3, b = 4;
decltype(a) c = a; // c: int 3
decltype(a = b) d = a; // d: int& 3
```



### Exercise 2.38

*Q: Describe the differences in type deduction between decltype and auto. Give an example of an expression where auto and decltype will deduce the same type and an example where they will deduce differing types.*

When the expression to which we apply `decltype` is a variable, `decltype` returns the type of that variable, including top-level `const` and references.

```c++
int i = 0, &r = i;
auto c = r; // c: int
decltype(r) d = r; // d: int&
```



### Exercise 2.39

*Q: Compile the following program to see what happens when you forget the semicolon after a class definition. Remember the message for future reference.*

```c++
struct Foo { /* empty  */ } // Note: no semicolon
int main()
{
    return 0;
}
```

`[Error] expected a ';'`



### Exercise 2.40

*Q: Write your own version of the Sales_data class.*

```c++
struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}
```



### Exercise 2.41

*Q: Use your Sales_data class to rewrite the exercises in § 1.5.1(p. 22), § 1.5.2(p. 24), and § 1.6(p. 25). For now, you should define your Sales_data class in the same file as your main function.*

1.5.1

```c++
#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main() {
    Sale_data book;
    double price;
    std::cin >> book.bookNo >> book.units_sold >> price;
    book.revenue = book.units_sold * price;
    std::cout << book.bookNo << " " << book.units_sold << " " << book.revenue << " " << price;
    return 0;
}
```



1.5.2

```c++
#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sale_data book1, book2;
    double price1, price2;
    std::cin >> book1.bookNo >> book1.units_sold >> price1;
    std::cin >> book2.bookNo >> book2.units_sold >> price2;
    book1.revenue = book1.units_sold * price1;
    book2.revenue = book2.units_sold * price2;

    if (book1.bookNo == book2.bookNo)
    {
        unsigned totalCnt = book1.units_sold + book2.units_sold;
        double totalRevenue = book1.revenue + book2.revenue;
        std::cout << book1.bookNo << " " << totalCnt << " " << totalRevenue << " ";
        if (totalCnt != 0)
            std::cout << totalRevenue / totalCnt << std::endl;
        else
            std::cout << "(no sales)" << std::endl;
        return 0; // indicate success
    }
    else
    {
        std::cerr << "Data must refer to same ISBN" << std::endl;
        return -1;  // indicate failure
    }
}
```



1.6

```c++
#include <iostream>
#include <string>

struct Sale_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main()
{
    Sale_data total;
    double totalPrice;
    if (std::cin >> total.bookNo >> total.units_sold >> totalPrice)
    {
        total.revenue = total.units_sold * totalPrice;

        Sale_data trans;
        double transPrice;
        while (std::cin >> trans.bookNo >> trans.units_sold >> transPrice)
        {
            trans.revenue = trans.units_sold * transPrice;

            if (total.bookNo == trans.bookNo)
            {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            }
            else
            {
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
                if (total.units_sold != 0)
                    std::cout << total.revenue / total.units_sold << std::endl;
                else
                    std::cout << "(no sales)" << std::endl;

                total.bookNo = trans.bookNo;
                total.units_sold = trans.units_sold;
                total.revenue = trans.revenue;
            }
        }

        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
        if (total.units_sold != 0)
            std::cout << total.revenue / total.units_sold << std::endl;
        else
            std::cout << "(no sales)" << std::endl;

        return 0;
    }
    else
    {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```



### Exercise 2.42

*Q: Write your own version of the Sales_data.h header and use it to rewrite the exercise from § 2.6.2(p. 76)*

```c++
#ifndef SALES_dATA_H
#define SALES_dATA_H
#include <string>
#include <iostream>

struct Sales_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```

