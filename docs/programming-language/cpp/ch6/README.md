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





### Exercise 6.7

*Q: Write a function that returns 0 when it is first called and then generates numbers in sequence each time it is called again.*



### Exercise 6.8

*Q: Write a header file named Chapter6.h that contains declarations for the functions you wrote for the exercises in § 6.1 (p. 205).*



### Exercise 6.9

*Q: Write your own versions of the fact.cc and factMain.cc files. These files should include your Chapter6.h from the exercises in the previous section. Use these files to understand how your compiler supports separate compilation.*



### Exercise 6.10

*Q: Using pointers, write a function to swap the values of two ints. Test the function by calling it and printing the swapped values.*



### Exercise 6.11

*Q: Write and test your own version of reset that takes a reference.*



### Exercise 6.12

*Q: Rewrite the program from exercise 6.10 in § 6.2.1 (p. 210) to use references instead of pointers to swap the value of two ints. Which version do you think would be easier to use and why?*



### Exercise 6.13

*Q: Assuming T is the name of a type, explain the difference between a function declared as void f(T) and void f(T&).*



### Exercise 6.14

*Q: Give an example of when a parameter should be a reference type. Give an example of when a parameter should not be a reference.*



### Exercise 6.15

*Q: Explain the rationale for the type of each of find_char’s parameters In particular, why is s a reference to const but occurs is a plain reference? Why are these parameters references, but the char parameter c is not? What would happen if we made s a plain reference? What if we made occurs a reference to const?*





### Exercise 6.16

*Q: The following function, although legal, is less useful than it might be. Identify and correct the limitation on this function:*





### Exercise 6.17

*Q: Write a function to determine whether a string contains any capital letters. Write a function to change a string to all lowercase. Do the parameters you used in these functions have the same type? If so, why? If not, why not?*



### Exercise 6.18

*Q: Write declarations for each of the following functions. When you write these declarations, use the name of the function to indicate what the function does.*
*(a) A function named compare that returns a bool and has two parameters that are references to a class named matrix.*
*(b) A function named change_val that returns a vector\<int> iterator and takes two parameters: One is an int and the other is an iterator for a vector\<int>.*



### Exercise 6.19

*Q: Given the following declarations, determine which calls are legal and which are illegal. For those that are illegal, explain why.*



### Exercise 6.20

*Q: When should reference parameters be references to const? What happens if we make a parameter a plain reference when it could be a reference to const?*



### Exercise 6.21

*Q: Write a function that takes an int and a pointer to an int and returns the larger of the int value or the value to which the pointer points. What type should you use for the pointer?*



### Exercise 6.22

*Q: Write a function to swap two int pointers.*



### Exercise 6.23

*Q: Write your own versions of each of the print functions presented in this section. Call each of these functions to print i and j defined as follows:*



### Exercise 6.24

*Q: Explain the behavior of the following function. If there are problems in the code, explain what they are and how you might fix them.*



### Exercise 6.25

*Q: Write a main function that takes two arguments. Concatenate the supplied arguments and print the resulting string.*





### Exercise 6.26

*Q: Write a program that accepts the options presented in this section. Print the values of the arguments passed to main.*



### Exercise 6.27

*Q: Write a function that takes an initializer_list\<int> and produces the sum of the elements in the list.*



### Exercise 6.28

*Q: In the second version of error_msg that has an ErrCode parameter, what is the type of elem in the for loop?*



### Exercise 6.29

*Q: When you use an initializer_list in a range for would you ever use a reference as the loop control variable? If so, why? If not, why not?*



### Exercise 6.30

*Q: Compile the version of str_subrange as presented on page 223 to see what your compiler does with the indicated errors.*



### Exercise 6.31

*Q: When is it valid to return a reference? A reference to const?*



### Exercise 6.32

*Q: Indicate whether the following function is legal. If so, explain what it does; if not, correct any errors and then explain it.*



### Exercise 6.33

*Q: Write a recursive function to print the contents of a vector.*



### Exercise 6.34

*Q: What would happen if the stopping condition in factorial were if (val != 0)*



### Exercise 6.35

*Q: In the call to fact, why did we pass val - 1 rather than val--?*



### Exercise 6.36

*Q: Write the declaration for a function that returns a reference to an array of ten strings, without using either a trailing return, decltype, or a type alias.*



### Exercise 6.37

*Q: Write three additional declarations for the function in the previous exercise. One should use a type alias, one should use a trailing return, and the third should use decltype. Which form do you prefer and why?*



### Exercise 6.38

*Q: Revise the arrPtr function on to return a reference to the array.*



### Exercise 6.39

*Q: Explain the effect of the second declaration in each one of the following sets of declarations. Indicate which, if any, are illegal.*

