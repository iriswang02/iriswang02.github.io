# Chapter 6 Functions

### Exercise 6.1

*Q: What is the difference between a parameter and an argument?*

**Parameters**: Local variable declared inside the function parameter list.

**Arguments**: Initializers for a function's parameters.



### Exercise 6.2

*Q: Indicate which of the following functions are in error and why. Suggest how you might correct the problems.*

```c++
(a) int f() {  // string f()
          string s;
          // ...
          return s;
    }
(b) f2(int i) { /* ... */ }  // void f2(int i) {}
(c) int calc(int v1, int v1) { /* ... */ }  // parameter list cannot use same name twice
(d) double square (double x) { return x * x; }  // function body needs braces
```



### Exercise 6.3

*Q: Write and test your own version of fact.*

```c++
int fact(int i) {
    return (i > 1) ? i*fact(i-1) : 1;
}
```



### Exercise 6.4

*Q: Write a function that interacts with the user, asking for a number and generating the factorial of that number. Call this function from main.*

```c++
void fact2 () {
    cout << "Enter a positive integer" << endl;
    int i;
    while (cin >> i) {
        cout << fact(i) << endl;
    }
}
int main () {
    fact2();
    return 0;
}
```



### Exercise 6.5

*Q: Write a function to return the absolute value of its argument.*

```c++
int abs(int i) {
    return (i < 0) ? -i : i;
}
```



### Exercise 6.6

*Q: Explain the differences between a parameter, a local variable, and a local static variable. Give an example of a function in which each might be useful.*

- local variable: "local" to that function and hide declarations of the same name made in an outer scope.

- parameter: Local variables declared inside the function parameter list

- local static variable: local static object is initialized before the first time execution passes through the object’s definition. Local statics are not destroyed when a function ends; they are destroyed when the program terminates.



### Exercise 6.7

*Q: Write a function that returns 0 when it is first called and then generates numbers in sequence each time it is called again.*

```c++
size_t generate() {
    static size_t i = 0;
    return i++;
}
```



### Exercise 6.8

*Q: Write a header file named Chapter6.h that contains declarations for the functions you wrote for the exercises in § 6.1 (p. 205).*

```c++
int fact(int val);
```



### Exercise 6.9

*Q: Write your own versions of the fact.cc and factMain.cc files. These files should include your Chapter6.h from the exercises in the previous section. Use these files to understand how your compiler supports separate compilation.*



### Exercise 6.10

*Q: Using pointers, write a function to swap the values of two ints. Test the function by calling it and printing the swapped values.*

```c++
void swap(int* p1, int* p2) {
    int temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}
```



### Exercise 6.11

*Q: Write and test your own version of reset that takes a reference.*

```c++
void reset(int &ip) {
    ip = 0;
}
```



### Exercise 6.12

*Q: Rewrite the program from exercise 6.10 in § 6.2.1 (p. 210) to use references instead of pointers to swap the value of two ints. Which version do you think would be easier to use and why?*

```c++
void swap(int& val1, int& val2) {
    int temp = val1;
    val1 = val2;
    val2 = temp;
}
```

This version is easier to use.



### Exercise 6.13

*Q: Assuming T is the name of a type, explain the difference between a function declared as void f(T) and void f(T&).*

pass by value | pass by reference



### Exercise 6.14

*Q: Give an example of when a parameter should be a reference type. Give an example of when a parameter should not be a reference.*

```c++
//
void reset(int& i) {
    i = 0;
}
//
void print(const char* cp) {
    if (cp) {
        while (*cp) {
            cout << *cp++;
        }
    }
}
```



### Exercise 6.15

*Q: Explain the rationale for the type of each of find_char’s parameters In particular, why is s a reference to const but occurs is a plain reference? Why are these parameters references, but the char parameter c is not? What would happen if we made s a plain reference? What if we made occurs a reference to const?*

s should not be changed, but occurs must be changed.

c is only be used to compare, not change.

s might be changed, occurs cannot be changed.



### Exercise 6.16

