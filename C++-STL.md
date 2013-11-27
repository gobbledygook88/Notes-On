# Notes On: C++ STL

## Introduction
The following document is a record of useful pointers and tips as I research the C++ Standard Template Library.

A variety of sources are used as I continually update this document. See the Resources section for a full list.

The primary resource is Stephan T. Lavavej's STL video lectures over at [Channel 9](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-), and as such, this document will be arranged as individual video lecture notes.

## Part 1: Containers, Iterators and Algorithms

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Introduction-to-STL-with-Stephan-T-Lavavej)_

The C++ Standard Template Library (STL) is a library, as opposed to a framework, for general computation focussed on speed and optimisation of general purpose tasks.

There are three main components to the STL: containers, iterators and algorithms. There are also functors. More on this later. Algorithms and containers are implemented without needing knowledge of each other, and only act on each other via iterators.

The STL extensively uses templates. Hence, when it gets instantiated by the compiler, everything is inline-able, with virtually no call overheads.

### Sequence containers

Containers, such as `vector`, stores a sequence of elements of any valid type. These include your own classes, as long as they are copyable. 

### Vectors and vector memory management

Vectors are flexible, expandable arrays which are contiguous in memory. They are also space efficient.

	std::vector<int> v;		// Default constructor

	v.push_back(11);
	v.push_back(22);
	v.push_back(33);

Since vectors are contiguous in memory, there will be times when more memory is required. In this case, a new contiguous memory block is allocated, and the existing elements are copied over. New elements can then be inserted to the new block; the old block is deleted.

Geometric memory allocation is used to do this, i.e. if there are N elements, vector will allocate memory for kN elements, where k > 1. Common implementations of the C++ STL use k = 1.5, or k = 2. This results in _amortized constant time_ for adding elements.

Containers are flexible, so it can store arbitrary number of elements. This prevents security and correctness problems, like buffer and heap overflows. Each container knows its own size.

	v.size();

_Undefined behaviour_ may occur if you try to access an element of a container, outside of the range of the number of elements, e.g.

	v[5] = 55;

The C++ standard does not specify what should happen in these cases.

### Accessing a vector via an iterator

Iterators point to elements within a container, in this case a vector. They are a generalisation of pointers, via operator overloading, abstracting away the implementations for different containters.

	vector<int>::iterator i = v.begin() + 1;

	std::cout << *i << std::endl;  // 22

`begin()` returns an iterator to the first element of the vector. `end()` returns an iterator to the element one past the end of the container. This is known as an _inclusive-exclusive range_. This makes empty ranges natural, i.e. an empty vector with have `begin()` and `end()` iterators pointing to the same element. It also imitates how a normal array works. (pointer arithmetic)

### Using algorithms on iterators

We can include the algorithm header and use any of its algorithms.

	#include <algorithm>
	...
	std::sort(v.begin(), v.end());

The sort algorithm is templated on iterator type. As long as the container is of random access type, e.g. vector, we can use `sort()` on it. `std::list` is a doubly-linked list and is not of random access type, i.e. we cannot access the nth element of the list in constant time, so we cannot use `sort()` on it.

If we have a vector of strings, by default, they are sorted lexicographically. But we can change this.

	#include <algorithm>
	#include <functional>
	#include <string>
	...
	std::vector<std::string> v;

	v.push_back("cat");
	v.push_back("antelope");
	v.push_back("puppy");
	v.push_back("bear");

	std::sort(v.begin(), v.end());  						// Lexicographical order
	std::sort(v.begin(), v.end(), std::greater<string>());  // Reverse lexicographical order

	// Using lambdas ...
	std::sort(v.begin(), v.end(), 
		[](const std::string& left, const std::string& right) {
			return left.size() < right.size();				// Soft by increasing string length
		});

To preserve element orders when the sort algorithm cannot determine which element should appear first, e.g if the lengths of strings are equal, then we can use `std::stable_sort()`. This is not the default as it may be less efficient than `sort()` due to additional checks.

