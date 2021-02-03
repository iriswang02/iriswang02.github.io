# Chapter 7 Classes

### Exercise 7.1

*Q: Write a version of the transaction-processing program from § 1.6 (p. 24) using the Sales_data class you defined for the exercises in § 2.6.1 (p. 72).*

```c++
struct Sales_data
{
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};

int main() {
    Sales_data total;
    if (std::cin >> total.bookNo >> total.units_sold >> total.revenue) {
        Sales_data trans;
        while (std::cin >> trans.bookNo >> trans.units_sold >> total.revenue) {
            if (total.bookNo == trans.bookNo) {
                total.units_sold += trans.units_sold;
                total.revenue += trans.revenue;
            }
            else {
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
            }
        }
        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
        return 0;
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```



### Exercise 7.2

*Q: Add the combine and isbn members to the Sales_data class you wrote for the exercises in § 2.6.2 (p. 76).*

```c++
struct Sale_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
    std::string isbn() const {
        return bookNo;
    }
    Sales_data& combine(const Sales_data& rhs) {
        units_sold += rhs.units_sold;
        revenue += rhs.revenue;
        return *this;
    }
};
```





### Exercise 7.3

*Q: Revise your transaction-processing program from § 7.1.1 (p. 256) to use these members.*

```c++
int main() {
    Sales_data total;
    if (std::cin >> total.bookNo >> total.units_sold >> total.revenue) {
        Sales_data trans;
        while (std::cin >> trans.bookNo >> trans.units_sold >> total.revenue) {
            if (total.isbn() == trans.isbn()) {
                total.combine(trans);
            }
            else {
                std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
            }
        }
        std::cout << total.bookNo << " " << total.units_sold << " " << total.revenue << " ";
        return 0;
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```



### Exercise 7.4

*Q: Write a class named Person that represents the name and address of a person. Use a string to hold each of these elements. Subsequent exercises will incrementally add features to this class.*

```c++
class Person {
	std::string name;
    std::string address;
};
```



### Exercise 7.5

*Q: Provide operations in your Person class to return the name and address. Should these functions be const? Explain your choice.*

```c++
class Person {
	std::string name;
    std::string address;
public:
   	std::string get_name() const {
        return name;
    }
    std::string get_address() const {
        return address;
    }
};
```



### Exercise 7.6

*Q: Define your own versions of the add, read, and print functions.*

```c++
Sales_data add(const Sales_data& lhs, const Sales_data& rhs) {
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}

istream& read(istream& is, Sales_data& item) {
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}

ostream& print(ostream& os, const Sales_data& item) {
    os << item.isbn() << " " << item.units_sold << " " << item.revenue << " " << item.avg_price();
    return os;
}
```



### Exercise 7.7

*Q: Rewrite the transaction-processing program you wrote for the exercises in § 7.1.2 (p. 260) to use these new functions.*

```c++
int main()
{
    Sales_data total;
    if (read(cin, total))
    {
        Sales_data trans;
        while (read(cin, trans))
        {
            if (total.isbn() == trans.isbn()) {
                total.combine(trans);
            }
            else {
                print(cout, total);
            }
        }
        print(cout, total);
        return 0;
    }
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```



### Exercise 7.8

*Q: Why does read define its Sales_data parameter as a plain reference and print define its parameter as a reference to const?*

`read()` function needs to change the element value of Sales_data

`print()` function doesnt need to change the element value of Sales data.



### Exercise 7.9

*Q: Add operations to read and print Person objects to the code you wrote for the exercises in § 7.1.2 (p. 260).*

```c++
class Person {
	...
};
istream& read(istream& is, Person& p) {
    is >> p.name >> p.address;
    return is;
}

ostream& print(ostream& os, const Person& p) {
    os << p.name << " " << p.address;
    return os;
}
```



### Exercise 7.10

*Q: What does the condition in the following if statement do? if (read(read(cin, data1), data2))*

read two data at one time.



### Exercise 7.11

*Q: Add constructors to your Sales_data class and write a program to use each of the constructors.*

```c++
struct Sales_data {
    ...
	Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is);
};
Sales_data::Sales_data(std::istream &is) {
    read(is, *this);
}

Sales_data item;
Sales_data item("bookNo");
Sales_data item("bookNo", 10, 10.0);
Sales_data item(cin);
```



### Exercise 7.12

*Q: Move the definition of the Sales_data constructor that takes an istream into the body of the Sales_data class.*

```c++
Sales_data(std::istream &is) {
    read(is, *this);
}
```



### Exercise 7.13

*Q: Rewrite the program from page 255 to use the istream constructor.*

