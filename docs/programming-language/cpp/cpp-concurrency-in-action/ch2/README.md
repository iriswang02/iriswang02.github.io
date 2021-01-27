# Chapter 2 Managing threads



Basics:

1. launch a thread
2. waiting for it to finish, or running it in the background



**launching a thread**

```c++
// pass a void-returning function
void do_something();
thread my_thread(do_something);

// pass an instance of a class with a function call operator
class background_task {
public:
    void operator() () const {
        std::cout << " 1 ";
    }
};
background_task f;
thread my_thread(f);
my_thread.join();

// This is a declareation of my_thread function which return a thread object
thread my_thread(f());
// Correction:
thread my_thread( (f()) );
thread my_thread{f()};
thread my_thread([] { std::cout << " 1 "; });
```



It should be decided whether to wait for thread to finish or leave it to run on its own before the `std::thread` object is destroyed, then your program is terminated.

```c++
struct func {
    int& i;
    func(int& i_) : i(i_) {}
    void operator()() const {
        for (int j = 0; j < 1000000; ++j)
        {
            do_something(i); // potential access to dangling reference (local variable some_local_state is already destroyed => undefined behavior)
        }
    }
};

void f() {
    int some_local_state = 0;
    func ff(some_local_state);
    thread my_thread(ff);
    my_thread.detach(); // dont wait for thread to finish
}
```

To handle this scenario:

- copy the data into the thread rather than sharing the data
- using `join()`



If an exception is thrown after the thread has been started but before the call to `join()`, the call to `join()` is liable to be skipped.

To avoid application being terminated when an exception is thrown:

```c++
void f() {
    int some_local_state = 0;
    func ff(some_local_state);
    thread t(ff);
    try {
        do_something_in_current_thread();
    }
    catch(...) {
        t.join(); // for exceptional case
        throw;
    }
    t.join(); // for non-exceptional case
}
```



**passing arguments to a thread function**

By default, the arguments are copied into internal storage, where they can be accessed by the newly created thread of execution, and then passed to the callable object or function as rvalues as if they were temporaries. (This is done even if the corresponding parameter in the function is expecting a reference.)

```c++
void f(int i,std::string const& s);
void oops(int some_param) {
  char buffer[1024];
  sprintf(buffer, "%i",some_param);
  thread t(f,3,buffer); // the oops function may exit before the buffer has been converted to a string on the new thread => undefined behavior
  // correction: std::thread t(f,3,std::string(buffer));
  t.detach();
}
```



If the calling function expects a non-const reference, this wont compile, because the arguments copied and passed to the function are rvalues. 

```c++
void update_data_for_widget(widget_id w,widget_data& data); // expecting a non-const reference
void oops_again(widget_id w) {
  widget_data data;
  thread t(update_data_for_widget,w,data);
  // correction: thread t(update_data_for_widget,w,std::ref(data));
  display_status();
  t.join();
  process_widget_data(data); // data is not updated
}
```



Passing a member function pointer as the function, a suitable object pointer as the first argument

```c++
class X {
public:
  void do_lengthy_work();
};
X my_x;
std::thread t(&X::do_lengthy_work,&my_x); // This invokes my_x.do_lengthy_work() on the new thread, because the address of my_x is supplied as the object pointer.
// the third argument to the thread constructor will be the first argument to the member function, and so forth.
// std::thread t(&X::do_lengthy_work,&my_x, third_argument);
```



**transferring ownership of a thread**

Many resource-owning types in the C++ Standard Library, such as `ifstream` and `unique_ptr`, are movable but not copyable, and `thread` is one of them.

```c++
void some_function();
void some_other_function();
std::thread t1(some_function); // some_function -> t1, some_other_function -> null
std::thread t2=std::move(t1); // some_function -> t2, some_other_function -> null
t1=std::thread(some_other_function); // some_function -> t2, some_other_function -> t1
std::thread t3;
t3=std::move(t2); // some_function -> t3, some_other_function -> t1
t1=std::move(t3); // t1 already had an associated thread, so terminate() is called
```



transferring ownership out of a function