(More containers: deque, forward_list)

## Part 2: Associate Containers

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-2-of-n)_

The associative containers in STL are `map`, `multimap`, `set`, and `multiset` (C++03), `unordered_map`, `unordered_multimap`, `unordered_set`, and `unordered_multiset` (C++0X).

### Map

Map is templated on its element type, on both its key and value. Elements are automatically sorted. 

	#include <map>
	...
	std::map<int, std::string> m;

	m[1] = "one";
	m[2] = "two";
	m[4] = "four";
	m[3] = "three";

If we have, as above, a `map<int, std::string>`, each element has type `pair<const int, std::string>`. We can also use iterators on maps.

	// Using const iterators and C++11 auto pointer
	// in place of std::map<int, std::string>::iterator
	for(auto i = m.cbegin(); i != m.cend(); ++i) {
		std::cout << "(" << i->first << ", " << i->second << ")" << std::endl;
	}

Map is generally implemented as a semi-balanced binary tree, or a red-black tree. This ensures efficient operations, such as

- Insertion: O(log N)
- Deletion: O(log N)
- Iteration: O(N)
- Destroy: O(N)

To read an element from a constant reference of a map, we need to use `find()`, instead of `operator[]`.

	void foo(const map<int, std::string>& m) {
		auto i = m.find(3);

		if(i == m.end()) {
			std::cout << "not found" << std::endl;
		} else {
			std::cout << i->second << std::endl;
		}
	}

Note that map is the only associative container which has `operator[]`.

The set container is similar to map in terms of operation run times, but it enforces uniqueness. It also permits logarithmic time loookup, as opposed to linear lookup time with vector or map.

### Unordered Map

Unordered maps work in the exact same way as map, but elements are unordered upon insertion. However, its data structure is very different to map. It is in fact implemented as a hash table. Each bucket will be associated with a chain of values, to support collisions.

Insertion into an unordered map is expected to be constant time. In the case of a collision, the worst case is O(N), i.e. efficiency degrades.

Generally use maps for performance guarantees. Unordered maps can be used when performance requires it.

### Deleting multiple entries from a vector

To do this, we need to use the erase-remove idiom. 

	#include <algorithm>
	#include <vector>
	...
	std::vector<int> v;

	v.push_back(11);
	v.push_back(22);
	v.push_back(33);
	v.push_back(44);
	v.push_back(55);

Removing each unwanted element from the vector with a loop will have performance drawbacks. Firstly, vector will maintain contiguous block memory. So when one element is deleted, it will shift the remaining elements back, in linear time. This is for each unwanted element, so it will result in quadratic time.

Instead, we first use `std::remove()`, and then `std::vector::erase()`, which are part of the STL `<algorithm>` header. These are non-member functions working on elements via iterators.

`remove()` will maintain two iterators to perform a linear pass on the vector to identify and move valid elements. `erase()` then removes the unwanted elements at the end of the vector.

	v.erase(remove_if(v.begin(), v.end(),
		[](int e) {
			return e % 2 == 1;
		}), v.end());

## Part 3: Smart Pointers

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-3-of-n)_

Introduced from C++0X, we use `shared_ptr` and `unique_ptr` in place of `new` and `delete`. Shared pointers were originally in the Boost library. 

Note that `auto_ptr` has been officially deprecated.

### Unique Pointers

A unique pointer is the unique owner of a dynamically allocated object. It is a smart pointer: a class which imitates a raw pointer via operator overloading (on `*` and `->`).

	#include <memory>
	...
	std::unique_ptr<int> up(new int(1729));

	std::cout << *up << std::endl;

We did not call `delete` since `unique_ptr` will destroy the object for us once it goes out of scope.

Ownership of the object can be transferred with the `std::move()` function.

	std::unique_ptr<int> up2 = std::move(up);

