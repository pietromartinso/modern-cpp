# **Starting my recycling studies on C++** #

- [x] STL / Containers 
- [x] Smart Pointers
- [x] Lambda Functions (functors)
- [ ] Multithreading
- [ ] Memory management, memory leaks, smart pointers: https://hackingcpp.com/cpp/lang/manual_memory_management.html 
- [ ] Segmentation faults 
- [ ] Debugging
- [ ] Static code analysis: https://learn.microsoft.com/en-us/cpp/code-quality/code-analysis-for-c-cpp-overview?view=msvc-170
- [ ] Unit testing: https://learn.microsoft.com/en-us/visualstudio/test/writing-unit-tests-for-c-cpp?view=vs-2022
- [ ] c++11: https://en.cppreference.com/w/cpp/11
- [ ] c++14: https://en.cppreference.com/w/cpp/14
- [ ] c++17: https://en.cppreference.com/w/cpp/17
- [ ] c++20: https://en.cppreference.com/w/cpp/20
- [ ] c++23: https://en.cppreference.com/w/cpp/23
-------------

## **Containers (STL)** ##

https://hackingcpp.com/cpp/std/sequence_containers.html

Containers are data-structure objects that allow us to store elements. They are built as templates, so we can insert as any data type needed, including other objects. Containers manages the memory used in the storage process as well as implements vairous member functions that allow us to interact with the container (add, remove, sort, etc). 

### **Sequence containers** ###

All sequence containers are deeply copyable, that means copying creates a new container object and copies all values; all sequence containers are deeply assignable, that menas all contained objects are copied from source to assignment target; all sequence containers are deeply comparable, which means that comparision between two containers implies comparing contained objects; all sequence containers are deeply owned, which means that destroying the container also destroys all contained objects.

As of c++17, when we use the initializer list, '{}', we doesn't need to specify what is the template type of the container, since it can be infered by the constructor of the container.

- ***Common Interface***
  - ``container.begin()`` & ``container.end()`` iterators (as of c++11, there are free standing ``static`` functions ``std::begin(container)`` & ``std::end(container)``)
    - there are also constant iterators ``cbegin()`` and ``cend()`` - same standing static functions since c++11
    - there are also "reverse" iterators, in the same manner, but this is not true for all sequence containers (not common interface)
  - ``container.empty()`` & c++11 ``std::empty(container)`` - to check if container has elements or not.

- **``array<T, size>``** (introduced in c++11)
  - Static contiguous memory (fixed size);
  - Random access without overhead;
  - Fast linear traversal;
  - Can't resize (thus, ``size()`` function must be a constant expression).
  - Reference: https://en.cppreference.com/w/cpp/container/array

    *Member functions*:
      - ``at()`` can access elements of the array;
      - ``operator[]`` access elements like C language;
      - ``front()`` returns the reference to the fist elemento of the array;
      - ``back()`` returns the reference to the fist elemento of the array;
      - ``size()`` retrieves the size of the array (``max_size()`` does the same, since we cant resize arrays);
      - ``swap()`` swaps elemenents positions inside the array
      - ``empty()`` returns true if the array size is zero
      - ``fill()`` populates the array with a specific vlue

- **``vector<T>``**:
  - Dynamic contiguous memory (can be resized);
  - C++ default container;
  - Random access withour overhead;
  - Fast linear traversal;
  - Fast insertion/deletion at the end (back);
    - Inserting data at the end may have diferential time, since there will be situations in which a resize will be needed - mostly constant time;
  - Insertion/deletion in the middle or in the middle takes linear time;
  - Reference: https://en.cppreference.com/w/cpp/container/vector

- **``deque<T>``**:
  - Dynamic not (necessarily) contiguous in memory;
  - Holds references for both extremities;
  - Fast insertion/deletion at both extremities;
  - Accessing elements can be made at constant time;
  - Insertion/deletion inside the structure has linear time;
  - Almost same operations as vectors, plus add/remove elements at the front;
  - Reference: https://en.cppreference.com/w/cpp/container/deque

- **``list<T>``**:
  - Doubly-linked list (circular) not contiguous in memory;
  - Slow traversal (when compared to vectors);
    - But, when position is found, insertion/deletion are fast - constant time;
  - Fast splicing 
  - Reference: https://en.cppreference.com/w/cpp/container/list

