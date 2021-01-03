# Chapter 3 Strings, Vectors, and Arrays

### Exercise 3.1

*Q: Rewrite the exercise from 1.4.1 and 2.6.2 with appropriate `using` declarations*

1.4.1

```c++
#include <iostream>

using std::cout;
using std::endl;

int main()
{
    int sum = 0;
    for (int val = 1; val <= 10; ++val) sum += val;
    cout << "Sum of 1 to 10 inclusive is " << sum << endl;

    return 0;
}
```



2.6.2

```c++
#include <iostream>
#include <string>

using std::cin;
using std::cout;
using std::endl;
using std::cerr;

...
```



### Exercise 3.2

*Q: Write a program to read the standard input a line at a time. Modify your program to read a word at a time.*

```c++
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;
using std::getline;

int main()
{
    // for (string str; getline(cin, str); cout << str << endl);
    for (string str; cin >> str; cout << str << endl);
    return 0;
}
```



### Exercise 3.3

*Q: Explain how whitespace characters are handled in the string input operator and in the `getline` function.*

- For code like `is >> s`, input is separated by whitespaces while reading into string `s`.
- For code like `getline(is, s)` input is separated by newline `\n` while reading into string `s`. Other whitespaces are ignored.
- For code like `getline(is, s, delim)`input is separated by `delim` while reading into string `s`. All whitespaces are ignored.



### Exercise 3.4

*Q: Write a program to read two strings and report whether the strings are equal. If not, report which of the two is larger. Now, change the program to report whether the strings have the same length, and if not, report which is longer.*

```c++
#include <iostream>
#include <string>
using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
    string str1, str2;
    while (cin >> str1 >> str2)
    {
        if (str1 == str2) // if (str1.size() == str2.size())
            cout << "The two strings are equal." << endl;
        else
            cout << "The larger string is " << ((str1 > str2) ? str1 : str2);
    }

    return 0;
}
```



### Exercise 3.5

*Q: Write a program to read strings from the standard input, concatenating what is read into one large string. Print the concatenated string. Next, change the program to separate adjacent input strings by a space.*

```c++
#include <iostream>
#include <string>

using std::string;
using std::cin;
using std::cout;
using std::endl;

int main()
{
    string concatenated;
    // for (string buffer; cin >> buffer; concatenated += buffer);
    for (string buff; cin >> buff; concatenated += " " + buff);
    cout << "The concatenated string is " << concatenated << endl;

    return 0;
}
```



### Exercise 3.6

*Q: Use a range for to change all the characters in a string to X.*

```c++
#include <iostream>
#include <string>

using std::string;
using std::cout;
using std::endl;

int main()
{
    string str("a simple string");
    for (auto &c : str) c = 'X';
    cout << str << endl;

    return 0;
}
```



### Exercise 3.7

*Q: What would happen if you define the loop control variable in the previous exercise as type char? Predict the results and then change your program to use a char to see if you were right.*

auto=char



### Exercise 3.8

*Q: Rewrite the program in the first exercise, first using a while*
*and again using a traditional for loop. Which of the three approaches do*
*you prefer and why?*



### Exercise 3.9

*Q: What does the following program do? Is it valid? If not, why not?*

```c++
string s;
cout << s[0] << endl; // invalid, s is empty string
```



### Exercise 3.10

*Q: Write a program that reads a string of characters including punctuation and writes what was read but with the punctuation removed*

```c++
#include <iostream>
#include <string>

using std::string;
using std::cout;
using std::cin;
using std::endl;

int main()
{
    cout << "Enter a string of characters including punctuation." << endl;
    for (string s; getline(cin, s); cout << endl)
        for (auto i : s) 
            if (!ispunct(i)) cout << i;

    return 0;
}
```



### Exercise 3.11

*Q: Is the following range for legal? If so, what is the type of c?*

```c++
const string s = "Keep out!";
for (auto &c : s){ /*... */ } // legal, c is const char&
```



### Exercise 3.12

*Q: Which, if any, of the following vector definitions are in error? For those that are legal, explain what the definition does. For those that are not legal, explain why they are illegal.*

```c++
vector<vector<int>> ivec;         // legal(c++11), vectors.
vector<string> svec = ivec;       // illegal, different type.
vector<string> svec(10, "null");  // legal, vector have 10 strings: "null".
```



### Exercise 3.13

*Q: How many elements are there in each of the following vectors? What are the values of the elements?*