Unique pointers are not copy constructable -- it is move constructable -- but can be returned from functions. C++0X will automatically perform a move construction upon return.

Ownership transfer is implicit when unique pointer is returned from a method/function

### Shared Pointers

Shared pointers share ownership to an object -- they are reference counted shared pointers. Many shared pointers may point to the same object.

	std::shared_ptr<int> sp(new int(1234));

	std::shared_ptr<int> sp2(sp);  // Using copy constructor

	std::cout << *sp << std::endl;
	std::cout << *sp2 << std::endl;

The pointed object is destroyed when all the shared pointers pointing to the object are destroyed, preventing resource leaks. 

The reference counter is incremented when a new shared pointer points to the object. The reference counter is decremented when a shared pointer to the object is destroyed
	
The pointed object is deleted when the reference counter goes to zero.

	auto sp = make_shared<int>(1729);

`make_shared` is cleaner and a more efficient helper function. 

Shared pointers may be used in STL containers. For example, you could declare a vector of shared pointers.

## Part 4 & 5: Nurikabe Solver

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-4-of-n)_ _[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-5-of-n)_

Solve the Nurikabe puzzle to demonstrate STL. Refer to the source file on the video link page.

class Grid - Stores state of puzzle, top-level member functions, write to file

class Region - keep track of all the contiguous regions

## Part 6: Algorithms and Functors

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-6-of-n)_

STL algorithms are usually templated on iterator types (to work with arbitrary containers with arbitrary elements) and predicates. 

### Non-modifying/non-mutating algorithms

- `for_each`: Iterate over all elements to perform an operation.
- `count`: Counts the elements that match the passed value (`==`).

		#include <algorithm>
		...
		std::vector<int> v;
		v.push_back(11);
		v.push_back(22);
		v.push_back(33);
		v.push_back(44);
		v.push_back(55);

		std::cout << std::count(v.begin(), v.end(), 33) << std::endl; // 1

- `count_if`: Counts elements that match a certain criterion via the given predicate.

		bool even(int n) {
			return n % 2 == 0;
		}
		...
		std::cout << std::count_if(v.begin(), v.eng(), even) << std::endl; // 2

		// or using lambdas

		std::cout << std::count_if(v.begin(), v.end(), 
			[](int n) { return n % 2 == 0 }) << std::endl; // 2

- `find`: Finds a matching element in linear time
- `find_if`: Find an element that matches a certain criterion
- `equal`: Compares sequences match

		std::equal(first.begin(), first.end(), second.begin());

	- Note that lengths are not compared, we can check this manually if needed
	- The second sequence should contain at least as many elements as the first sequence; it is an error if otherwise
- `all_of`: Do _all_ of the elements satisfy the criterion?
- `any_of`: Do _any_ of the elements satisfy the criterion? 
- `none_of`: Do _none_ of the elements satisfy the criterion?

### Functors 

Functors can either be plain functions (non-member functions and we can get function pointers to them), or function objects (classes which overrides `operator()` ); anything that can be called with "()". Function objects can then either be hand-written or lambdas. Compilers tend to do a better job inlining function objects and lambdas.

### Modifying/Mutating algirithms

- `copy`: Copies from a range of input iterators into an output iterator. The output sequence container should have enough space the elements to be copied.
		
		std::copy(v.begin(), v.end(), u.begin());

	`copy()` can also be used to expand a container. 

		// WRONG - vector does not know it needs to expand (it is empty)
		std::vector<int> dest;

		std::copy(v.begin(), v.end(), dest.begin());

		// RIGHT
		std::vector<int> dest(5);

		std::copy(v.begin(), v.end(), dest.begin());

- `replace_if`: Replaces a element that matches a certain criterion.
- `transform`: Transforms the elements in a container

## Part 7: Algorithms and Sorting

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-7-of-n)_

