# Chapter 5 Statements

### Exercise 5.1

*Q: What is a null statement? When might you use a null statement?*

```c++
while (cin >> s && s != sought)
	; // null statement
```



### Exercise 5.2

*Q: What is a block? When might you might use a block?*

A block is a (possibly empty) sequence of statements and declarations surrounded by a pair of curly braces.

```c++
while (val <= 10) {
    sum += val;
    ++val;
}
```



### Exercise 5.3

*Q: Use the comma operator (§ 4.10, p. 157) to rewrite the while loop from § 1.4.1 (p. 11) so that it no longer requires a block. Explain whether this rewrite improves or diminishes the readability of this code.*

```c++
while (val <= 10)
        sum += val, ++val;
```

This rewritten version diminishes the readability.



### Exercise 5.4

*Q: Explain each of the following examples, and correct any problems you detect.*

```c++
(a) while (string::iterator iter != s.end()) { /* . . . */ }
// string::iterator iter = s.begin();
// while (iter != s.end()) {}
(b) while (bool status = find(word)) { /* . . . */ } if (!status) { /* . . . */ }
// bool status;
// while (status = find(word)) {}
// if (!status) {}
```



### Exercise 5.5

*Q: Using an if-else statement, write your own version of the program to generate the letter grade from a numeric grade.*

```c++
vector<string> scores = {"F", "D", "C", "B", "A", "A++"};
if (grade < 60) {
    letter = scores[0];
} else {
    letter = scores[(grade - 50) / 10];
    if (grade != 100) {
        if (grade % 10 >= 3) {
            if (grade % 10 > 7) {
                letter += "+";
            }
        } else {
            letter += "-";
        }
    } 
}
```



### Exercise 5.6

*Q: Rewrite your grading program to use the conditional operator in place of the if-else statement.*

```c++
vector<string> scores = {"F", "D", "C", "B", "A", "A++"};
lettergrade = (grade >= 60) ? scores[(grade - 50)/10] : scores[0];
letter += (grade == 100 || grade < 60) ? "" : (grade % 10 > 7) ? "+" : (grade % 10 < 3) ? "-" : "";
```



### Exercise 5.7

*Q: Correct the errors in each of the following code fragments:*

```c++
(a) if (ival1 != ival2) ival1 = ival2;  // add semicolon
    else ival1 = ival2 = 0;
(b) if (ival < minval) { // add curly braces for multiple statements
        minval = ival;
        occurs = 1;
    }
(c) if (int ival = get_value())
        cout << "ival = " << ival << endl;
    if (!ival) // else if (!ival)
        cout << "ival = 0\n";
(d) if (ival = 0) // if (ival == 0)
    ival = get_value();
```



### Exercise 5.8

*Q: What is a “dangling else”? How are else clauses resolved in C++?*

In C++ the ambiguity is resolved by specifying that each else is matched with the closest preceding unmatched if.



### Exercise 5.9

*Q: Write a program using a series of if statements to count the number of vowels in text read from cin.*

```c++
while (cin >> ch) {
    if (ch == 'a') ++aCnt;
    else if (ch == 'e') ++eCnt;
    else if (ch == 'i') ++iCnt;
    else if (ch == 'o') ++oCnt;
    else if (ch == 'u') ++uCnt;
}
```



### Exercise 5.10

*Q: There is one problem with our vowel-counting program as we’ve implemented it: It doesn’t count capital letters as vowels. Write a program that counts both lower- and uppercase letters as the appropriate vowel—that is, your program should count both 'a' and 'A' as part of aCnt, and so forth.*

```c++
switch (ch) {
    case 'a': case 'A':
        ++aCnt;
        break;
    case 'e': case 'E':
        ++eCnt;
        break;
    case 'i': case 'I':
        ++iCnt;
        break;
    case 'o': case 'O':
        ++oCnt;
        break;
    case 'u': case 'U':
        ++uCnt;
        break;
}
```



### Exercise 5.11

*Q: Modify our vowel-counting program so that it also counts the number of blank spaces, tabs, and newlines read.*