- **``forward_list<T>``** (introduced in c++11):
  - Singly-linked list not contiguous in memory;
  - Lower memory overhead because it doesn't hold "reversal" references;
  - Reference: https://en.cppreference.com/w/cpp/container/forward_list

- ***Which sequence container should one use?***

  - if number os elements to be stored is small and known at compilation time: use array;
  - else, try vector;
    - if vector is too slow and/or memory usage is too high, check where are the possible bottlenecks;
      - if insertion/deletion at begin/end dominate: try deque
        - still bad solution? try list;
      - if copying elements and/or insert/erase at random positions dominate: try list;
        - memory usage is slightly high and reverse traversal not needed: try forward_list;
      - if too many memory allocations and/or consumption being high: try ``vector.reserve(aproximated_expected_size)``;
      - if everything else is too slow, non-linear memory access dominate and can't be avoided: try another custom approach or non-standard solution with better characteristics.

### **Associative Containers** ###

Associative containers can be more efficient in operations like insertion, deletion and search. They usually are organized as trees (ordered) or hash tables (unordered). They have also two distinct "types" of containers: the ones that allows only insertion of unique keys, and the ones that allows insertion of duplicate keys.

- **``set<KeyType>``**:
  - implemented as a red-black tree (ordered ascending or descending)
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at logarithmic complexity
  - composed of single values/objects (not pairs)

- **``unordered_set<KeyType>``** (introduced in c++11):
  - implemented as unordered hashtable
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of single values/objects (not pairs)

- **``multiset<KeyType>``**:
  - implemented as a red-black tree (ordered ascending or descending)
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at logarithmic complexity
  - composed of single values/objects (not pairs)

- **``unordered_multiset<KeyType>``** (introduced in c++11):
  - implemented as unordered hashtable
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of single values/objects (not pairs)

- **``map<KeyType, ValueType>``**:
  - implemented as a red-black tree (ordered ascending or descending)
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at logarithmic complexity
  - composed of pairs of values/objects (key-value pairs)

- **``unordered_map<KeyType, ValueType>``** (introduced in c++11):
  - implemented as unordered hashtable
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of pairs of values/objects (key-value pairs)

- **``multimap<KeyType, ValueType>``**:
  - implemented as a red-black tree (ordered ascending or descending)
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at logarithmic complexity
  - composed of pairs of values/objects (key-value pairs)

- **``unordered_multimap<KeyType, ValueType>``** (introduced in c++11):
  - implemented as unordered hashtable
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of pairs of values/objects (key-value pairs)

-------------

## **Smart Pointers** ##

Raw pointers (conventional ones) are simple variables that store the address of another variable. This means they are just a "memory position" with not much other features. Raw pointers can be used to allocate memory dynamically but, in C++, dynamic memory needs to be freed "manually" by the programmer. The problem, here, is: 
- In which point of the code should we deallocate that dynamically allocated variable? Should we do it always inside the same scope where it was allocated?
- What would happen if we allocate an object via pointer, and between the allocation and the deletion process, an exception occours? How could we avoid memory leak in that case? 
- Is there any situation in which the address that this pointer is pointing to could still be in use by another scope or pointer?