*Q: The following function, although legal, is less useful than it might be. Identify and correct the limitation on this function:*

```c++
bool is_empty(string& s) { return s.empty(); } // const string& s
```



### Exercise 6.17

*Q: Write a function to determine whether a string contains any capital letters. Write a function to change a string to all lowercase. Do the parameters you used in these functions have the same type? If so, why? If not, why not?*

```c++
bool anyCapitalLetter(const string& s) {
    for (auto& c : s)
        if (isupper(c)) return true;
    return false;
}

void allToLower(string& s) {
    for (auto& c : s)
        if (isupper(c)) tolower(s[i]);
    }
}
```



### Exercise 6.18

*Q: Write declarations for each of the following functions. When you write these declarations, use the name of the function to indicate what the function does.*
*(a) A function named compare that returns a bool and has two parameters that are references to a class named matrix.*
*(b) A function named change_val that returns a vector\<int> iterator and takes two parameters: One is an int and the other is an iterator for a vector\<int>.*

```c++
bool compare(matrix& p1, matrix& p2);
vector<int>::iterator change_val(int i, vector<int>::iterator it)
```



### Exercise 6.19

*Q: Given the following declarations, determine which calls are legal and which are illegal. For those that are illegal, explain why.*

```c++
a) double calc(double);
calc(23.4, 55.1); // illegal, two arguments
b) int count (const string&, char);
count("abcda", 'a'); //legal
c) calc(66); //legal
d) int sum(vector<int>::iterator, vector<int>::iterator, int);
sum(vec.begin(), vec.end(), 3.8); //legal
```



### Exercise 6.20

*Q: When should reference parameters be references to const? What happens if we make a parameter a plain reference when it could be a reference to const?*

Use const when it's possible. It may be misleading, and also limits the type of arguments that can be used with the function.



### Exercise 6.21

*Q: Write a function that takes an int and a pointer to an int and returns the larger of the int value or the value to which the pointer points. What type should you use for the pointer?*

```c++
int func(const int& i, const int *const p) {
    return (i > *p) ? i : *p;
}
```



### Exercise 6.22

*Q: Write a function to swap two int pointers.*

```c++
void swap(int** a, int** b) {
    int* temp = *a;
    *a = *b;
    *b = temp;
}

int main()
{
    int i = 42, j = 99;
    int* p1 = &i;
    int* p2 = &j;
    std::cout << *p1 << " " << *p2 << std::endl;
    swap(&p1, &p2);
    std::cout << *p1 << " " << *p2 << std::endl;

    return 0;
}
```



### Exercise 6.23

*Q: Write your own versions of each of the print functions presented in this section. Call each of these functions to print i and j defined as follows:*

```c++
int i = 0, j[2] = {0, 1};
void print(const int& i, const int j[], size_t n) {
    std::cout << i << std::endl;
    while (n--) std::cout << *j++ << std::endl;
}
```



### Exercise 6.24

*Q: Explain the behavior of the following function. If there are problems in the code, explain what they are and how you might fix them.*

```c++
void print(const int ia[10]) { // const int (&ia)[10]
    for (size_t i=0; i!= 10; ++i) {
        cout << ia[i] <<endl;
    }
}
```



### Exercise 6.25

*Q: Write a main function that takes two arguments. Concatenate the supplied arguments and print the resulting string.*

```c++
int main(int argc, char** argv) {
    string s = "";
    for (int i=0; i<argc; i++) {
        s += argv[i];
    }
    cout << s << endl;
}
```



### Exercise 6.26

*Q: Write a program that accepts the options presented in this section. Print the values of the arguments passed to main.*

```c++
int main(int argc, char** argv) {
    string s = "";
    for (int i=0; i<argc; i++) {
        s += argv[i];
    }
    cout << s << endl;
}
```



### Exercise 6.27

*Q: Write a function that takes an initializer_list\<int> and produces the sum of the elements in the list.*

```c++
int sum(std::initializer_list<int>& il) {
    int sum = 0;
    for (auto& i : il) sum += i;
    return sum;
}
```