We can also use back inserters to copy elements from one container to another (as another way of performing the copy task in the previous Part)

	const int arr[] = (22, 99, 33, 44, 55);

	vector<int> v;
	v.push_back(11);
	v.push_back(77);

	// Simple print out loop
	for(auto i = v.begin(); i != v.end(); ++i) {
		std::cout << *i << " ";
	}
	std::cout << std::endl;

	std::copy(arr, arr + sizeof(arr) / sizeof(arr[0]), std::back_inserter(v));

	// Print out via for_each and lambdas
	for_each(v.begin(), v.end(), [](int n) { std::cout << n << " "; });
	std::cout << std::endl;

Back inserters return an `insert_iterator`. Dereferencing and incrementing an `insert_iterator` adds a new element using `push_back`. For example, passing to to a `back_inserter` to copy will add elements.

Note that this example can be executed better via a range construct in a constructor

	std::vector<int> v(arr, arr + sizeof(arr) / sizeof(arr[0]));

However, 

	v.insert(v.end(), arr, arr + sizeof(arr) / sizeof(arr[0]))

is more efficient than `back_insertor` since it only calls `push_back` once. 

We can then sort this vector via STL

	sort(v.begin(), v.end());

	// Another way to print out
	copy(v.begin(), v.end(), ostream_iterator<int>(std::cout, " "));
	std::cout << std::endl;

Writing to `ostream_iterator` writes to ostream, but is not needed in C++ 11 as lambdas provide a cleaner design.

### Binary Search

`std::binary_search` is not particuarly useful as it just tells you the element is present or absent (it returns a `bool`). 

On the other hand, `std::lower_bound` returns the _first_ position where a value can be inserted and still maintain sorted order. If the element does not exist, `lower_bound` returns the first position where the new element could be inserted, and still maintains sorted order.

	auto i = lower_bound(v.begin(), e.end(), 50);

	std::cout << i - v.begin() << std::endl; // 4

`begin()` is returned if the element has to be inserted as the first element. `end()` is returned if the element has to be inserted as the last element.

Note that binary search requires that the sequence is sorted beforehand.

`std::upper_bound` returns the _last_ position where a value can be inserted and still maintain sorted order. (Remember include-exclude ranges.)

`std::equal_range` returns a _pair of iterators_, containing the lower bound and upper bound.

As we have seen so far, STL uses "less than" as the default comparators. Custom comparators should behave like "less than" (strict weak ordering), i.e. x compared to x should always return `false`, otherwise STL will return invalid results.

The order of "same" elements is not guaranteed. Use `std::stable_sort` if the order of "same" elements needs to be preserved. `std::partial_sort` allows you to sort a small subset of the full collection.

If this sequence was sorted, `std::nth_element` will return the nth_element.

### Set Operations

- `set_union` and `set_intersection`s can be obtained for sequences.
- `min` can be used to obtain the least element
- `max` returns the largest element
- `min_element` returns in iterator to the least element
- `min_max` returns the minimum and the maximum. This is more efficient than calling min and max separately. 

Most algorithms are in `<algorithm>` header file, but some are in `<numeric>` (for generic numerical algorithms).

- `accumulate`: Iterate to perform a binary operation value. Initial value can be specified here. The default version uses plus, but accumulate can be changed to use multiply to find the product.
- `iota`: Used to initialize a sequence of increasing numbers. (C++OX only)

		std::vector<int> v(10);

		iota(v.begin(), v.end(), 0); // {0,1,...,9}

### Transformations

`std::transform` has two overloads:
	
- unary: Perform a unary operation on an input sequence and write to an output sequence. In place transformation can be carried out by passing the same sequence as input as well as output.

		std::transform(v.begin(), v.end(), result, unary_operation);

- binary: Takes elements from the first and the second sequences, performs a binary operation and then writes the results to the output sequence. Lets you perform pair wise transformation on two sequences.

		std::transform(v.begin(), v.end(), u.begin(), result, binary_operation);

## Part 8: Regular Expressions in C++11

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-8-of-n)_