```c++
std::thread f() {
  void some_function();
  return std::thread(some_function);
}

std::thread g() {
  void some_other_function(int);
  std::thread t(some_other_function,42);
  return t;
}
```

transferring ownership into a function

```c++
void f(std::thread t);
void g() {
  void some_function();
  f(std::thread(some_function));
  std::thread t(some_function);
  f(std::move(t));
}
```

Benefit of the move support: can build on the `thread_guard` class and have it take ownership of the thread. This avoids any unpleasant consequences should the `thread_guard` object outlive the thread it was referencing, and it also means that no one else can join or detach the thread once ownership has been transferred into the object.

```c++
class scoped_thread {
    std::thread t;
public:
    explicit scoped_thread(std::thread t_) : t(std::move(t_)) {
        if (!t.joinable()) { // check in the constructor
            throw std::logic_error("no thread");
        }
    }
    ~scoped_thread() { t.join(); }
    scoped_thread(const scoped_thread&) = delete;
    scoped_thread& operator=(const scoped_thread&)=delete;
};

struct func { ... };

void f() {
    int some_local_state = 0;
    scoped_thread g{std::thread(func(some_local_state))};
    do_something_in_current_thread();
}
```



This move support also allows for containers of `thread` objects, if those containers are move-aware.

```c++
void do_work(unsigned id);
void f() {
    std::vector<std::thread> threads;
    for (int i = 0; i < 10; ++i) {
        threads.emplace_back(do_work, i); // spawns threads
    }
    for (auto& entry : threads)
        entry.join();
}
```



**choosing the number of threads at runtime**

```c++
std::thread::hardware_concurrency()
```

This function returns an indication of the number of threads that can truly run concurrently for a given execution of a program.



A simple implementation of a parallel version of `std::accumulate`.

```c++
template<typename Iterator,typename T>
struct accumulate_block
{
  void operator()(Iterator first,Iterator last,T& result)
  {
    result=std::accumulate(first,last,result);
  }
};

template<typename Iterator,typename T>
T parallel_accumulate(Iterator first,Iterator last,T init)
{
  unsigned long const length=std::distance(first,last);

  if(!length)
    return init; // Input is empty, return init value

  unsigned long const min_per_thread=25; // minimum block size (minimum number of tasks in each thread)
  unsigned long const max_threads=
      (length+min_per_thread-1)/min_per_thread;

  unsigned long const hardware_threads=
      std::thread::hardware_concurrency();

  unsigned long const num_threads= 
      std::min(hardware_threads != 0 ? hardware_threads : 2, max_threads); // dont want to run more threads than the hardware can support, because the context switching between more threads will decrease the performance

  unsigned long const block_size=length/num_threads;

  std::vector<T> results(num_threads);
  std::vector<std::thread> threads(num_threads-1);

  Iterator block_start=first;
  for(unsigned long i=0; i < (num_threads-1); ++i) { // num_threads - 1(main thread)
    Iterator block_end=block_start;
    std::advance(block_end,block_size); // advance block_end iterator to the end of the current block
    threads[i]=std::thread(
        accumulate_block<Iterator,T>(),
        block_start,block_end,std::ref(results[i]));
    block_start=block_end; // the start of the next block is the end of this one
  }
  accumulate_block<Iterator,T>()( // the final block (for uneven division)
      block_start,last,results[num_threads-1]);
  for (auto& entry : threads)
        entry.join();

  return std::accumulate(results.begin(),results.end(),init); // add up all results
}
```



**identifying threads**

 Thread identifiers are of type`std::thread_id`.

- The identifier for a thread can be obtained from its associated `thread` object by calling the `get_id()` member function.
- The identifier for the current thread can also be obtained by calling `std::this_thread::get_id()`

Objects of type`std::thread_id` can be freely copied and compared.



`std::thread_id` are often used to check whether a thread needs to perform some operation.

```c++
std::thread::id master_thread;
void some_core_part_of_algorithm()
{
  if(std::this_thread::get_id()==master_thread)
  {
    do_master_thread_work();
  }
  do_common_work();
}
```









