# Starting my recycling studies on C++

- STL / Containers
- Smart Pointers
- Lambda Functions (functors)
- Multithreading

- Memory management, memory leaks, smart pointers: https://hackingcpp.com/cpp/lang/manual_memory_management.html 
- Segmentation faults 
- Debugging
- c++11: https://en.cppreference.com/w/cpp/11
- c++14: https://en.cppreference.com/w/cpp/14
- c++17: https://en.cppreference.com/w/cpp/17
- c++20: https://en.cppreference.com/w/cpp/20
- c++23: https://en.cppreference.com/w/cpp/23

## Containers (STL) 

https://hackingcpp.com/cpp/std/sequence_containers.html

Containers are data-structure objects that allow us to store elements. They are built as templates, so we can insert as any data type needed, including other objects. Containers manages the memory used in the storage process as well as implements vairous member functions that allow us to interact with the container (add, remove, sort, etc). 

### Sequence containers

All sequence containers are deeply copyable, that means copying creates a new container object and copies all values; all sequence containers are deeply assignable, that menas all contained objects are copied from source to assignment target; all sequence containers are deeply comparable, which means that comparision between two containers implies comparing contained objects; all sequence containers are deeply owned, which means that destroying the container also destroys all contained objects.

As of c++17, when we use the initializer list, '{}', we doesn't need to specify what is the template type of the container, since it can be infered by the constructor of the container.

- Common Interface
  - container.begin() & container.end() iterators (as of c++11, there are free standing static functions std::begin(container) & std::end(container) )
    - there are also constant iterators cbegin() and cend() - same standing static functions since c++11
  - container.empty() & c++11 std::empty(container)

- array<T, size> (introduced in c++11)
  - Static contiguous memory (fixed size);
  - Random access without overhead;
  - Fast linear traversal;
  - Can't resize (thus, size() function must be a constant expression).
  - Reference: https://en.cppreference.com/w/cpp/container/array

    Member functions:
      - at() can access elements of the array;
      - operator[] access elements like C language;
      - front() returns the reference to the fist elemento of the array;
      - back() returns the reference to the fist elemento of the array;
      - size() retrieves the size of the array (max_size() does the same, since we cant resize arrays);
      - swap() swaps elemenents positions inside the array
      - empty() returns true if the array size is zero
      - fill() populates the array with a specific vlue

- vector<T>:
  - Dynamic contiguous memory (can be resized);
  - C++ default container;
  - Random access withour overhead;
  - Fast linear traversal;
  - Fast insertion/deletion at the end (back);
    - Inserting data at the end may have diferential time, since there will be situations in which a resize will be needed - mostly constant time;
  - Insertion/deletion in the middle or in the middle takes linear time;
  - Reference: https://en.cppreference.com/w/cpp/container/vector

- deque<T>:
  - Dynamic not (necessarily) contiguous in memory;
  - Holds references for both extremities;
  - Fast insertion/deletion at both extremities;
  - Accessing elements can be made at constant time;
  - Insertion/deletion inside the structure has linear time;
  - Almost same operations as vectors, plus add/remove elements at the front;
  - Reference: https://en.cppreference.com/w/cpp/container/deque

- list<T>:
  - Doubly-linked list (circular) not contiguous in memory;
  - Slow traversal (when compared to vectors);
    - But, when position is found, insertion/deletion are fast - constant time;
  - Fast splicing 
  - Reference: https://en.cppreference.com/w/cpp/container/list

- forward_list<T> (introduced in c++11):
  - Singly-linked list not contiguous in memory;
  - Lower memory overhead because it doesn't hold "reversal" references;
  - Reference: https://en.cppreference.com/w/cpp/container/forward_list

- Which sequence container should one use?

  - if number os elements to be stored is small and known at compilation time: use array;
  - else, try vector;
    - if vector is too slow and/or memory usage is too high, check where are the possible bottlenecks;
      - if insertion/deletion at begin/end dominate: try deque
        - still bad solution? try list;
      - if copying elements and/or insert/erase at random positions dominate: try list;
        - memory usage is slightly high and reverse traversal not needed: try forward_list;
      - if too many memory allocations and/or consumption being high: try vector.reserve(aproximated_expected_size);
      - if everything else is too slow, non-linear memory access dominate and can't be avoided: try another custom approach or non-standard solution with better characteristics.

### Associative Containers

Associative containers can be more efficient in operations like insertion, deletion and search. They usually are organized as trees (ordered) or hash tables (unordered). They have also two distinct "types" of containers: the ones that allows only insertion of unique keys, and the ones that allows insertion of duplicate keys.

- set<KeyType>:
  - implemented as a red-black tree (ordered ascending or descending)
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at logarithmic complexity
  - composed of single values/objects (not pairs)

- unordered_set<KeyType> (introduced in c++11):
  - implemented as unordered hashtable
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of single values/objects (not pairs)

- multiset<KeyType>:
  - implemented as a red-black tree (ordered ascending or descending)
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at logarithmic complexity
  - composed of single values/objects (not pairs)

- unordered_multiset<KeyType> (introduced in c++11):
  - implemented as unordered hashtable
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of single values/objects (not pairs)

- map<KeyType, ValueType>:
  - implemented as a red-black tree (ordered ascending or descending)
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at logarithmic complexity
  - composed of pairs of values/objects (key-value pairs)

- unordered_map<KeyType, ValueType> (introduced in c++11):
  - implemented as unordered hashtable
  - doesn't allow insertion of repeated values (unique keys, only)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of pairs of values/objects (key-value pairs)

- multimap<KeyType, ValueType>:
  - implemented as a red-black tree (ordered ascending or descending)
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at logarithmic complexity
  - composed of pairs of values/objects (key-value pairs)

- unordered_multimap<KeyType, ValueType> (introduced in c++11):
  - implemented as unordered hashtable
  - it allows insertion of repeated values (not unique keys)
  - insertion, deletion and removal at constant complexity at best case and linear complexity in worst case scenarios
  - composed of pairs of values/objects (key-value pairs)