Regular expressions are declarative string algorithms for flexible string processing. C++ regular expressions have a Perl type expression.

A few examples of regular expression uses include

- Validation: is this input well-formed? (serial numbers)
- Decision: what set does this string belong to? (`.jpg` or `.png`)
- Parsing: what is in this input? (year)
- Transformation: format strings for output or further procession. (escape speciall characters)
- Iteration: find each occurrence of a pattern within a string. (iterate through URLs)
- Tokenisation: systematically split apart a string. (break a string into whitespace-separated words)

To use regular expressions in C++, we must include the `<regex>` header file, which include templated functions. `std::regex` is acutally a typedef for `basic_regex<char>` (regular strings), and `std::wregex` is a typedef for `basic_regex<wchar_t>` (unicode).

	#include <regex>
	...
	const std::regex r("ab+c");

	for(std::string s; getline(std::cin,s); ) {
		std::cout << regex_match(s,r) << std::endl;
	}

There are three main regular expression functions

- `regex_match`: Does the _whole_ regex match the _entire_ string?
	
	- We can construct an object which represents the currently matched component of the string. It is of class `match_results` and is templated on string type.
	- `smatch` is a typedef for results for regular strings
	- `cmatch` is a typedef for results for const strings

		const std::regex r("a(b+)(c+)d");

		for(std::string s; getline(cin,s); ) {
			std::smatch m;
			const bool b = std::regex_match(s,m,r);
			
			std::cout << b << std::endl;

			if(b) {
				std::cout << m[1] << std::endl; // Substring that matches the first capture group
				std::cout << m[2] << std::endl; // Substring that mathces the second capture group
			}
		}

- `regex_search`: Searches for substrings of the large string that match the specified regular expression. Returns the first occurance of the match. Never use `regex_search` to iterate through each occurance of the large string; use `regex_iterator` instead. This is because `regex_search` does not handle zero-length string matches, which then may cause infinite loops.
	
	- `sub_match` contains iterators for the start and end of the match
	- The string can be extracted from the individual matches

		const std::regex r("c+d+e+");

		for(std::string s; getline(cin,s); ) {
			smatch m;
			const bool b = std::regex_search(s,m,r);
			
			std::cout << b << std::endl;

			if(b) {
				std::cout << m[0] << std::endl; // Whole substring that matches
				// m[0].first                   // Iterator to the match
				// m[0].first - s.begin()       // Starting index of match
				// m[0].second - s.begin()      // Ending index of match
			}
		}

- `regex_replace`: Search and replace strings that match the regular expression. The replacements can be controlled at capture group level.

		const std::regex r("c+d+e+");

		const string fmt("[$3]-[$2]-[$1]");

		for(std::string s; getline(cin,s); ) {
			std::cout << std::regex_replace(s,r,fmt) << std::endl;
		}

As mentioned previously, `regex_iterator` may be used to look at all instances of the matching regular expression. Use `sregex_iterator` for regular strings, and `wregex_iterator` for unicode strings.

	const std::regex r("c+d+e+");

	for(std::string s; getline(cin,s); ) {
		for(std::sregex_iterator i(s.begin(), s.end(), r), end; i != end; ++i ) {
			std::cout << (*i)[2]<< std::endl;
		}
	}

`regex_token_iterator`: Use to generate tokens from a string, e.g. if you are only interested in a subset of capture groups. This is the generalised form of `strtok` from the C library.

Note that C++ will parse the regular expression string before passing it onto the regular expression engine. Hence, we must use double-backslashes to escape regular expression backslashes. Furthermore, regular expression tree generation from the string definition of a regular expression is time consuming. This generation should be done once and reused multiple times.

Further information: a `regex` object is a pointer to a "Non-deterministic Finite Automaton".

## Part 9: R-value References in C++11

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-9-of-n)_

L-values are _persistent named objects_. R-values values are _temporaries_: objects which do not have a name (e.g. integer literals, or `x+y`). Both apply to expressions, which can refer to objects; ojects themselves are not inherently l-values or r-values.