```c++
switch (ch) {
    case 'a': case 'A':
        ++aCnt;
        break;
    case 'e': case 'E':
        ++eCnt;
        break;
    case 'i': case 'I':
        ++iCnt;
        break;
    case 'o': case 'O':
        ++oCnt;
        break;
    case 'u': case 'U':
        ++uCnt;
        break;
    case ' ':
        ++bCnt;
        break;
    case '\t':
        ++tCnt;
        break;
    case '\n':
        ++nCnt;
        break;
}
```



### Exercise 5.12

*Q: Modify our vowel-counting program so that it counts the number of occurrences of the following two-character sequences: ff, fl, and fi.*

```c++
case 'f':
	if (prev = 'f') ++ffCnt;
	break;
case 'l':
	if (prev = 'f') ++flCnt;
	break;
case 'i':
	if (prev = 'f') ++fiCnt;
	break;
```



### Exercise 5.13

*Q: Each of the programs in the highlighted text on page 184 contains a common programming error. Identify and correct each error.*

```c++
(a) unsigned aCnt = 0, eCnt = 0, iouCnt = 0;
    char ch = next_text();
    switch (ch) {
        case 'a': aCnt++; // break;
        case 'e': eCnt++; // break;
        default: iouCnt++; // break;
    }
(b) unsigned index = some_value();
	//int ix
    switch (index) {
        case 1:
            int ix = get_value(); // ix = get_value();
            ivec[ ix ] = index;
            break;
        default:
            ix = ivec.size()-1;
            ivec[ ix ] = index;
    }
(c) unsigned evenCnt = 0, oddCnt = 0;
    int digit = get_num() % 10;
    switch (digit) {
        case 1, 3, 5, 7, 9: // case 1: case 3: case 5: case 7: case 9:
            oddcnt++;
            break;
        case 2, 4, 6, 8, 10: // case 2: case 4: case 6: case 8: case 0:
            evencnt++;
            break;
    }
(d) unsigned ival=512, jval=1024, kval=4096;
    unsigned bufsize;
    unsigned swt = get_bufCnt();
    switch(swt) {
        case ival: // case 512:
            bufsize = ival * sizeof(int);
            break;
        case jval: // case 1024:
            bufsize = jval * sizeof(int);
            break;
        case kval: // case 4096:
            bufsize = kval * sizeof(int);
            break;
    }
```



### Exercise 5.14

*Q: Write a program to read strings from standard input looking for duplicated words. The program should find places in the input where one word is followed immediately by itself. Keep track of the largest number of times a single repetition occurs and which word is repeated. Print the maximum number of duplicates, or else print a message saying that no word was repeated. For example, if the input is*

```c++
how now now now brown cow cow
```

```c++
string s;
string prev = "";
int cnt = 0, max_cnt = 0;
while (cin >> s) {
    if (prev == s) {
        cnt += 1;
    } else {
        cnt = 0;
    }
    if (max_cnt < cnt) {
        max_cnt = cnt;
    }
    prev = s;
}
if (max_cnt == 0) {
    cout<<"no word was repeated"<<endl;
} else {
    cout<<"maximum number of duplicates: "<<max_cnt<<endl;
}
```



### Exercise 5.15

*Q: Explain each of the following loops. Correct any problems you detect.*

```c++
(a) // int ix;
	for (int ix = 0; ix != sz; ++ix) { /* ... */ } // for (ix = 0; ix != sz; ++ix)
    if (ix != sz)
    // . . .
(b) int ix;
    for (ix != sz; ++ix) { /* ... */ } // for (; ix != sz; ++ix)
(c) for (int ix = 0; ix != sz; ++ix, ++sz) { /*...*/ } // endless loop if sz != 0
	// for (int ix = 0; ix != sz; ++ix)
```



### Exercise 5.16

*Q: The while loop is particularly good at executing while some condition holds; for example, when we need to read values until end-of-file. The for loop is generally thought of as a step loop: An index steps through a range of values in a collection. Write an idiomatic use of each loop and then rewrite each using the other loop construct. If you could use only one loop, which would you choose? Why?*