```c++
vector<int> v1;         // size:0,  no values.
vector<int> v2(10);     // size:10, value:0
vector<int> v3(10, 42); // size:10, value:42
vector<int> v4{ 10 };     // size:1,  value:10
vector<int> v5{ 10, 42 }; // size:2,  value:10, 42
vector<string> v6{ 10 };  // size:10, value:""
vector<string> v7{ 10, "hi" };  // size:10, value:"hi"
```



### Exercise 3.14

*Q: Write a program to read a sequence of ints from cin and store those values in a vector.*

```c++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec;
    for (int i; std::cin >> i; vec.push_back(i));

    return 0;
}
```



### Exercise 3.15

*Q: Repeat the previous program but read strings this time.*

```c++
#include <iostream>
#include <vector>
#include <string>

int main()
{
    std::vector<std::string> vec;
    for (std::string buffer; std::cin >> buffer; vec.push_back(buffer));

    return 0;
}
```



### Exercise 3.16

*Q: Write a program to print the size and contents of the vectors from exercise 3.13. Check whether your answers to that exercise were correct. If not, restudy § 3.3.1 (p. 97) until you understand why you were wrong.*

```c++
#include <iostream>
#include <vector>
#include <string>

using std::vector;
using std::string;
using std::cout;
using std::endl;

int main()
{
    vector<int> v1;
    cout << v1.size() << endl;
    for (auto i : v1)
        cout << i << " " << endl;
    return 0;
}
```



### Exercise 3.17

*Q: Read a sequence of words from cin and store the values a vector. After you’ve read all the words, process the vector and change each word to uppercase. Print the transformed elements, eight words to a line.*

```c++
#include <iostream>
#include <vector>
#include <string>

using std::cin;
using std::cout;
using std::endl;
using std::vector;
using std::string;

int main()
{
    vector<string> vec;
    for (string word; cin >> word; vec.push_back(word));
    for (auto &str : vec) for (auto &c : str) c = toupper(c);

    for (string::size_type i = 0; i != vec.size(); ++i) {
        if (i != 0 && i % 8 == 0) cout << endl;
        cout << vec[i] << " ";
    }
    cout << endl;
    return 0;
}
```





### Exercise 3.18

*Q: Is the following program legal? If not, how might you fix it?*

```c++
vector<int> ivec;
ivec[0] = 42; // illegal, ivec.push_back(42)
```



### Exercise 3.19

*Q: List three ways to define a vector and give it ten elements, each with the value 42. Indicate whether there is a preferred way to do so and why.*

```c++
vector<int> v1(10,42);
vector<int> v2{ 42, 42, 42, 42, 42, 42, 42, 42, 42, 42 };
vector<int> v3;
for (int i = 0; i < 10; ++i) v3.push_back(42);
```



### Exercise 3.20

*Q: Read a set of integers into a vector. Print the sum of each pair of adjacent elements. Change your program so that it prints the sum of the first and last elements, followed by the sum of the second and second-tolast, and so on.*

```c++
#include <iostream>
#include <vector>

using std::vector; using std::cout; using std::endl; using std::cin;

int main()
{
    vector<int> ivec;
    for (int i; cin >> i; ivec.push_back(i));
	
    for (int i = 0; i < ivec.size() - 1; ++i)
        cout << ivec[i] + ivec[i + 1] << " ";
    cout << endl;
    
    int left = 0, right = ivec.size() - 1;
    while (left <= right) {
        cout << ivec[left] + ivec[right] << " ";
        left++;
        right--;
    }
    cout << endl;
    return 0;
}
```



### Exercise 3.21

*Q: Redo the first exercise from § 3.3.3 (p. 105) using iterators.*

```c++
...
    vector<int> v1(10);
    cout << v1.size() << endl;
    for (auto it = v1.begin(); it != v1.end(); ++it)
        cout << *it << " ";
...
```



### Exercise 3.22

*Q: Revise the loop that printed the first paragraph in text to instead change the elements in text that correspond to the first paragraph to all uppercase. After you’ve updated text, print its contents.*

```c++
...
for (auto it = text.begin(); it != text.end() && !it->empty(); ++it) {
    if (isalpha(*it)) *it = toupper(*it);
    cout << *it << " ";
}
...
```



### Exercise 3.23

*Q: Write a program to create a vector with ten int elements. Using an iterator, assign each element a value that is twice its current value. Test your program by printing the vector.*

```c++
for (auto it = v.begin(); it != v.end(); ++it) (*it) *= 2;
```



### Exercise 3.24

*Q: Redo the last exercise from § 3.3.3 (p. 105) using iterators.*

```c++
for (auto it = v.cbegin(); it != v.cend(); ++it)
    cout << *it + *(it+1) << " ";
cout << endl;

for (auto left = v.cbegin(), right = v.cend() - 1; left <= right; ++left, --right) {
    cout << *left + *right << " ";
}
cout << endl;
```