```c++
int main()
{
    Sales_data total(cin);
    istream& is = cin;
    while (is)
    {
        Sales_data trans(is);
        if (total.isbn() == trans.isbn()) {
            total.combine(trans);
        }
        else {
            print(cout, total);
        }
    }
    print(cout, total);
    return 0;
    else {
        std::cerr << "No data?!" << std::endl;
        return -1;  // indicate failure
    }
}
```



### Exercise 7.14

*Q: Write a version of the default constructor that explicitly initializes the members to the values we have provided as in-class initializers.*

```c++
Sales_data() : bookNo(""), units_sold(0), revenue(0){ }
```



### Exercise 7.15

*Q: Add appropriate constructors to your Person class.*

```c++
class Person {
    string name;
    string address;
    
    Person(): name(""), address("") {}
    Person(const std::string name_in, const std::string address_in):name(name_in), address(address_in){ }
    Person(std::istream &is){ read(is, *this); }
};
```



### Exercise 7.16

*Q: What, if any, are the constraints on where and how often an access specifier may appear inside a class definition? What kinds of members should be defined after a public specifier? What kinds should be private?*

No constraints on where and how often an access specifier may appear.

public: accessible to all parts of the program 

private: only accessible to the member functions of the class



### Exercise 7.17

*Q: What, if any, are the differences between using class or struct?*

The only difference between using class and using struct to define a class is the default access level. 

class : private

struct : public



### Exercise 7.18

*Q: What is encapsulation? Why is it useful?*

Encapsulation is to hide some implementation using access specifiers (private).



### Exercise 7.19

*Q: Indicate which members of your Person class you would declare as public and which you would declare as private. Explain your choice.*

public: constructors, `getName()`, `getAddress()`. (interface: constructors + member functions)

private: `name`, `address`. (data member)



### Exercise 7.20

*Q: When are friends useful? Discuss the pros and cons of using friends.*

When a another class or function wants to access a non-public members.

pros: convenient

cons: separate declarations



### Exercise 7.21

*Q: Update your Sales_data class to hide its implementation. The programs you’ve written to use Sales_data operations should still continue to work. Recompile those programs with your new class definition to verify that they still work.*

```c++
class Sales_data {
    friend std::istream &read(std::istream &is, Sales_data &item);
    friend std::ostream &print(std::ostream &os, const Sales_data &item);
    friend Sales_data add(const Sales_data &lhs, const Sales_data &rhs);

public:
    Sales_data() = default;
    Sales_data(const std::string &s):bookNo(s) { }
    Sales_data(const std::string &s, unsigned n, double p):bookNo(s), units_sold(n), revenue(n*p){ }
    Sales_data(std::istream &is) { read(is, *this); }

    std::string isbn() const { return bookNo; };
    Sales_data& combine(const Sales_data&);

private:
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
```



### Exercise 7.22

*Q: Update your Person class to hide its implementation.*

```c++
class Person {
    friend std::istream &read(std::istream &is, Person &person);
    friend std::ostream &print(std::ostream &os, const Person &person);

public:
    Person() = default;
    Person(const std::string sname, const std::string saddr):name(sname), address(saddr){ }
    Person(std::istream &is){ read(is, *this); }

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
private:
    std::string name;
    std::string address;
};
```



### Exercise 7.23

*Q: Write your own version of the Screen class.*

```c++
class Screen {
public:
    using pos = std::string::size_type; // typedef std::string::size_type pos;

    Screen() = default;
    Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }

    char get() const { return contents[cursor]; }
    inline char get(pos ht, pos wd) const;
    Screen &move(pos r, pos c);

private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
};
char Screen::get(pos r, pos c) const {
    return contents[r*width+c]; 
}
inline Screen &Screen::move(pos r, pos c) {
    pos row = r*width;
    cursor = row + c;
    return *this;
}
```



### Exercise 7.24

*Q: Give your Screen class three constructors: a default constructor; a constructor that takes values for height and width and initializes the contents to hold the given number of blanks; and a constructor that takes values for height, width, and a character to use as the contents of the screen.*

```c++
Screen() = default;
Screen(pos ht, pos wd): :height(ht), width(wd), contents(ht*wd, ' '){ }
Screen(pos ht, pos wd, char c):height(ht), width(wd), contents(ht*wd, c){ }
```



### Exercise 7.25

*Q: Can Screen safely rely on the default versions of copy and assignment? If so, why? If not, why not?*



### Exercise 7.26

*Q: Define Sales_data::avg_price as an inline function.*

```c++
inline double Sales_data::avg_price() const {
    return units_sold ? revenue/units_sold : 0;
}
```



### Exercise 7.27

*Q: Add the move, set, and display operations to your version of Screen. Test your class by executing the following code:*

```c++
Screen myScreen(5, 5, 'X');
myScreen.move(4,0).set('#').display(cout);
cout << "\n";
myScreen.display(cout);
cout << "\n";
```

```
XXXXXXXXXXXXXXXXXXXX#XXXX
XXXXXXXXXXXXXXXXXXXX#XXXX
```