```c++
// while idiomatic
int i;
while ( cin >> i )
    // ...

// same as for
for (int i = 0; cin >> i;)
    // ...

// for idiomatic
for (int i = 0; i != size; ++i)
    // ...

// same as while
int i = 0;
while (i != size)
{
    // ...
    ++i;
}
```



### Exercise 5.17

*Q: Given two vectors of ints, write a program to determine whether one vector is a prefix of the other. For vectors of unequal length, compare the number of elements of the smaller vector. For example, given the vectors containing 0, 1, 1, and 2 and 0, 1, 1, 2, 3, 5, 8, respectively your program should return true.*

```c++
vector<int> nums1 = {0,1,2};
vector<int> nums2 = {0,1,2,3,4,5};
int n1 = nums1.size(), n2 = nums2.size();
for (int i=0; i<n1&&i<n2; ++i) {
    if (nums1[i] != nums2[i]) {
        return false;
    }
}
return true;
```



### Exercise 5.18

*Q: Explain each of the following loops. Correct any problems you detect.*

```c++
(a) do { // added bracket.
        int v1, v2;
        cout << "Please enter two numbers to sum:" ;
        if (cin >> v1 >> v2)
            cout << "Sum is: " << v1 + v2 << endl;
    } while (cin);qq
(b) int ival;
    do {
        // . . .
    } while (int ival = get_response()); // } while ( ival = get_response());
(c) // int ival = get_response();
    do {
        int ival = get_response(); //ival = get_response();
    } while (ival); // ival is not declared in this scope.
```



### Exercise 5.19

*Q: Write a program that uses a do while loop to repetitively request two strings from the user and report which string is less than the other.*

```c++
string rsp;
do {
    cout << "Input two strings: ";
    string str1, str2;
    cin >> str1 >> str2;
    cout << (str1 < str2 ? str1 : str2) 
        << " is less than the other. " << "\n\n"
        << "More? Enter yes or no: ";
    cin >> rsp;
} while (!rsp.empty() && rsp[0] != 'n');
```



### Exercise 5.20

*Q: Write a program to read a sequence of strings from the standard input until either the same word occurs twice in succession or all the words have been read. Use a while loop to read the text one word at a time. Use the break statement to terminate the loop if a word occurs twice in succession. Print the word if it occurs twice in succession, or else print a message saying that no word was repeated.*

```c++
string s, prev;
while (cin >> s) {
    if (s == prev) {
        cout << read << " occurs twice in succession." << endl;
        break;
    }
	else {
        prev = s;
    }
}
```



### Exercise 5.21

*Q: Revise the program from the exercise in § 5.5.1 (p. 191) so that it looks only for duplicated words that start with an uppercase letter.*

```c++
while (cin >> s)  {
    if (isupper(s[0]) && prev == s) {
        cout << s << ": occurs twice in succession." << endl;
        break;
    }
    prev = s;
}
```



### Exercise 5.22

*Q: The last example in this section that jumped back to begin could be better written using a loop. Rewrite the code to eliminate the goto.*

```c++
for (int sz = get_size(); sz<=0; sz = get_size())
```



### Exercise 5.23

*Q: Write a program that reads two integers from the standard input and prints the result of dividing the first number by the second.*

```c++
int i, j; 
cin >> i >> j;
cout << i / j << endl;
```



### Exercise 5.24

*Q: Revise your program to throw an exception if the second number is zero. Test your program with a zero input to see what happens on your system if you don’t catch an exception.*

```c++
int i, j; 
cin >> i >> j;
try {
    if (j == 0) {
        throw runtime_error("the second number is zero");
    }
    cout << i / j << endl;
} catch (runtime_error err){
    cout << err.what();
}
```



### Exercise 5.25

*Q: Revise your program from the previous exercise to use a try block to catch the exception. The catch clause should print a message to the user and ask them to supply a new number and repeat the code inside the try.*

```c++
int i, j; 
while (cin >> i >> j) {
    try {
        if (j == 0) {
            throw runtime_error("the second number is zero");
        }
        cout << i / j << endl;
    } catch (runtime_error err){
        cout << err.what()
             << "\nTry Again? Enter y or n " << endl;
        char c;
        cin >> c;
        if (!cin || c == 'n') {
            break;
        }
    }
}
```