### Exercise 3.25

*Q: Rewrite the grade clustering program from § 3.3.3 (p. 104) using iterators instead of subscripts.*

```c++
while (cin >> grade) {
    if (grad <= 100) {
        ++*(scores.begin() + grade/10);
    }
}
```



### Exercise 3.26

*Q: In the binary search program on page 112, why did we write mid = beg + (end - beg) / 2; instead of mid = (beg + end)/2;?*

There's no operator + for adding two iterators.



### Exercise 3.27

Q: Assuming txt_size is a function that takes no arguments and returns an int value, which of the following definitions are illegal? Explain why.

```c++
unsigned buf_size = 1024;

int ia[buf_size];   // illegal, The dimension value must be a constant expression.
int ia[4 * 7 - 14]; // legal
int ia[txt_size()]; // ok if txt_size() is constexpr, error otherwise.
char st[11] = "fundamental";  // illegal, the string's size is 12.
```



### Exercise 3.28

*Q: What are the values in the following arrays?*

```c++
string sa[10];      //all elements are empty strings
int ia[10];         //all elements are 0

int main() 
{
    string sa2[10]; //all elements are empty strings
    int ia2[10];    //all elements are undefined
}
```



### Exercise 3.29

*Q: List some of the drawbacks of using an array instead of a vector.*

fixed size



### Exercise 3.30

*Q: Identify the indexing errors in the following code:*

```c++
constexpr size_t array_size = 10;
int ia[array_size];
for (size_t ix = 1; ix <= array_size; ++ix) // buffer overflow, ix < array_size
      ia[ix] = ix;
```



### Exercise 3.31

*Q: Write a program to define an array of ten ints. Give each element the same value as its position in the array.*

```c++
int arr[10];
for (int i = 0; i < 10; ++i) arr[i] = i;
```



### Exercise 3.32

*Q: Copy the array you defined in the previous exercise into another array. Rewrite your program to use vectors.*

```c++
int arr[10], arr2[10];
for (int i = 0; i < 10; ++i) arr2[i] = arr[i];

vector<int> v;
for (int i = 0; i < 10; ++i) v[i] = arr[i];
```



### Exercise 3.33

*Q: What would happen if we did not initialize the scores array in the program on page 116?*

values in the array are undefined.



### Exercise 3.34

*Q: Given that p1 and p2 point to elements in the same array, what does the following code do? Are there values of p1 or p2 that make this code illegal?*

```c++
p1 += p2 - p1; // p1  = p1 + (p2-p1); it moves p1 to p2. Any legal value p1, p2 make this code legal.
```



### Exercise 3.35

*Q: Using pointers, write a program to set the elements in an array to zero.*

```c++
for (auto ptr = arr; ptr < arr + size; ++ptr) *ptr = 0;
```



### Exercise 3.36

*Q: Write a program to compare two arrays for equality. Write a similar program to compare two vectors.*

```c++
for (int *i = begin(arr1), *j = begin(arr2); (i < end(arr1)) && (j < end(arr2)); ++i, ++j)
	if (*i != *j) return false;

if (v1 != v2) return false;
```



### Exercise 3.37

*Q: What does the following program do?*

```c++
const char ca[] = { 'h', 'e', 'l', 'l', 'o' }; // not null terminated
const char *cp = ca;
while (*cp) { // traverse ca array
    cout << *cp << endl; // the result is undefined
    ++cp;
}
```



### Exercise 3.38

*Q: In this section, we noted that it was not only illegal but meaningless to try to add two pointers. Why would adding two pointers be meaningless?*





### Exercise 3.39

*Q: Write a program to compare two strings. Now write a program to compare the values of two C-style character strings.*





### Exercise 3.40

*Q: Write a program to define two character arrays initialized from string literals. Now define a third character array to hold the concatenation of the two arrays. Use strcpy and strcat to copy the two arrays into the third.*





### Exercise 3.41

*Q: Write a program to initialize a vector from an array of ints.*





### Exercise 3.42

*Q: Write a program to copy a vector of ints into an array of ints.*





### Exercise 3.43

*Q: Write three different versions of a program to print the elements of ia. One version should use a range for to manage the iteration, the other two should use an ordinary for loop in one case using subscripts and in the other using pointers. In all three programs write all the types directly. That is, do not use a type alias, auto, or decltype to simplify the code.*





### Exercise 3.44

*Q: Rewrite the programs from the previous exercises using a type alias for the type of the loop control variables.*





### Exercise 3.45

*Q: Rewrite the programs again, this time using auto.*