A key question to distinguish l-values and r-values are: "Can you take its address?". If you can, it is an l-value. Otherwise, it is an r-value.

An example of an l-value reference is `int& r`, and can be used to bind a reference to a temporary object (when passing an argument by reference).

An r-value reference behaves just like an l-value reference, but with subtle differences. It can bind two r-values specifically, and can never bind l-values.

	const string& cr = s+t; // The temporary will persist as long as cr is in scope
	string&& rr = s+t;      // An r-value reference (not realistic)

There are two functions within the STL, namely `std::move()` and `std::forward<T> ()`. R-value references support two radically different things

- It supports move semantics, and
- perfect forwarding

C++ uses the copy constructor whenever an object needs to be copied. This happens in cases like:
	
- Passing a parameter by value
- Returning an object by value
- Assigning an object

The problem here is that a copy constructor might sometimes result in redundant copies. For example:
	
	myvalue = MethodReturingValue() 

results in a copy constructor creating an object on return from `MethodReturningValue()`, and so another copy constructor is called for the assignment. This occurs in many cases, e.g. when using `std::string`, `std::vector`, or your own classes with copy constructors and `operator=` which dynamically allocates memory.

C++11 defines a move constructor so that this double copy can be avoided.
	
In the above example, the method returns a value with a copy constructor. Now the compiler knows that this is a temporary object so the second time it calls the move constructor. The move constructor does not copy the object, instead it just replaces the contents of the object.
	
A _move constructor_ has a signature of:
		
	ClassA(ClassA&& rvalue)  // Note the absence of const and double ampersand

Compare this to a _copy constructor_:
		
	ClassA(const ClassA& lvalue)

C++11 version of standard template library will use move constructor to reuse a temporary object (`std::string`, `std::vector`). When objects of a class are saved by value in STL containers, defining the move constructor will improve performance as unnecessary copying will be avoided.

`std::move` takes a value or an object, and returns an r-value. 

	ClassA& operator=(ClassA&& other) {
		ClassA(std::move(other)).swap(*this);
		other.privatePointer = nullptr;
		return *this;
	}

We need to do this since `other` -- an r-value reference -- is an l-value (since it has a name).

As a word of warning, only use r-value references within the move semantics and perfect forward idioms, i.e. move constructors and `operator=` move assignments.

We can unify the copy assignment operator and move assignment operator

	ClassA& operator=(ClassA other) {
		other.swap(*this);
		return *this;
	}

## Part 10: Type Traits for Template Meta-Programming

_[Video link](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-10-of-10)_

Type traits enable template meta programming with _template argument deduction_.

The `type_traits` header allows you to check the type of the typename in a template, and define methods that are appropriate for that type. For example,

- `true_type` and `false_type` are typedefs for two separate types (they are equivalent of bool in runtime code). Queries about types return `true_type` or `false_type`.
- `is_integral`: is the type an integer? (`int`, `long long`, etc)
- `is_floatingpoint`: is it a floating point type?
- `is_pointer`: is it a pointer type?

`true_type` and `false_type` can be used to define different overloads for a method. This way you can control the overload that gets called for different types.

	#include <type_traits>

	template <typename T> void foo(T t, std::true_type) {
		std::cout << t << " integral!" << std::endl;
	}

	template <typename T> void foo(T t, std::false_type) {
		std:;cout << t << " floating!" << std::endl;
	}

	template <typename T> void var(T t) {
		foo(t, typename std::is_integral<T>::type());
	}

	int main() {
		bar(1729);
		var(3.14);
	}

## Resources

- [C++ Standard Template Library - Stephan T. Lavavej](http://channel9.msdn.com/Series/C9-Lectures-Stephan-T-Lavavej-Standard-Template-Library-STL-)
- [EventHelix](http://www.eventhelix.com/realtimemantra/object_oriented/stl-tutorial.htm) for video summaries