### Exercise 6.28

*Q: In the second version of error_msg that has an ErrCode parameter, what is the type of elem in the for loop?*

a reference to const string, `const string&`



### Exercise 6.29

*Q: When you use an initializer_list in a range for would you ever use a reference as the loop control variable? If so, why? If not, why not?*

I would use reference, it saves time to copy variable.



### Exercise 6.30

*Q: Compile the version of str_subrange as presented on page 223 to see what your compiler does with the indicated errors.*



### Exercise 6.31

*Q: When is it valid to return a reference? A reference to const?*

return a reference to a local object is invalid, because after a function terminates, its storage is freed.

if the return type is a reference to const, then we may not assign to the result of the call.



### Exercise 6.32

*Q: Indicate whether the following function is legal. If so, explain what it does; if not, correct any errors and then explain it.*

legal, the function get return a reference to the element in ia.



### Exercise 6.33

*Q: Write a recursive function to print the contents of a vector.*

```c++
void print(const vector<int>& v, const int& idx) {
    cout << v[idx] << endl;
    if (idx == v.size() - 1) {
        return;
    }
    return print(v, idx+1);
}
print(v, 0);
```



### Exercise 6.34

*Q: What would happen if the stopping condition in factorial were if (val != 0)*

If the argument is positive, the result is the same.

If the argument is negative, the recursion would never stop.



### Exercise 6.35

*Q: In the call to fact, why did we pass val - 1 rather than val--?*

val-- will be executed after return, the recursion would never stop.



### Exercise 6.36

*Q: Write the declaration for a function that returns a reference to an array of ten strings, without using either a trailing return, decltype, or a type alias.*

```c++
string (&func(string (&arrS)[10]))[10];
```



### Exercise 6.37

*Q: Write three additional declarations for the function in the previous exercise. One should use a type alias, one should use a trailing return, and the third should use decltype. Which form do you prefer and why?*

```c++
string (&func(string (&arrS)[10]))[10];
// type alias
using arrS = string[10];
arrS &func(arrS &arrS_val);
// trailing return
auto func(string (&arrS)[10]) -> string (&) [10];
// decltype
string s[10];
decltype(s) &func(string (&arrS)[10])
```



### Exercise 6.38

*Q: Revise the arrPtr function on to return a reference to the array.*

```c++
decltype(odd) &arrPtr(int i) {
    return (i%2) ? odd : even;
}
```



### Exercise 6.39

*Q: Explain the effect of the second declaration in each one of the following sets of declarations. Indicate which, if any, are illegal.*

```c++
a) int calc(int, int);
   int calc(const int, const int); // top-level const, redeclare
b) int get());
   double get(); // error: only the return type if different
c) int *reset(int *);
   double *reset(double *); // new function
```



### Exercise 6.40

*Q: Which, if either, of the following declarations are errors? Why?*

```c++
(a) int ff(int a, int b = 0, int c = 0); // ok
(b) char *init(int ht = 24, int wd, char bckgrnd); // wrong, defaults can be specified only if all parameters to the right already have defaults.
```



### Exercise 6.41

*Q: Which, if any, of the following calls are illegal? Why? Which, if any, are legal but unlikely to match the programmer’s intent? Why?*

```c++
char *init(int ht, int wd = 80, char bckgrnd = ' ');
(a) init(); // illegal, ht doesnt have default value.
(b) init(24,10); // legal, init(24,10,' ')
(c) init(14, '*'); // legal, init(14,'*',' ')
```



### Exercise 6.42

*Q: Give the second parameter of make_plural (§ 6.3.2, p.224) a default argument of 's'. Test your program by printing singular and plural versions of the words success and failure.*

```c++
string make_plural(size_t ctr, const string& word, const string& ending = "s") {
    return (ctr > 1) ? word + ending : word;
}
cout << "singual: " << make_plural(1, "person") << endl;
cout << "plural : " << make_plural(2, "person") << endl;
```



### Exercise 6.43

