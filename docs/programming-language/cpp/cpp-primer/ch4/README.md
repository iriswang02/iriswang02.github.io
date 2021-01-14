# Chapter 4 Expressions

### Exercise 4.1

*Q: What is the value returned by 5 + 10 * 20/2?*

105



### Exercise 4.2

*Q: Using Table 4.12 (p. 166), parenthesize the following expressions to indicate the order in which the operands are grouped:*

```c++
* vec.begin() //=> *(vec.begin())
* vec.begin() + 1 //=> (*(vec.begin())) + 1
```



### Exercise 4.3

*Q: Order of evaluation for most of the binary operators is left undefined to give the compiler opportunities for optimization. This strategy presents a trade-off between efficient code generation and potential pitfalls in the use of the language by the programmer. Do you consider that an acceptable trade-off? Why or why not?*

No, standard rule is much more clear



### Exercise 4.4

*Q: Parenthesize the following expression to show how it is evaluated. Test your answer by compiling the expression (without parentheses) and printing its result.*

```c++
12 / 3 * 4 + 5 * 15 + 24 % 4 / 2; // ((12/3)*4) + (5*15) + ((24%4)/2)
```



### Exercise 4.5

*Q: Determine the result of the following expressions.*

```c++
-30 * 3 + 21 / 5  // -90+4 = -86
-30 + 3 * 21 / 5  // -30+63/5 = -30+12 = -18
30 / 3 * 21 % 5   // 10*21%5 = 210%5 = 0
-30 / 3 * 21 % 4  // -10*21%4 = -210%4 = -2
```



### Exercise 4.6

*Q: Write an expression to determine whether an int value is even or odd.*

```c++
if (i%2 == 1)
```



### Exercise 4.7

*Q: What does overflow mean? Show three expressions that will overflow.*

Overflow happens when a value is computed that is outside the range of values that the type can represent.



### Exercise 4.8

Q: Explain when *operands* are evaluated in the logical AND, logical OR, and equality operators.

- logical `AND` : the second operand is evaluated if and only if the left side is `true`.
- logical `OR` : the second operand is evaluated if and only if the left side is `false`
- equality operators `==` : undefined.



### Exercise 4.9

*Q: Explain the behavior of the condition in the following if:*

```c++
const char *cp = "Hello World";
if (cp && *cp)
```

`cp` is not a `nullptr`, then `*cp` is evaluated, it is a const char 'H'.

true&&true



### Exercise 4.10

*Q: Write the condition for a while loop that would read ints from the standard input and stop when the value read is equal to 42.*

```c++
int i;
while (cin >> i && i != 42)
```



### Exercise 4.11

*Q: Write an expression that tests four values, a, b, c, and d, and ensures that a is greater than b, which is greater than c, which is greater than d.*

```c++
a>b&&b>c&&c>d
```



### Exercise 4.12

*Q: Assuming i, j, and k are all ints, explain what i != j < k means.*

i != ( j < k )



### Exercise 4.13

*Q: What are the values of i and d after each assignment?*

```c++
int i;   double d;
d = i = 3.5; // i = 3, d = 3.0
i = d = 3.5; // d = 3.5, i = 3
```



### Exercise 4.14

*Q: Explain what happens in each of the if tests:*

```c++
if (42 = i)   // error: literals are rvalues
if (i = 42)   // true. if(42)
```



### Exercise 4.15

*Q: The following assignment is illegal. Why? How would you correct it?*

```c++
double dval; int ival; int *pi;
dval = ival = pi = 0;
// pi is a pointer to int.
// error: incompatible pointer to integer conversion assigning to 'int' from 'int *';
```



### Exercise 4.16

*Q: Although the following are legal, they probably do not behave as the programmer expects. Why? Rewrite the expressions as you think they should be.*

```c++
if (p = getPtr() != 0) // legal, != would be the value returned from getPtr() and 0. The true or false result of that test would be assigned to p.
if (i = 1024) // legal, if (1024), should be if (i == 1024)
```



### Exercise 4.17

*Q: Explain the difference between prefix and postfix increment.*

```c++
int i=0,j;
j = ++i; // j = 1, i = 1
j = i++; // j = 1, i = 2
```



### Exercise 4.18

*Q: What would happen if the while loop on page 148 that prints the elements from a vector used the prefix increment operator?*

the first element is skipped and it will dereference v.end().



### Exercise 4.19

*Q: Given that ptr points to an int, that vec is a vector, and that ival is an int, explain the behavior of each of these expressions. Which, if any, are likely to be incorrect? Why? How might each be corrected?*

```c++
ptr != 0 && *ptr++  // 1. ptr is a nullptr? 2. *ptr = 0? 3. ptr++
ival++ && ival // 1. ival = 0? 2. ival++ 3. ival = 0?
vec[ival++] <= vec[ival] // incorrect. undefined behavior, vec[ival] <= vec[ival+1]
```



### Exercise 4.20

*Q: Assuming that iter is a vector::iterator, indicate which, if any, of the following expressions are legal. Explain the behavior of the legal expressions and why those that aren’t legal are in error.*

```c++
*iter++;  // return *iter, then ++iter.
(*iter)++;  // illegal, *iter is a string, cannot increment value.
*iter.empty() // illegal, iter should use '->' to indicate whether empty.
iter->empty();  // indicate the iter's value whether empty.
++*iter;        // illegal, string have not increment.
iter++->empty();  // return iter->empty(), then ++iter.
```



### Exercise 4.21

*Q: Write a program to use a conditional operator to find the elements in a vector\<int> that have odd value and double the value of each such element.*

```c++
for (auto &i : nums) {
    cout << ((i%2 == 1) ? i * 2 : i) << endl;
}
```