Well... The fact is that raw pointers are very dangerous and can lead to lots of problems like memory leaks (reference to a dynamically allocated portion of memory is lost - so it can't be deallocated) or even segmentation faults (situations in which pointers store address that not valid anymore - object already deleted or memory region belonging to other process). Given this reality, in order to mitigate or even completelly resolve that problems, c++11 introduced and c++17 mainteined the concept of smart pointers.

Basically, smart pointers can manage memory (de)allocation by themselves. That means, when a smart pointer is created, it is capable to cover situations that it should allocate and deallocate memory by itself. Smart pointers are also usefull for controlling bounds checking. At c++17 specification, there are 3 definitions of smart pointers (``<memory>`` header), as we can see:

- **``unique_ptr<T>``**: these are the ones that restrict controls that there is one, and only one pointer pointing to a memory position once a time. Whenever the scope of the object being held by the pointer scope is ended, then the object is destroyed, that is, memory is deallocated. There is an exception case, where the object is not destroyed whenever its scope finishes: this is when the pointer holding the dynamic object is returned from a function - here, for the sake of good semanthics, the invoker of the function can still use the object's address even if the object was created inside the scope of the invoked function. There is almost no overhead when compared to raw pointers (eventyally little time overhead in constructor and little time+memory overhead at destructor).
  - Good practice: we should preffer to allocate a unique_ptr inside a function and return it to the "client code" so if it forgets to deallocate memory, unique_ptr will do this for him.

- **``shared_ptr<T>``**: these are the ones that allows more than one pointer pointing to a memory position at time. This kind of smart pointer keeps a counter, recording how much pointers are pointing to the current object/memory position. Whenever the counter reaches zero, the object is destroyed and memory is freed automatically. Shared pointers have some more overheads since it has the counter state to maintain, and also, since shared pointers can be held by different threads, they have to do counter (creation, increment, decrement) operations atomically.
  1) Good practice: one should not create a raw pointer and allocate memory of new operator, and then instantiate shared pointers that use the original pointer as means of initialization. This should lead to an inconsistent count of "how much shared pointers are pointing to that memory position". This will lead to unknown behavior (maybe segmentation fault will happen - maybe).
  2) Good practice: one should avoid constructing shared_pointers using the traditional ``this`` operator. Instead, the class that needs to create shared_pointers that points using ``this`` should inherit from ``std::enable_shared_from_this<InheritingClass>`` and then use the ``shared_from_this()`` in order to create a safe reference to "``this``".
  3) Good practice: one should be aware of possible circular references. In this cases, the counter will never reach zero and, therefore, the referenced object will never be destroyed (memory leak). In this cases, we shoulde never use ``shared_ptr``, but ``weak_ptr``.
  4) Good practice: one won't be allowed to create a ``shared_ptr`` to a raw array (``T[]``). Instead, we should use a default container, like ``std::array``.

- **``weak_ptr<T>``**: like the shared pointer, the weak pointer allows one object (memory position) to be referenced by more than one pointer. But, differently, it doesn't hold a counter for that references. In other words, whenever a ``weak_ptr`` points to an object, it doesn't interfere in the counting process. Given that, it's existence depends on the existence of a ``shared_ptr``. That means that ``weak_ptr`` doesn't make sense by itself, and should always be used in conjunction with a ``shared_ptr``. Basically, it should be used when it is necessary to point to an object, but when we don't want/need to control de life cycle of that object.
  - There are no operators ``*`` and ``->`` for this kind of pointer, so, whenever we need to access the owned object, we must use the ``lock()`` function in order to retreive a ``shared_ptr`` from that ``weak_ptr`` and then use the resulting ``shared_ptr`` to do our operations. If the object has already been destroyed, ``lock()`` will return a default valued ``shared_ptr`` (probably ``nullptr`` - dangling pointer). The ``lock()`` function must be executed in atomic way to deal with race conditions (since another thread could be deleting the object in the same moment ``lock()`` was invoked), thus overheading the operation.
    1) Good practice: whenever using the ``lock()`` operation, we should check if it returns a dangling pointer and, if so, we should treat that case.

### Usefull functions

- ``make_unique()``: safelly creates a unique pointer (forces the evaluation and execution of allocation process, in order to avoid the compiler to try to create an object "before" others, and then possibly causing memory leak whenever and exception occours in the process)
- ``make_shared()``: safelly creates a shared pointer (forces the evaluation and execution of allocation process, in order to avoid the compiler to try to create an object "before" others, and then possibly causing memory leak whenever and exception occours in the process)
- ``get()``: returns the raw pointer wrapped inside the smart pointer;
- ``weak_ptr.lock()``: returns a usefull shared_ptr for an owned object that needs to be accessed and probably have some operations made with it;
...

### *How to decide which smart pointer to use?* ###

- Whenever is necessary to terminate the object when it scopes locally ends (a block of code starts, and then ends, and so does the object), we should try ``unique_ptr``
- If and object's lifecyle is kind of complex, and it's lifecycle depends on complex class relations, we will probably need to use ``shared_ptr``.
- When we have a simple bidirectional association, we may want to use ``weak_ptr``.
- When we have ciclical referencing, we should use ``weak_ptr``.
- When we need to create a cache, we could use ``shared_ptr`` (i.e. to avoid going to a database / file everytime we need to retreive persistent data to memory) - in conjunction with ``weak_ptr`` to prevent cache in maintaning unused references in memory (``static shared_ptr`` cache does would do so - therefore, use ``weak_ptr``). It is a Flyweight like design pattern.
- Design patterns like Flyweight and Observer could use ``weak_ptr`` to implement it.

-------------

### **Functors: Function Objects & Lambda Functions** ###

