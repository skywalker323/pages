# Data Structures
# C++ Data Structures and Algorithms Cheat Sheet

## Table of Contents

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [C++ Data Structures and Algorithms Cheat Sheet](#c-data-structures-and-algorithms-cheat-sheet)
	- [Table of Contents](#table-of-contents)
	- [1.0 Data Structures](#10-data-structures)
		- [1.1 Overview](#11-overview)
		- [1.2 Vector `std::vector`](#12-vector-stdvector)
		- [1.3 Deque `std::deque`](#13-deque-stddeque)
		- [1.4 List `std::list` and `std::forward_list`](#14-list-stdlist-and-stdforward_list)
		- [1.5 Map `std::map` and `std::unordered_map`](#15-map-stdmap-and-stdunordered_map)
		- [1.6 Set `std::set`](#16-set-stdset)
		- [1.7 Stack `std::stack`](#17-stack-stdstack)
		- [1.8 Queue `std::queue`](#18-queue-stdqueue)
		- [1.9 Priority Queue `std::priority_queue`](#19-priority-queue-stdpriority_queue)
		- [1.10 Heap `std::priority_queue`](#110-heap-stdpriority_queue)
		- [1.11 nth_element 'nth_element`](#nth_element)
		- [1.12 string `string` ](#string)
		- [1.13 istringstream `istringstream`](#istringstream)
		- [1.14 other `other`](#other)
		- [1.15 Sorting Algorithms `Sorting Algorithms`](#Sorting_Algorithms)
		- [1.16 Sorted Data Algorithms `Sorted Data Algorithms`](#Sorted_Data_Algorithms)
		- [1.17 Tuple `Tuple`](#Tuple)

	- [2.0 Trees](#20-trees)
		- [2.1 Binary Tree](#21-binary-tree)
		- [2.2 Balanced Trees](#22-balanced-trees)
		- [2.3 Binary Search](#23-binary-search)
		- [2.4 Depth-First Search](#24-depth-first-search)
		- [2.5 Breadth-First Search](#25-breadth-first-search)
	- [3.0 NP Complete Problems](#30-np-complete-problems)
		- [3.1 NP Complete](#31-np-complete)
		- [3.2 Traveling Salesman Problem](#32-traveling-salesman-problem)
		- [3.3 Knapsack Problem](#33-knapsack-problem)
	- [4.0 Algorithms](#40-algorithms)
		- [4.1 Insertion Sort](#41-insertion-sort)
		- [4.2 Selection Sort](#42-selection-sort)
		- [4.3 Bubble Sort](#43-bubble-sort)
		- [4.4 Merge Sort](#44-merge-sort)
		- [4.5 Quicksort](#45-quicksort)

<!-- /TOC -->


## 1.0 Data Structures
### 1.1 Overview

![DataStructures](data_structures/Data%20Structures.png "Data Structures")

![ComplexityChart](data_structures/Complexity%20Chart.png "Complexity Chart")

![DataStructureSelection](data_structures/Data%20Structures%20Selection.png "Data Structures Selection")
-------------------------------------------------------
### 1.2 Vector `std::vector` (Sequence Containers)
**Use for**
* Simple storage
* Adding but not deleting
* Serialization
* Quick lookups by index
* Easy conversion to C-style arrays
* Efficient traversal (contiguous CPU caching)

**Do not use for**
* Insertion/deletion in the middle of the list
* Dynamically changing storage
* Non-integer indexing

**Time Complexity**

| Operation    | Time Complexity |
|--------------|-----------------|
| Insert Head  |          `O(n)` |
| Insert Index |          `O(n)` |
| Insert Tail  |          `O(1)` |
| Remove Head  |          `O(n)` |
| Remove Index |          `O(n)` |
| Remove Tail  |          `O(1)` |
| Find Index   |          `O(1)` |
| Find Object  |          `O(n)` |

**Example Code**
```c++
std::vector<int> v;

//---------------------------------
// General Operations
//---------------------------------

// Size
unsigned int size = v.size();

// Insert head, index, tail
v.insert(v.begin(), value);             // head
v.insert(v.begin() + index, value);     // index
v.push_back(value);                     // tail

// Access head, index, tail
int head = v.front();       // head
head = v[0];                // or using array style indexing

int value = v.at(index);    // index
value = v[index];           // or using array style indexing

int tail = v.back();        // tail
tail = v[v.size() - 1];     // or using array style indexing

// Iterate
for(std::vector<int>::iterator it = v.begin(); it != v.end(); it++) {
    std::cout << *it << std::endl;
}

// Remove head, index, tail
v.erase(v.begin());             // head
v.erase(v.begin() + index);     // index
v.pop_back();                   // tail
v.erase(std::remove(v.begin(), v.end(), 5), v.end()); // Erase–remove idiom

// two dims(10x10) vect initied to -1
std::vector<std::vector<int>> vi3(10, std::vector<int>(10, -1));

// Clear
v.clear();
```
-------------------------------------------------------
### 1.3 Deque `std::deque` (Sequence Containers)
**Use for**
* Similar purpose of `std::vector`
* Basically `std::vector` with efficient `push_front` and `pop_front`

**Do not use for**
* C-style contiguous storage (not guaranteed)

**Notes**
* Pronounced 'deck'
* Stands for **D**ouble **E**nded **Que**ue

**Example Code**
```c++
std::deque<int> d;

//---------------------------------
// General Operations
//---------------------------------

// Insert head, index, tail
d.push_front(value);                    // head
d.insert(d.begin() + index, value);     // index
d.push_back(value);                     // tail

// Access head, index, tail
int head = d.front();       // head
int value = d.at(index);    // index
int tail = d.back();        // tail

// Size
unsigned int size = d.size();

// Iterate
for(std::deque<int>::iterator it = d.begin(); it != d.end(); it++) {
    std::cout << *it << std::endl;
}

// Remove head, index, tail
d.pop_front();                  // head
d.erase(d.begin() + index);     // index
d.pop_back();                   // tail

// Clear
d.clear();
```
-------------------------------------------------------
### 1.4 List `std::list` and `std::forward_list` (Sequence Containers)
**Use for**
* Insertion into the middle/beginning of the list
* Efficient sorting (pointer swap vs. copying)

**Do not use for**
* Direct access

**Time Complexity**

| Operation    | Time Complexity |
|--------------|-----------------|
| Insert Head  |          `O(1)` |
| Insert Index |          `O(n)` |
| Insert Tail  |          `O(1)` |
| Remove Head  |          `O(1)` |
| Remove Index |          `O(n)` |
| Remove Tail  |          `O(1)` |
| Find Index   |          `O(n)` |
| Find Object  |          `O(n)` |

**Example Code**
```c++
std::list<int> l;

//---------------------------------
// General Operations
//---------------------------------

// Insert head, index, tail
l.push_front(value);                    // head
l.insert(l.begin() + index, value);     // index
l.push_back(value);                     // tail

// Access head, index, tail
int head = l.front();                                           // head
int value = std::next(l.begin(), index);		        // index
int tail = l.back();                                            // tail

// Size
unsigned int size = l.size();

// Iterate
for(std::list<int>::iterator it = l.begin(); it != l.end(); it++) {
    std::cout << *it << std::endl;
}

// Remove head, index, tail
l.pop_front();                  // head
l.erase(l.begin() + index);     // index
l.pop_back();                   // tail

// Clear
l.clear();

//---------------------------------
// Container-Specific Operations
//---------------------------------

// Splice: Transfer elements from list to list
//	splice(iterator pos, list &x)
//  	splice(iterator pos, list &x, iterator i)
//  	splice(iterator pos, list &x, iterator first, iterator last)
l.splice(l.begin() + index, list2);

// Remove: Remove an element by value
l.remove(value);

// Unique: Remove duplicates
l.unique();

// Merge: Merge two sorted lists
l.merge(list2);

// Sort: Sort the list
l.sort();

// Reverse: Reverse the list order
l.reverse();
```

	Vector vs Deque vs List :
		◊ Vector only push_back and deque/list both push_back and push_front
		◊ A vector is a single contiguous memory block.
		◊ A deque is a set of linked memory blocks, where more than one element is stored in each memory block.
		◊ A list is a set of elements dispersed in memory, i.e.: only one element is stored per memory "block".


-------------------------------------------------------
### 1.5 Map `std::map` and `std::unordered_map`
**Use for**
* Key-value pairs
* Constant lookups by key
* Searching if key/value exists
* Removing duplicates
* `std::map`
    * Ordered map
* `std::unordered_map`
    * Hash table

**Do not use for**
* Sorting

**Notes**
* Typically ordered maps (`std::map`) are slower than unordered maps (`std::unordered_map`)
* Maps are typically implemented as *binary search trees*

**Time Complexity**

**`std::map`**

| Operation           | Time Complexity |
|---------------------|-----------------|
| Insert              |     `O(log(n))` |
| Access by Key       |     `O(log(n))` |
| Remove by Key       |     `O(log(n))` |
| Find/Remove Value   |     `O(log(n))` |

**`std::unordered_map`**

| Operation           | Time Complexity |
|---------------------|-----------------|
| Insert              |          `O(1)` |
| Access by Key       |          `O(1)` |
| Remove by Key       |          `O(1)` |
| Find/Remove Value   |              -- |

**Example Code**
```c++
std::map<std::string, std::string> m;

//---------------------------------
// General Operations
//---------------------------------

// Insert
m.insert(std::pair<std::string, std::string>("key", "value"));
m.insert{"key", "value"});

// Access by key
std::string value = m.at("key");

// Size
unsigned int size = m.size();

// Iterate
for(std::map<std::string, std::string>::iterator it = m.begin(); it != m.end(); it++) {
    std::cout << *it << std::endl;
}

for(auto const &i : m)
 std::cout << it.first << it.second << std::endl;

// Remove by key
m.erase("key");

// Clear
m.clear();

//---------------------------------
// Container-Specific Operations
//---------------------------------

// Find if an element exists by key
bool exists = (m.find("key") != m.end());

// Count the number of elements with a certain key
unsigned int count = m.count("key");
```
-------------------------------------------------------
### 1.6 Set `std::set`
**Use for**
* Removing duplicates
* Ordered dynamic storage

**Do not use for**
* Simple storage
* Direct access by index

**Notes**
* Sets are often implemented with binary search trees

**Time Complexity**

| Operation    | Time Complexity |
|--------------|-----------------|
| Insert       |     `O(log(n))` |
| Remove       |     `O(log(n))` |
| Find         |     `O(log(n))` |

**Example Code**
```c++
std::set<int> s;

//---------------------------------
// General Operations
//---------------------------------

// Insert
s.insert(20);

// Size
unsigned int size = s.size();

// Iterate
for(std::set<int>::iterator it = s.begin(); it != s.end(); it++) {
    std::cout << *it << std::endl;
}

// Remove
s.erase(20);

// Clear
s.clear();

//---------------------------------
// Container-Specific Operations
//---------------------------------

// Find if an element exists
bool exists = (s.find(20) != s.end());

// Count the number of elements with a certain value
unsigned int count = s.count(20);
```
-------------------------------------------------------
### 1.7 Stack `std::stack` (Adapter on (Sequence Containers))
**Use for**
* First-In Last-Out operations
* Reversal of elements

**Time Complexity**

| Operation    | Time Complexity |
|--------------|-----------------|
| Push         |          `O(1)` |
| Pop          |          `O(1)` |
| Top          |          `O(1)` |

**Example Code**
```c++
std::stack<int> s;

//---------------------------------
// Container-Specific Operations
//---------------------------------

// Push
s.push(20);

// Size
unsigned int size = s.size();

// Pop
s.pop();

// Top
int top = s.top();
```
-------------------------------------------------------
### 1.8 Queue `std::queue` (Adapter on (Sequence Containers))
**Use for**
* First-In First-Out operations
* Ex: Simple online ordering system (first come first served)
* Ex: Semaphore queue handling
* Ex: CPU scheduling (FCFS)

**Notes**
* Often implemented as a `std::deque`

**Example Code**
```c++
std::queue<int> q;

//---------------------------------
// General Operations
//---------------------------------

// Insert
q.push(value);

// Access head, tail
int head = q.front();       // head
int tail = q.back();        // tail

// Size
unsigned int size = q.size();

// Remove
q.pop();
```
-------------------------------------------------------
### 1.9 Priority Queue `std::priority_queue` (Adapter on (Sequence Containers))
**Use for**
* First-In First-Out operations where **priority** overrides arrival time
* Ex: CPU scheduling (smallest job first, system/user priority)
* Ex: Medical emergencies (gunshot wound vs. broken arm)
* **use multiset if you have to delete an element**
* **use rbegin rend of multiset to use small or bigger element**

**Notes**
* Often implemented as a `std::vector`

**Example Code**
```c++
std::priority_queue<int> p;

//---------------------------------
// General Operations
//---------------------------------

// Insert
p.push(value);

// Access
int top = p.top();  // 'Top' element

// Size
unsigned int size = p.size();

// Remove
p.pop();

priority_queue <int> prq;
for (int i=0; i<5; i++) prq.push(i+1); // 5 4 3 2 1 default bigger element on top
cout << prq.top() << endl; // 5
prq.pop (); // removes 5
priority_queue<int> q;
for(int n : {1,8,5,6,3,4,0,9,7,2}) q.push(n); //9 8 7 6 5 4 3 2 1 0
std::priority_queue<int, std::vector<int>, std::greater<int> > q2;
for(int n : {1,8,5,6,3,4,0,9,7,2})       q2.push(n); // 0 1 2 3 4 5 6 7 8 9
// Using lambda to compare elements.
auto cmp = [](int left, int right) { return (left ^ 1) < (right ^ 1); };
std::priority_queue<int, std::vector<int>, decltype(cmp)> q3(cmp);
// 8 9 6 7 4 5 2 3 0 1
auto p_comp = [] (const std::pair<int, int>& left, const std::pair<int, int>& right) { return left.first < right.first;};
std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, decltype(p_comp)> q4(p_comp);

```
-------------------------------------------------------
### 1.10 Heap `std::priority_queue`
**Notes**
* A heap is essentially an instance of a priority queue
* A **min** heap is structured with the root node as the smallest and each child subsequently larger than its parent
* A **max** heap is structured with the root node as the largest and each child subsequently smaller than its parent
* A min heap could be used for *Smallest Job First* CPU Scheduling
* A max heap could be used for *Priority* CPU Scheduling

**Max Heap Example (using a binary tree)**

![MaxHeap](General/MaxHeap.png)




### 1.11 nth_element
**Example Code**
```c++
std::vector<int> v = {0,1,2,3,4,5,6,7,8,9};
std::nth_element(v.begin(), v.begin() + v.size()/2, v.end());
// find nth smallest element in the array => this case find median
nth_element is a partial sorting algorithm that rearranges elements in [first, last) such that:

std::nth_element(v.begin(), v.begin()+1, v.end(), std::less<int>());
The element pointed at by nth is changed to whatever element would occur in that position
if [first, last) were sorted.
All of the elements before this new nth element are less than or equal to the elements
after the new nth element.


 std::vector<int> v{5, 6, 4, 3, 2, 6, 7, 9, 3};
    std::nth_element(v.begin(), v.begin() + v.size()/2, v.end());
    std::cout << "The median is " << v[v.size()/2] << '\n';
     std::nth_element(v.begin(), v.begin()+1, v.end(), std::greater<int>());
    std::cout << "The second largest element is " << v[1] << '\n';
	This means that std::nth_element can bail out early - as soon as it can tell what
	the n'th element of your range is going to be, it can stop. For instance, for a range
	[9,3,6,2,1,7,8,5,4,0]
	asking it to give you the fourth element may yield something like
	[2,0,1,3,8,5,6,9,7,4]
        The list was partially sorted, just good enough to be able to tell that the
        fourth element in order will be 3.

```
-------------------------------------------------------
### 1.12 string
``` cpp
	string str1("first string");
	string str4(str1, 6, 6); //    from 6th index (second parameter) & 6 characters (third parameter)
	string str3(5, '#');// initialization by character with number of occurence
	str2.clear() ; // clears all the chars
	char ch_f = str6.front();  // Same as "ch_f = str6[0];"
	char ch_b = str6.back();   // "ch_b = str6[str6.length() - 1];"
	str6.append(" extension"); 	//  same as str6 += " extension"
	str4.append(str6, 0, 6);  // from 0th position append 6 characters
	if (str6.find(str4) != string::npos) cout << "str4 found in str6 at " << str6.find(str4) ;
	Find returns zero index based index
	str6.substr(7, 3) // substring from 7th index 3 chars
	str6.substr(7) // substring from position 7th index till the end
	str6.erase(7, 4); // erase 4 chars from position 7
	str6 = "This is a examples";
	//  replace(a, b, str)  remove  b characters from a index and place str at index a
	str6.replace(2, 7, "ese are test"); // these are test examples
	str.find_first_of("reef") // find position of any character in r/e/f

```
-------------------------------------------------------
### 1.13 istringstream
```c++
istringstream iss("/a/b/c/../../c/")
while(getline(iss, dir, "/"){ std::cout << dir << std::endl; // prints a b c .. .. c}
const char* const delim = ", ";
std::ostringstream imploded;
std::copy(strings.begin(), strings.end(),  std::ostream_iterator<std::string>(imploded, delim));

auto it = std::find(src.cbegin(), src.cend(), 'd');

std::copy(it, src.cend(), std::ostream_iterator<char>(std::cout));
````
-------------------------------------------------------
### 1.14 other
``` c++
std::partition
	    std::vector<int> v = {0,1,2,3,4,5,6,7,8,9};
	    auto it = std::partition(v.begin(), v.end(), [](int i){return i % 2 == 0;});
	    std::copy(std::begin(v), it, std::ostream_iterator<int>(std::cout, " "));
	    std::cout << " * ";
	    std::copy(it, std::end(v), std::ostream_iterator<int>(std::cout, " "));
	    //    0 8 2 6 4  *  5 3 7 1 9
	template <class ForwardIt>
 void quicksort(ForwardIt first, ForwardIt last)
 {
    if(first == last) return;
    auto pivot = *std::next(first, std::distance(first,last)/2);
    ForwardIt middle1 = std::partition(first, last,
                         [pivot](const auto& em){ return em < pivot; });
    ForwardIt middle2 = std::partition(middle1, last,
                         [pivot](const auto& em){ return !(pivot < em); });
    quicksort(first, middle1);
    quicksort(middle2, last);
 }

back_inserter:
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::fill_n(std::back_inserter(v), 3, -1);
    for (int n : v) std::cout << n << ' '; //1 2 3 4 5 6 7 8 9 10 -1 -1 -1

int getRandNum_New() {
    static std::minstd_rand eng{std::random_device{}()};
    static std::uniform_int_distribution<int> dist{0, 5};
    return dist(eng);

    // or
    /* static std::minstd_rand eng{std::random_device{}()};
       return eng() % 6;     */
generate_random_array
	int arr[100];
	std::random_device rd;
	std::default_random_engine dre(rd());
	std::uniform_int_distribution<int> uid(0,9);
	std::generate(arr, arr + sizeof(arr) / sizeof(int), [&] () { return uid(dre); });

Std::fill
	std::vector<int> v{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
	std::fill(v.begin(), v.end(), -1); // all -1s

Std::generate
    std::generate(v.begin(), v.end(), [n = 0] () mutable { return n++; }); //  0 1 2 3 …vec.size()
Std::iota
   std::list<int> l(10);
    std::iota(l.begin(), l.end(), -4); //Contents of the list: -4 -3 -2 -1 0 1 2 3 4 5

Std::fill vs std::generate vs std::iota

Fill assigns a value whereast generate generates a value based on function provided
Iota generates sequence of numbers from a range or initial value etc


Stream iterators:
std::istringstream istr("1\t 2     3 4");
std::vector<int> v;

// Constructing stream iterators and copying data from stream into vector.
std::copy(
    // Iterator which will read stream data as integers.
    std::istream_iterator<int>(istr),
    // Default constructor produces end-of-stream iterator.
    std::istream_iterator<int>(),
    std::back_inserter(v));

// Print vector contents.
std::copy(v.begin(), v.end(),
    //Will print values to standard output as integers delimeted by " -- ".
    std::ostream_iterator<int>(std::cout, " -- "));

std::istreambuf_iterartor
std::ostream_iterator
std::istream_iterator
````
-------------------------------------------------------
### 1.15 Sorting Algorithms
```cpp
std::array<int, 10> s{5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

std::partial_sort(s.begin(), s.begin() + 3, s.end());
for (int a : s) {
    std::cout << a << " "; //0 1 2 7 8 6 5 9 4 3
}
std::array<int, 10> s = {5, 7, 4, 2, 8, 6, 1, 9, 0, 3};

// sort using the default operator< default if a <  b returns true
std::sort(s.begin(), s.end()); // 0 1 2 3 4 5 6 7 8 9
std::sort(s.begin(), s.end(), std::greater<int>());  //9 8 7 6 5 4 3 2 1 0
// sort using a lambda expression
std::sort(s.begin(), s.end(), [](int a, int b) {  // 9 8 7 6 5 4 3 2 1 0
        return a > b;
});
```
-------------------------------------------------------
### 1.16 Sorted Data Algorithms
 These are the algorithms that require data being pre-sorted.
	• Binary search: It checks for data in a data range. It returns a Boolean as a result.
	 std::vector<int> haystack {1, 3, 4, 5, 9};
	    std::vector<int> needles {1, 2, 3};

	    for (auto needle : needles) {
	        std::cout << "Searching for " << needle << '\n';
	        if (std::binary_search(haystack.begin(), haystack.end(), needle)) {
	            std::cout << "Found " << needle << '\n';
	        } else {
	            std::cout << "no dice!\n";
	        }
	    }

	• Merge: Merge operation does two ranges of sorted data into one range of big sorted data.

	    std::vector<int> dst;
	    std::merge(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(dst));


	• Set operations: Set Union, Set intersection, Set difference,
	 Symmetric difference all these are set operations.
	std::vector<int> v1 = {1, 2, 3, 4, 5};
	        std::vector<int> v2 = {      3, 4, 5, 6, 7};
	        std::vector<int> dest1;

	        std::set_union(v1.begin(), v1.end(),
	                       v2.begin(), v2.end(),
	                       std::back_inserter(dest1)); // 1 2 3 4 5 6 7

	 std::vector<int> v1 {1, 2, 5, 5, 5, 9}; // sorted vector
	    std::vector<int> v2 {2, 5, 7}; // sorted vector
	    std::vector<int> diff;

	    std::set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(),
	            std::inserter(diff, diff.begin()));
	       // 1 2 5 5 5 9 minus 2 5 7 is: 1 5 5 9


	std::vector<int> v1{1,2,3,4,5,6,7,8};
	    std::vector<int> v2{        5,  7,  9,10};
	    std::sort(v1.begin(), v1.end());
	    std::sort(v2.begin(), v2.end());
	     std::vector<int> v_intersection;
	    std::set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(v_intersection)); // 5 7

	std::unique
		std::sort(v.begin(), v.end()); // {1 1 2 3 4 4 5}
	       print(3);
	       last = std::unique(v.begin(), v.end());
	       // v now holds {1 2 3 4 5 x x}, where 'x' is indeterminate
	       v.erase(last, v.end());

-------------------------------------------------------
### 1.17 Tuple
**Example Code**
```c++
std::tuple<int, short, float, std::string>  tps{1, 2, 10.0f, "aa"};
auto a1 = std::get<0>(tps);
auto a2 = std::get<1>(tps);


double gpa1;
char grade1;
std::string name1;
auto t =  std::make_tuple(3.8, 'A', "Lisa Simpson");
std::tie(gpa1, grade1, name1) = t;

std::partition:
std::vector<int> v = {0,1,2,3,4,5,6,7,8,9};
auto it = std::partition(v.begin(), v.end(), [](int i){return i % 2 == 0;});
// 0 8 2 6 4 5 3 7 1 9omp);

```
-------------------------------------------------------


## 2.0 Trees
### 2.1 Binary Tree
* A binary tree is a tree with at most two (2) child nodes per parent
* Binary trees are commonly used for implementing `O(log(n))` operations for ordered maps, sets, heaps, and binary search trees
* Binary trees are **sorted** in that nodes with values greater than their parents are inserted to the **right**, while nodes with values less than their parents are inserted to the **left**

**Binary Search Tree**

![BinarySearchTree](General/BinarySearchTree.png)
-------------------------------------------------------
### 2.2 Balanced Trees
* Balanced trees are a special type of tree which maintains its balance to ensure `O(log(n))` operations
* When trees are not balanced the benefit of `log(n)` operations is lost due to the highly vertical structure
* Examples of balanced trees:
    * AVL Trees
    * Red-Black Trees

-------------------------------------------------------
### 2.3 Binary Search
**Idea:**
1. If current element, return
2. If less than current element, look left
3. If more than current element, look right
4. Repeat

**Data Structures:**
* Tree
* Sorted array

**Space:**
* `O(1)`

**Best Case:**
* `O(1)`

**Worst Case:**
* `O(log n)`

**Average:**
* `O(log n)`

**Visualization:**

![BinarySearch](Searching/Animations/Binary%20Search.gif "Binary Search")
-------------------------------------------------------
### 2.4 Depth-First Search
**Idea:**
1. Start at root node
2. Recursively search all adjacent nodes and mark them as searched
3. Repeat

**Data Structures:**
* Tree
* Graph

**Space:**
* `O(V)`, `V = number of verticies`

**Performance:**
* `O(E)`, `E = number of edges`

**Visualization:**

![DepthFirstSearch](Searching/Animations/Depth-First%20Search.gif "Depth-First Search")
-------------------------------------------------------
### 2.5 Breadth-First Search
**Idea:**
1. Start at root node
2. Search neighboring nodes first before moving on to next level

**Data Structures:**
* Tree
* Graph

**Space:**
* `O(V)`, `V = number of verticies`

**Performance:**
* `O(E)`, `E = number of edges`

**Visualization:**

![DepthFirstSearch](Searching/Animations/Breadth-First%20Search.gif "Breadth-First Search")
-------------------------------------------------------
## 3.0 NP Complete Problems
### 3.1 NP Complete
* **NP Complete** means that a problem is unable to be solved in **polynomial time**
* NP Complete problems can be *verified* in polynomial time, but not *solved*

-------------------------------------------------------
### 3.2 Traveling Salesman Problem

-------------------------------------------------------
### 3.3 Knapsack Problem

[Implementation](NP-complete/knapsack/)

-------------------------------------------------------

## 4.0 Algorithms
###  4.1 Insertion Sort
#### Idea
1. Iterate over all elements
2. For each element:
    * Check if element is larger than largest value in sorted array
3. If larger: Move on
4. If smaller: Move item to correct position in sorted array

#### Details
* **Data structure:** Array
* **Space:** `O(1)`
* **Best Case:** Already sorted, `O(n)`
* **Worst Case:** Reverse sorted, `O(n^2)`
* **Average:** `O(n^2)`

#### Advantages
* Easy to code
* Intuitive
* Better than selection sort and bubble sort for small data sets
* Can sort in-place

#### Disadvantages
* Very inefficient for large datasets

#### Visualization

![InsertionSort](Sorting/Animations/Insertion%20Sort.gif "Insertion Sort")
-------------------------------------------------------
### 4.2 Selection Sort
#### Idea
1. Iterate over all elements
2. For each element:
    * If smallest element of unsorted sublist, swap with left-most unsorted element

#### Details
* **Data structure:** Array
* **Space:** `O(1)`
* **Best Case:** Already sorted, `O(n^2)`
* **Worst Case:** Reverse sorted, `O(n^2)`
* **Average:** `O(n^2)`

#### Advantages
* Simple
* Can sort in-place
* Low memory usage for small datasets

#### Disadvantages
* Very inefficient for large datasets

#### Visualization

![SelectionSort](Sorting/Animations/Selection%20Sort.gif "Selection Sort")

![SelectionSort](Sorting/Animations/Selection%20Sort%202.gif "Selection Sort 2")
-------------------------------------------------------
### 4.3 Bubble Sort
#### Idea
1. Iterate over all elements
2. For each element:
    * Swap with next element if out of order
3. Repeat until no swaps needed

#### Details
* **Data structure:** Array
* **Space:** `O(1)`
* **Best Case:** Already sorted `O(n)`
* **Worst Case:** Reverse sorted, `O(n^2)`
* **Average:** `O(n^2)`

#### Advantages
* Easy to detect if list is sorted

#### Disadvantages
* Very inefficient for large datasets
* Much worse than even insertion sort

#### Visualization

![BubbleSort](Sorting/Animations/Bubble%20Sort.gif "Bubble Sort")
-------------------------------------------------------
### 4.4 Merge Sort
#### Idea
1. Divide list into smallest unit (1 element)
2. Compare each element with the adjacent list
3. Merge the two adjacent lists
4. Repeat

#### Details
* **Data structure:** Array
* **Space:** `O(n) auxiliary`
* **Best Case:** `O(nlog(n))`
* **Worst Case:** Reverse sorted, `O(nlog(n))`
* **Average:** `O(nlog(n))`

#### Advantages
* High efficiency on large datasets
* Nearly always O(nlog(n))
* Can be parallelized
* Better space complexity than standard Quicksort

#### Disadvantages
* Still requires O(n) extra space
* Slightly worse than Quicksort in some instances

#### Visualization

![MergeSort](Sorting/Animations/Merge%20Sort.gif "Merge Sort")

![MergeSort](Sorting/Animations/Merge%20Sort%202.gif "Merge Sort 2")
-------------------------------------------------------
### 4.5 Quicksort
#### Idea
1. Choose a **pivot** from the array
2. Partition: Reorder the array so that all elements with values *less* than the pivot come before the pivot, and all values *greater* than the pivot come after
3. Recursively apply the above steps to the sub-arrays

#### Details
* **Data structure:** Array
* **Space:** `O(n)`
* **Best Case:** `O(nlog(n))`
* **Worst Case:** All elements equal, `O(n^2)`
* **Average:** `O(nlog(n))`

#### Advantages
* Can be modified to use O(log(n)) space
* Very quick and efficient with large datasets
* Can be parallelized
* Divide and conquer algorithm

#### Disadvantages
* Not stable (could swap equal elements)
* Worst case is worse than Merge Sort

#### Optimizations
* Choice of pivot:
    * Choose median of the first, middle, and last elements as pivot
    * Counters worst-case complexity for already-sorted and reverse-sorted

#### Visualization

![QuickSort](Sorting/Animations/Quicksort.gif)