### Exercise 7.28

*Q: What would happen in the previous exercise if the return type of move, set, and display was Screen rather than Screen&?*

every function returns a rvalue, myScreen is not modified, so the second row doesnt have '#'



### Exercise 7.29

*Q: Revise your Screen class so that move, set, and display functions return Screen and check your prediction from the previous exercise.*



### Exercise 7.30

*Q: It is legal but redundant to refer to members through the this pointer. Discuss the pros and cons of explicitly using the this pointer to access members.*

pros: explicit, avoid misunderstanding

cons: redundant



### Exercise 7.31

*Q: Define a pair of classes X and Y, in which X has a pointer to Y, and Y has an object of type X.*

```c++
class Y;
class X {
    Y* pointer;
};
class Y {
    X object;
};
```



### Exercise 7.32

*Q: Define your own versions of Screen and Window_mgr in which clear is a member of Window_mgr and a friend of Screen.*

```c++
class Screen;
class Window_mgr {
    void clear(ScreenIndex);
}
class Screen {
    friend void Window_mgr::clear(ScreenIndex);
}
inline void Window_mgr::clear(ScreenIndex i) {
    ...
}
```



### Exercise 7.33

*Q: What would happen if we gave Screen a size member defined as follows? Fix any problems you identify.*

```c++
pos Screen::size() const { // Screen::pos
	return height * width;
}
```



### Exercise 7.34

*Q: What would happen if we put the typedef of pos in the Screen class on page 285 as the last line in the class?*

identifier "pos" is undefined



### Exercise 7.35

*Q: Explain the following code, indicating which definition of Type or initVal is used for each use of those names. Say how you would fix any errors.*

```c++
typedef string Type;
Type initVal(); // string
class Exercise {
public:
    typedef double Type;
    Type setVal(Type); // double
    Type initVal(); // double
    // Type initVal() { // define
    // ...
    // }
private:
	int val;
};
Type Exercise::setVal(Type parm) { // first string, second double
// Exercise::Type Exercise::setVal(Type parm) {
    val = parm + initVal(); // member function
    return val;
}
```



### Exercise 7.36

*Q: The following initializer is in error. Identify and fix the problem.*

```c++
struct X {
    X (int i, int j): base(i), rem(base % j) { } // rem is initialized first, rem(i%j)
    int rem, base;
};
```



### Exercise 7.37

*Q: Using the version of Sales_data from this section, determine which constructor is used to initialize each of the following variables and list the values of the data members in each object:*

```c++
Sales_data first_item(cin); // istream&
int main() {
    Sales_data next; // string s="" (default)
    Sales_data last("9-999-99999-9"); // string 
}
```



### Exercise 7.38

*Q: We might want to supply cin as a default argument to the constructor that takes an istream&. Write the constructor declaration that uses cin as a default argument.*

```c++
Sales_data(std::istream& is = std::cin) {
    read(is, *this);
}
```



### Exercise 7.39

*Q: Would it be legal for both the constructor that takes a string and the one that takes an istream& to have default arguments? If not, why not?*

illegal, the complier doesnt know which one to use.



### Exercise 7.40

*Q: Choose one of the following abstractions (or an abstraction of your own choosing). Determine what data are needed in the class. Provide an appropriate set of constructors. Explain your decisions.*

```c++
(a) Book
(b) Date
(c) Employee
(d) Vehicle
(e) Object
(f) Tree
```



### Exercise 7.41

*Q: Rewrite your own version of the Sales_data class to use delegating constructors. Add a statement to the body of each of the constructors that prints a message whenever it is executed. Write declarations to construct a Sales_data object in every way possible. Study the output until you are certain you understand the order of execution among delegating constructors.*

```c++
Sales_data(): Sales_data("", 0, 0) {}
Sales_data(std::string& s): Sales_Data(s, 0, 0) {}
```



### Exercise 7.42

*Q: For the class you wrote for exercise 7.40 in § 7.5.1 (p. 291), decide whether any of the constructors might use delegation. If so, write the delegating constructor(s) for your class. If not, look at the list of abstractions and choose one that you think would use a delegating constructor. Write the class definition for that abstraction.*

```c++
class Employee {
    string name;
    int age;
    Employee(string name_in, int age_in): name(name_in), age(age_in) {}
    Employee(): Employee("dagongren", 35) {}
}
```



### Exercise 7.43

*Q: Assume we have a class named NoDefault that has a constructor that takes an int, but has no default constructor. Define a class C that has a member of type NoDefault. Define the default constructor for C.*

```c++
class C {
    NoDefault mem;
    C(): mem(0) {}
}
```



### Exercise 7.44

*Q: Is the following declaration legal? If not, why not?*

```c++
vector<NoDefault> vec(10);
```