Function Classes are those who provide at least one overloaded member function of the operator() - the call operator or application operator. Objects instantiated from that Class can be, then, used as functions, since they have the overloaded operator(). They have two main advantages conventional functions. First, since they are a type (class), they can be passed as argument in templates parameters. Second, these functors can also be stateful, that means, that they can have member variables and these variables can be updated at runtime. Since some standard algorithims doesn't garantee the evaluation order of the arguments passed as parameters to function objects, it is possible that state inconsistencies may happen. This is especially true for some c++17 parallel versions of those algorithms. Basically, member variables should not be used both for computing the return result as well as changing this variable state whenever operator() is called.
  
1) Good practice: calls to operator() in sequence need to be independent, one from another;
2) Good practice: onde may want to garantee that operator() is const, so it can't update function object's state
    - if one needs operator() to be non-const and use it within some of the standard algorithim (that may be parallel), it might be intelligent to garantee that concurrency won't affect the results (i.e. access to shared streams needs to be made carefully). 

They are often used as means of "costumizing behavior" in other function objects or algorithims (``<algorithim>`` header) data structures (standard containers). Some of the standard function objects, since c++11 defined in the ``<functional>`` header, are:

- **Comparisions**
  - ``std::equal_to<T>``
  - ``std::no_equal_to<T>``
  - ``std::greater<T>``
  - ``std::less<T>``
  - ``std::greater_equal<T>``
  - ``std::less_equal<T>``
- **Arithmetic**
  - ``std::plus<T>``
  - ``std::minus<T>``
  - ``std::multiplies<T>``
  - ``std::divides<T>``
  - ``std::modulus<T>``
  - ``std::negate<T>``
- **Logical and bitwise operations**
  - ``std::logical_and<T>``
  - ``std::logical_or<T>``
  - ``std::logical_not<T>``
  - ``std::bit_and<T>``
  - ``std::bit_or<T>``
  - ``std::bit_xor<T>``
  - ``std::bit_not<T>``
- **Hash**
  - ``std::hash<Arithmetic>``
  - ``std::hash<Enumeration>``
  - ``std::hash<T*>``
- Obs.: since c++14, it is not necessary to define the type T, since it can infer types automatically.

#### **Lambda Functions** ####

Lambda Functions are a special kind of function objects created at compilation time. It allows us to define custom anonymous functions, also known as closures, whose memory address is known only to compiler. They're syntax are a little bit different from what we are used to. Instead of the name of the function, we place the square brackets. Those square brackets allows us to specigy in which way we might "capture" variables that are in the same sorrounding scope of the function (it is called capture clause, also lambda-introducer). We might capture them by value ``[=]`` or by reference ``[&]``, or a combination of both (these captured variables act, lets say, as kind of member variables to our function objects/lambda functions).

 - When captured by value, original variable (outside the function, but in the same sorrounding  scope) will be copied into the function and, therefore, can't be altered by the function outside its own inner scope; the other way, by reference, the captured variable can be altered inside the function's sorrounding scope whereas will alter the original variable outside the functions inner scope.

The overall lambda function syntax is as follows:

``[<capture>] (<params>) <specs> -> <return_type> {<body>} ``

where:

- ``<capture>`` is where we define the sorrounding scope variables will be captured (by value or reference); we can also define new variables here (generalized capture c++14); it can also be empty, meaning no variable is being captured; it is also possible to define a default capture mode, and also the exceptional "opposite" capture mode;
- ``<params>`` is the optional list of parameters just like normal functions;
- ``<specs>`` is the optional specifications of the function (like mutable, exceptions/throw, etc);
- ``<return_type>`` is the optional explicit return type (also known as trailing-return-type);
- ``<body>`` contains the execution logic of the function.

Whenever we want to "name" the lambda function, we can use syntax like: `` auto name = [...] (...) {...} ``

Since c++14 we can also "define" new variables into the capture clause. This acts as a mean to introduce new "member variables" to the lambda function. The initialization of the variables can be made as any arbitrary expression. It is usefull to capture "move-only variables" (like unique_ptr) from the sorrounding scope into the lambda function's inner scope.

##### **Good references**: ##### 
  1) https://hackingcpp.com/cpp/design/function_objects.html
  2) https://learn.microsoft.com/en-us/cpp/standard-library/function-objects-in-the-stl?view=msvc-170
  3) https://www.geeksforgeeks.org/functors-in-cpp/
  4) https://learn.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp
  5) https://hackingcpp.com/cpp/lang/lambda_basics.html