*Q: Which one of the following declarations and definitions would you put in a header? In a source file? Explain why.*

```c++
(a) inline bool eq(const BigInt&, const BigInt&) {...} // header, the compiler needs the definition in order to expand the code.
(b) void putValues(int *arr, int size); // header
```



### Exercise 6.44

Q: Rewrite the isShorter function from § 6.2.2 (p. 211) to be inline.

```c++
inline bool isShorter(const string& s1, const string& s2) {return s1.sze() < s2.size();}
```



### Exercise 6.45

*Q: Review the programs you’ve written for the earlier exercises and decide whether they should be defined as inline. If so, do so. If not, explain why they should not be inline.*



### Exercise 6.46

*Q: Would it be possible to define isShorter as a constexpr? If so, do so. If not, explain why not.*

No, the return expression is not a constant expression.



### Exercise 6.47

*Q: Revise the program you wrote in the exercises in § 6.3.2 (p.228) that used recursion to print the contents of a vector to conditionally print information about its execution. For example, you might print the size of the vector on each call. Compile and run the program with debugging turned on and again with it turned off.*

```c++
#ifndef NDEBUG
	cout << vec.size() << endl;
#endif
```



### Exercise 6.48

*Q: Explain what this loop does and whether it is a good use of assert:*

```c++
string s;
while (cin >> s && s != sought) { } // empty body
assert(cin); // cannot input after matched, it is not a good use of assert.
```



### Exercise 6.49

Q: What is a candidate function? What is a viable function?

- candidate function: functions in the set of overloaded functions considered for the call.

- viable function: functions selected from the set of candidate functions those functions that can be called with the arguments, 



### Exercise 6.50

*Q: Given the declarations for f from page 242, list the viable functions, if any for each of the following calls. Indicate which function is the best match, or if the call is illegal whether there is no match or why the call is ambiguous.*

```c++
(a) f(2.56, 42) // illegal
(b) f(42) // f(int)
(c) f(42, 0) // f(int,int)
(d) f(2.56, 3.14) // f(double, double=3.14)
```



### Exercise 6.51

*Q: Write all four versions of f. Each function should print a distinguishing message. Check your answers for the previous exercise. If your answers were incorrect, study this section until you understand why your answers were wrong.*



### Exercise 6.52

*Q: Given the following declarations,*

```c++
void manip(int, int);
double dobj;
```

*what is the rank (§ 6.6.1, p. 245) of each conversion in the following calls?*

```c++
(a) manip('a', 'z'); // 3 promotion
(b) manip(55.4, dobj); // 4 arithmetic conversion
```



### Exercise 6.53

*Q: Explain the effect of the second declaration in each one of the following sets of declarations. Indicate which, if any, are illegal.*

```c++
(a) int calc(int&, int&);
int calc(const int&, const int&); // new function
(b) int calc(char*, char*);
int calc(const char*, const char*); // new function
(c) int calc(char*, char*);
int calc(char* const, char* const); // redeclare
```



### Exercise 6.54

*Q: Write a declaration for a function that takes two int parameters and returns an int, and declare a vector whose elements have this function pointer type.*

```c++
int func(int, int);
typedef decltype(func) *f;
vector<f> v;
```



### Exercise 6.55

*Q: Write four functions that add, subtract, multiply, and divide two int values. Store pointers to these values in your vector from the previous exercise.*

```c++
int add(int a, int b) { return a+b; }
int subtract(int a, int b) { return a-b; }
int multiply(int a, int b) { return a*b; }
int divide (int a, int b) { return a/b; }

typedef decltype(add) *f1;
vector<f1> v1;
using f2 = int (*)(int, int);
vector<f2> v2;
using f3 = int(int, int);
vector<*f3> v3;
typedef int (*f4)(int, int);
vector<f4> v4;
```



### Exercise 6.56

*Q: Call each element in the vector and print their result.*

```c++
vector<f4> v4{ add, subtract, multiply, divide };
for (auto f : v4)
    cout << f(1,1) << endl;
```