illegal, NoDefault doesnt have default constructor.



### Exercise 7.45

*Q: What if we defined the vector in the previous execercise to hold objects of type C?*

legal, C has default constructor.



### Exercise 7.46

*Q: Which, if any, of the following statements are untrue? Why?*
*(a) A class must provide at least one constructor.* [untrue, synthesized default constructor]
*(b) A default constructor is a constructor with an empty parameter list.* [untrue, no initializer is supplied]
*(c) If there are no meaningful default values for a class, the class should not provide a default constructor.* [untrue]
*(d) If a class does not define a default constructor, the compiler generates one that initializes each data member to the default value of its associated type.* [untrue, only if the class does not explicitly define any constructors]



### Exercise 7.47

*Q: Explain whether the Sales_data constructor that takes a string should be explicit. What are the benefits of making the constructor explicit? What are the drawbacks?*

pros:

- avoid errors of implicit conversion

cons:

- it can be used only with the direct form of initialization



### Exercise 7.48

*Q: Assuming the Sales_data constructors are not explicit, what operations happen during the following definitions*

```c++
string null_isbn("9-999-99999-9");
Sales_data item1(null_isbn);
Sales_data item2("9-999-99999-9");
```

*What happens if the Sales_data constructors are explicit?*

both are legal.



### Exercise 7.49

*Q: For each of the three following declarations of combine, explain what happens if we call i.combine(s), where i is a Sales_data and s is a string:*

```c++
(a) Sales_data &combine(Sales_data); // s is implicitly converted to Sales_data
(b) Sales_data &combine(Sales_data&); // error, cannot pass a temporary to a non-const parameter??
(c) Sales_data &combine(const Sales_data&) const; // error, can pass a temporary to a const parameter, but the output is also a const parameter which is not expected.
```



### Exercise 7.50

*Q: Determine whether any of your Person class constructors should be explicit.*

```c++
class Person {
    friend std::istream &read(std::istream &is, Person &person);
    friend std::ostream &print(std::ostream &os, const Person &person);

public:
    Person() = default;
    Person(const std::string sname, const std::string saddr):name(sname), address(saddr){ }
    explicit Person(std::istream &is){ read(is, *this); }

    std::string getName() const { return name; }
    std::string getAddress() const { return address; }
private:
    std::string name;
    std::string address;
};
```



### Exercise 7.51

*Q: Why do you think vector defines its single-argument constructor as explicit, but string does not?*

to avoid ambiguity



### Exercise 7.52

*Q: Using our first version of Sales_data from § 2.6.1 (p. 72), explain the following initialization. Identify and fix any problems.*

```c++
Sales_data item = {"978-0590353403", 25, 15.99}; // Sales_data is not an aggregate class. In-class initializers should be removed.
```



### Exercise 7.53

*Q: Define your own version of Debug.*

```c++
class Debug {
public:
    constexpr Debug(bool b = true) : hw(b), io(b), other(b) { }
    constexpr Debug(bool h, bool i, bool o) : hw(h), io(i), other(o) { }
    constexpr bool any() { return hw || io || other; }
    
    void set_hw(bool b) { hw = b; }
    void set_io(bool b) { io = b; }
    void set_other(bool b) { hw = b; }
    
private:
    bool hw;        // hardware errors other than IO errors
    bool io;        // I/O error
    bool other;     // the others
};
```



### Exercise 7.54

*Q: Should the members of Debug that begin with set_ be declared as constexpr? If not, why not?*

no, a constexpr function can only have return statement.



### Exercise 7.55

*Q: Is the Data class from § 7.5.5 (p. 298) a literal class? If not, why not? If so, explain why it is literal.*

no, string is not literal type. (`std::is_literal_type`)



### Exercise 7.56

*Q: What is a static class member? What are the advantages of static members? How do they differ from ordinary members?*

A class member that is associated with the class, rather than with individual objects of the class type.

The static member can be associated with the class, not with each individual object.

a static data member can have incomplete type.



### Exercise 7.57

*Q: Write your own version of the Account class.*

```c++
class Account {
public:
    void calculate() { amount += amount * interestRate; }
    static double rate() { return interestRate; }
    static void rate(double rate_in) {interestRate = rate_in;}
    
private:
    std::string owner;
    double amount;
    static double interestRate;
    static double initRate();
};
double Account::interestRate = initRate();
```



### Exercise 7.58

*Q: Which, if any, of the following static data member declarations and definitions are errors? Explain why.*

```c++
// example.h
class Example {
public:
static double rate = 6.5;
static const int vecSize = 20;
static vector<double> vec(vecSize); // static vector<double> vec; we must define and initialize each static data member outside the class body.
};
// example.C
#include "example.h"
double Example::rate;
vector<double> Example::vec; // Example::vec(Example::vecSize)
```