### Exercise 4.22

*Q: Extend the program that assigned high pass, pass, and fail grades to also assign low pass for grades between 60 and 75 inclusive. Write two versions: One version that uses only conditional operators; the other should use one or more if statements. Which version do you think is easier to understand and why?*

```c++
string result = g > 90 ? "high pass" : g < 60 ? "fail" : g < 75 ? "low pass" : "pass";

if (g>90) {
    cout << "high pass";
} else if (g<60) {
    cout << "fail";
} else if (g<75) {
    cout << "low pass";
} else {
    cout << "pass";
}
```



### Exercise 4.23

*Q: The following expression fails to compile due to operator precedence. Using Table 4.12 (p. 166), explain why it fails. How would you fix it?*

```c++
string s = "word";
string pl = s + s[s.size() - 1] == 's' ? "" : "s"; // string pl = s + ((s[s.size() - 1] == 's') ? "" : "s");
```



### Exercise 4.24

*Q: Our program that distinguished between high pass, pass, and fail depended on the fact that the conditional operator is right associative. Describe how that operator would be evaluated if the operator were left associative.*

```c++
string finalgrade = (grade > 90) ? "high pass" : (grade < 60) ? "fail" : "pass";
// right associative: string finalgrade = (grade > 90) ? "high pass" : ((grade < 60) ? "fail" : "pass");
// left associative: string finalgrade = ((grade > 90) ? "high pass" : (grade < 60)) ? "fail" : "pass";
```



### Exercise 4.25

*Q: What is the value of ~'q' << 6 on a machine with 32-bit ints and 8 bit chars, that uses Latin-1 character set in which 'q' has the bit pattern 01110001?*

?



### Exercise 4.26

Q: In our grading example in this section, what would happen if we used unsigned int as the type for quiz1?

unsigned int only has 16 bits, we need at least 27. It could return undesired result.



### Exercise 4.27

*Q: What is the result of each of these expressions?*

```c++
unsigned long ul1 = 3, ul2 = 7;
ul1 & ul2 // 3
ul1 | ul2 // 7
ul1 && ul2 // true
ul1 || ul2 // true
```



### Exercise 4.28

*Q: Write a program to print the size of each of the built-in types.*



### Exercise 4.29

*Q: Predict the output of the following code and explain your reasoning. Now run the program. Is the output what you expected? If not, figure out why.*

```c++
int x[10];   int *p = x;
cout << sizeof(x)/sizeof(*x) << endl; // 10
cout << sizeof(p)/sizeof(*p) << endl; // 2
```



### Exercise 4.30

*Q: Using Table 4.12 (p. 166), parenthesize the following expressions to match the default evaluation:*

```c++
sizeof x + y      // (sizeof x)+y
sizeof p->mem[i]  // sizeof(p->mem[i])
sizeof a < b      // sizeof(a) < b
sizeof f()        //If `f()` returns `void`, this statement is undefined, otherwise it returns the size of return type.
```



### Exercise 4.31

*Q: The program in this section used the prefix increment and decrement operators. Explain why we used prefix and not postfix. What changes would have to be made to use the postfix versions? Rewrite the program using postfix operators.*

```c++
for(vector<int>::size_type ix = 0; ix != ivec.size(); ix++, cnt--)  
    ivec[ix] = cnt;
```

no change.



### Exercise 4.32

*Q: Explain the following loop.*

```c++
constexpr int size = 5;
int ia[size] = { 1, 2, 3, 4, 5 };
for (int *ptr = ia, ix = 0;
    ix != size && ptr != ia+size;
    ++ix, ++ptr) { /* ... */ }
```

ix represents index, *ptr represents the value.



### Exercise 4.33

*Q: Using Table 4.12 (p. 166) explain what the following expression does:*

```c++
someValue ? ++x, ++y : --x, --y;
```

Because of the most lowest precedence of the comma operator, the expression is same as:

```c++
(someValue ? ++x, ++y : --x), --y
```



### Exercise 4.34

*Q: Given the variable definitions in this section, explain what conversions take place in the following expressions: (a) if (fval) (b) dval = fval + ival; (c) dval + ival \* cval; Remember that you may need to consider the associativity of the operators.*

```c++
if (fval) // fval -> bool
dval = fval + ival; // ival -> fval -> double
dval + ival * cval; // cval -> int -> double
```



### Exercise 4.35

*Q: Given the following definitions,*

```c++
char cval; int ival; unsigned int ui; float fval; double dval;
cval = 'a' + 3; // 'a' -> int -> char.
fval = ui - ival * 1.0; // ival-> double, ui -> double, double->float.
dval = ui * fval; // ui -> float -> double.
cval = ival + fval + dval;  // ival -> float -> double -> char(by truncation).
```



### Exercise 4.36

*Q: Assuming i is an int and d is a double write the expression i \*= d so that it does integral, rather than floating-point, multiplication.*

```c++
i *= static_cast<int>(d);
```



### Exercise 4.37

*Q: Rewrite each of the following old-style casts to use a named cast:*

```c++
int i; double d; const string *ps; char *pc; void *pv;
pv = (void*)ps; // pv = const_cast<string*>(ps); or pv = static_cast<void*>(const_cast<string*>(ps));
i = int(*pc);   // i = static_cast<int>(*pc);
pv = &d;        // pv = static_cast<void*>(&d);
pc = (char*)pv; // pc = static_cast<char*>(pv);
```



### Exercise 4.38

*Q: Explain the following expression:*

```c++
double slope = static_cast<double>(j/i);
```

j/i is an int(by truncation), then converted to double and assigned to slope.