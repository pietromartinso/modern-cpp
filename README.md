# Starting my recycling studies on C++

- STL / Containers
- Smart Pointers
- Lambda Functions (functors)
- Multithreading
- Debugging: memory leaks, segmentation faults & multithreading 
- c++11
- c++14
- c++17
- c++20

## Containers (STL) 

Containers are data-structure objects that allow us to store elements. They are built as templates, so we can insert as any data type needed, including other objects. Containers manages the memory used in the storage process as well as implements vairous member functions that allow us to interact with the container (add, remove, sort, etc).

### Sequence containers

- array: 
  - Static contiguous memory (fixed size);
  - Random access without overhead;
  - Fast linear traversal;
  - Can't resize (thus, size() function must be a constant expression).

    Methods:
      - at() can access elements of the array;
      - get() can also access elements of the array but isn't member of the array class - this is an overloeaded function from class tuple;
      - size() retrieves the size of the array (max_size() does the same, since we cant resize arrays);
      - operator[] access elements like C language
      - front() returns the reference to the fist elemento of the array
      - back() returns the reference to the fist elemento of the array
      - swap() swaps elemenents positions inside the array
      - empty() returns true if the array size is zero
      - fill() populates the array with a specific vlue

- vector:
  - Dynamic contiguous memory (can be resized);
  - C++ default container;
  - Random access withour overhead;
  - Fast linear traversal;
  - Fast insertion/deletion at the end (back);
    - Inserting data at the end may have diferential time, since there will be situations in which a resize will be needed;
  - Insertion/deletion in the middle or in the middle takes linear time;

- deque:
  - Dynamic not (necessarily) contiguous in memory;
  - Holds references for both extremities;
  - Fast insertion/deletion at both extremities;
  - Accessing elements can be made at constant time;
  - Insertion/deletion inside the structure has linear time;
  - Almost same operations as vectors, plus add/remove elements at the front;

- list :
  - Doubly-linked list (circular) not contiguous in memory;
  - Slow traversal (when compared to vectors);
    - But, when position is found, insertion/deletion are fast;
  - Fast splicing 



- forward_list (introduced in c++11):
  - Singly-linked list not contiguous in memory;
  - Lower memory overhead because it doesn't hold "reversal" references;


  https://www.geeksforgeeks.org/containers-cpp-stl/
  https://hackingcpp.com/cpp/cheat_sheets.html#hfold2a