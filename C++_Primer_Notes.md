###### Some Notations in the book

1. Must-read =><img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-24 at 00.39.23.png" alt="Screenshot 2022-03-24 at 00.39.23" style="zoom:50%;" />

2. Skim =><img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-24 at 00.39.49.png" alt="Screenshot 2022-03-24 at 00.39.49" style="zoom:50%;" />

3. Tricky =><img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-24 at 00.40.05.png" alt="Screenshot 2022-03-24 at 00.40.05" style="zoom:50%;" />

4. Important terms are indicated in **bold**; important terms that we assume are already familiar to the reader are indicated in ***bold italics\***

864 pages to read in total, estimate time 20 days.



##### Part 0. Intro

###### Ch1. Getting Started

1. Function `return 0` means success, otherwise means fail 

   use `echo $?` to check in `Linux Shell`

2. To compile file => `g++ -std=c++11 -o a.out main.cpp` then `./a.out` (Exercise 1.1)

3. `iostream` library => `cin` = standard input, `cout` = standard output, `cerr` = standard error, `clog`

4. `endl` => flush butter => ensure buffer goes to output stream

5. All the names defined by the standard library are in the `std` namespace.

6. Good comment style

   ```c++
   /*
    * Simple main function:
    * Read two numbers and write their sum 
    */
   ```

7. Reading an Unknown Number of Inputs => `while`

   An `istream` becomes invalid when we hit **end-of-file** (control-d) or encounter an invalid input

   ```c++
   // read until end-of-file, 
   while (std::cin >> value)
   ```

8. Common compile error => `syntax error`, `type error`, `declaration error`



##### Part 1. Basics

###### Ch2. Variables & Basic Types

1. Rule to choose data type: 

   • use `unsigned` as much as possible

   • do not overuse `char`(or specify `unsigned` or `signed`)

   • usually use `int`, `double`

2. Type conversion (happens automatically => compiler does the work)

   ```c++
   bool b = 42; // b is true, result is false if the value is 0 and true otherwise.
   int i = b; // i has value 1, resulting value is 1 if the bool is true and 0 if the bool is false.
   i = 3.14; // i has value 3
   double pi = i; // pi has value 3.0
   unsigned char c = -1; // assuming 8-bit chars, c has value 255
   signed char c2 = 256; // assuming 8-bit chars, the value of c2 is undefined
   ```

3. Avoid `undefined` and `implementation defined` (`nonportable`) behaviour

4. Don't mix `unsigned` with `signed`

   Regardless of whether one or both operands are unsigned, if we subtract a value from an unsigned, we must be sure that the result cannot be negative:

   ```c++
   unsigned u = 10;
   int i = -42;
   std::cout << u + i << std::endl; // if 32-bit ints, prints 4294967264
   ```

5. The compiler appends a null character `\0` to every string literal. Thus, the actual size of a string literal is one more than its apparent size.

6. separated only by spaces, tabs, or newlines are concatenated into one

    i.e. `"A" "B" is just "AB"`

7. Escape sequence: Note that if a `\` is followed by more than three octal digits, **only** the first three are associated with the `\`

   ```c++
   newline \n	horizontaltab \t	alert(bell) \a
   verticaltab  \v	backspace \b	doublequote \"
   backslash \\	question mark \?	single quote \’
   carriage return \r   formfeed \f
   ```

8. How to specify the Type of a Literal => pp.40

9. `list initialization` => `int x{0};`

   When used with variables of built-in type, this form of initialization has one important property: The compiler will not let us list initialize variables of built-in type if the initializer might lead to the loss of information

10. Separate compilation => A **declaration** makes a name known to the program. A file that wants to use a name defined elsewhere includes a declaration for that name. A **definition** creates the associated entity.

    ```c++
    extern int i; // declares but does not define i 
    int j; // declares and defines j
    // It is an error to provide an initializer on an extern inside a function.
    ```

11. C++ Keywords => pp.47

12. Scope (The global scope has no name)

    ```c++
    int reused = 42; // reused has global scope
    int main() {
    	int unique = 0; // unique has block scope 
      // output#1: uses global reused; prints 42 0
    	std::cout << reused << " " << unique << std::endl; 
      int reused=0;
      // new,local object named reused hides global reused
    	// output#2: uses local reused; prints 0 0
    	std::cout << reused << " " << unique << std::endl;
    	// output#3: explicitly requests the global reused; prints 42 0 
      std::cout << ::reused << " " << unique << std::endl;
    return 0; }
    ```

13. Because there is no way to rebind a reference, references *must* be initialized.

    Reference may be bound only to an object, not to a literal or to the result of a more general expression

14. Reference VS Pointer: reference can not be changed cuz it is not object

15. The result of accessing an invalid pointer is undefined.

16. `nullptr`

    ```c++
    int*p1=nullptr;// equivalent to int *p1 = 0;
    int*p2=0; // directly initializes p2 from the literal constant 0 
    // must #include cstdlib
    int *p3 = NULL; // equivalent to int *p3 = 0;
    ```

17. Generally, we use a void* pointer to deal with memory as memory, rather than using the pointer to access the object stored in that memory.

18. Base type and type modification:

    ```c++
    int i = 1024, *p = &i, &r = I;
    
    int* p1, p2; // p1 *is a pointer to* int*;* p2 *is an* int
    ```

19. How `const` is working?

    The compiler will usually replace uses of the variable with its corresponding value during compilation. That is, the compiler will generate code using the value 512 in the places that our code uses bufSize.

    When we define a const with the same name in multiple files, it is as if we had written definitions for separate variables in each file.

     To share a const object among multiple files, you must define the variable as `extern`.

20. A Reference to const May Refer to an Object That Is Not const

21. Const pointer => read from right to left

    ```c++
    const double *const pip = &pi;
    ```

22. For low-level const, we can convert a `nonconst` to `const` but not the other way round

    ```c++
    const int *const p3 = p2;
    int *p = p3;// error:p3 has a low-level const but p doesn’t
    ```

23. Variables declared as constexpr are implicitly const and must be initialised by constant expressions

    ```c++
    constexpr int mf = 20; // 20 is a constant expression
    constexpr int limit = mf + 1; // mf + 1 is a constant expression 
    constexpr int sz = size(); // ok only if size is a constexpr function
    ```

24. It is important to understand that when we define a pointer in a constexpr declaration, the constexpr specifier applies to the pointer, not the type to which the pointer points

    ```c++
    const int *p = nullptr; // p is a pointer to a const int 
    constexpr int *q = nullptr; // q is a const pointer to int
    ```

25. `typedef` => `using`

    ```c++
    typedef int alis;
    using alis = int;
    ```

26. Because a declaration can involve only a single base type, the initializers for all the variables in the declaration must have types that are consistent with each other:

    ```c++
    auto i = 0, *p = &i; // ok: i is int and p is a pointer to
    int auto sz = 0, pi = 3.14; // error: inconsistent types for sz and pi
    ```

    1. When we use a reference as an initializer, the initializer is the corresponding object

    2. `auto` ordinarily ignores top-level consts (§ 2.4.3, p. 63). As usual in initializations, low-level `consts`, such as when an initializer is a pointer to `const`, are kept

    ```c++
    const int ci = i, &cr = ci;
    auto b = ci; // b is an int (top-level const in ci is dropped)
    auto c = cr; // c is an int (cr is an alias for ci whose const is top-level) 
    auto d = &i; // d is an int*(& of an int object is int*)
    auto e = &ci; // e is const int* (& of a const object is low-level const)
    const auto f = ci; // deduced type of ci is int;f has type const int
    ```

27. The way decltype handles top-level const and references **differs** subtly from the way auto does, including top-level const and references

    Another important difference between decltype and auto is that the deduction done by decltype *depends on the form of its given expression*.

    ```c++
    decltype ((i)) d; // error: d is int& and must be initialized 
    decltype (i) e; // ok:e is an (uninitialized) int
    ```

28.  Whenever a header is updated, the source files that use that header must be recompiled to get the new or changed declarations

29. Preprocessor variable names **do not** respect C++ scoping rules. To avoid name clashes with other entities in our programs, preprocessor variables usually are written in all uppercase

    

###### Ch3. String, Vector, Array

1. ```c++
   using std::cin;
   using std::cout; 
   using std::endl;
   ```

2. NEVER `using` in header files

3. ```c++
   string s5 = "hiya"; // copy initialization
   string s6("hiya"); // direct initialization
   ```

4. String functions

   ```c++
   os << s	// Writes s onto output stream os. Returns os
   is >> s // Reads whitespace-separated string from is into s. Returns is
   getline(is, s) // Reads a line of input from is into s. Returns is
   s.empty() // Returns true if s is empty; otherwise returns false
   s.size() // Returns the number of characters in s
   s[n] // Returns a reference to the char at position n in s; positions start at 0
   s1+s2 // Returns a string that is the concatenation of s1 and s2
   s1=s2 // Replaces characters in s1 with a copy of s2
   s1==s2 // The strings s1 and s2 are equal if they contain the same characters. Equality is case-sensitive
   s1!=s2 <,<=,>,>= // Comparisons are case-sensitive and use dictionary ordering
   ```

5. The string input operator reads and discards any leading whitespace (e.g., spaces, newlines, tabs). It then reads characters until the next whitespace character is encountered

6.  The newline that causes getline to return is discarded; the newline is *not* stored in the string

7. Because `size` returns an unsigned type, it is essential to remember that expres- sions that mix signed and unsigned data can have surprising results

8. can’t add string literals 

   ```c++
   string s5 = "hello" + ","; // error:no string operand
   ```

9. In addition to facilities defined specifically for C++, the C++ library incorporates the C library. Headers in C have names of the form `name.h`. The C++ versions of these headers are named `cname` —they remove the `.h` suffix and precede the `name` with the letter `c`. The  `c` indicates that the header is part of the C library => In particular, the names defined in the `cname` headers are defined inside the `std` namespace

10. <img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-25 at 16.54.31.png" alt="Screenshot 2022-03-25 at 16.54.31" style="zoom:50%;" />

11. The process that the compiler uses to create classes or functions from templates is called **instantiation**

12.  Some compilers may require the old-style declarations for a vector of vectors, for example, `vector<vector<int> >`

13. <img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-25 at 17.24.58.png" alt="Screenshot 2022-03-25 at 17.24.58" style="zoom:50%;" />

14. misuse of `{}` and `()`

    ```c++
    vector<string> v5 {"hi"}; // list initialization: v5 has one element 
    vector<string> v6 ("hi");// error: can’t construct a vector from a string literal 
    vector<string> v7{10}; // v7 has ten default-initialized elements 
    vector<string> v8{10, "hi"}; // v8 has ten elements with value "hi"
    ```

15. To use `size_type`, we must name the type in which it is defined. A vector type *always* includes its element type 

    ```c++
    vector<int>::size_type // ok 
    vector::size_type // error
    ```

16.  If the container is empty, the `iterators` returned by `begin` and `end` are equal—they are both off-the-end iterators

17. `iterator`

    ```c++
    for (auto it = s.begin(); it != s.end(); ++it)
    	*it = toupper(*it); 
    ```

18. A `const_iterator` behaves like a const pointer

    ```c++
    vector<int>::iterator it; // it can read and write vector<int> elements
    string::iterator it2; // it2 can read and write characters in a string
    vector<int>::const_iterator it3; // it3 can read but not write elements 
    string::const_iterator it4; // it4 can read but not write characters
    ```

19. To let us ask specifically for the const_iterator type, the new standard introduced two new functions named `cbegin` and `cend`

    ```c++
    auto it3 = v.cbegin(); // it3 has type vector<int>::const_iterator
    ```

20. The arrow operator `->` combines dereference and member access into a single operation. That is, `it->mem` is a synonym for `(*it).mem`

21. Loops that use iterators should not add elements to the container to which the iterators refer => more in 9.3.6 (pp. 353)

22. `it1 - it2` returns a `difference_type`

23. `array` the dimension must be a constant expression

    ```c++
    unsigned cnt = 42; // not a constant expression 
    constexpr unsigned sz = 42; // constant expression
    											// constexpr see § 2.4.4 (p. 66)
    intarr[10]; // array of ten ints
    int *parr[sz]; // array of 42 pointers to int
    stringbad[cnt]; // error:cnt is not a constant expression
    stringstrs[get_size()];// okifget_sizeisconstexpr,errorotherwise
    ```

24. If we omit the dimension, the compiler infers it from the number of initializers.

    ```c++
    int a2[] = {0, 1, 2}; // an array of dimension 3
    stringa4[3]={"hi","bye"}; // sameasa4[]={"hi","bye",""}
    int a5[2] = {0,1,2}; // error: too many initializers
    ```

25. `char array` are special

    ```c++
    char a1[] = {’C’, ’+’, ’+’}; // list initialization, no null
    char a3[] = "C++"; // null terminator added automatically
    const char a4[6] = "Daniel"; // error: no space for the null!
    ```

26. No assignment! Some compilers allow array assignment as a **compiler extension**

27. it can be easier to read array declarations from the inside out rather than from right to left

    ```c++
    int *ptrs[10]; // ptrs is an array of ten pointers to int 
    int &refs[10] = /* ? */; // error: no arrays of references
    int (*Parray)[10] = &arr; // Parray points to an array of ten ints 
    int (&arrRef)[10] = arr; // arrRef refers to an array of ten ints
    ```

28.  In most expressions, when we use an object of array type, we are really using a pointer to the first element in that array

    ```c++
    string *p2 = nums; // equivalent to p2 = &nums[0]
    ```

29. `array` similar to `vector`

    ```c++
    int ia[] = {0,1,2,3,4,5,6,7,8,9}; // ia is an array of ten ints 
    int *beg = begin(ia); // pointer to the first element in ia
    int *last = end(ia); // pointer one past the last element in ia
    auto n = end(arr) - begin(arr); // n is ptrdiff_t
    ```

30. We can use the subscript operator on any pointer, as long as that pointer points to an element (or one past the last element) in an array

    ```c++
    int *p = &ia[2]; // p points to the element indexed by 2 
    int j = p[1]; // p[1] is equivalent to *(p + 1),
    							// p[1] is the same element as ia[3] 
    int k = p[-2]; // p[-2] is the same element as ia[0]
    ```

31. C strings are SHIT!

32. `string` `char*`  conversion

    ```c++
    char *str = s;// error: can’t initialize a char* from a string 
    string s(str);
    const char *str = s.c_str(); // ok
    ```

    

###### Ch4. Expression

1. lvalue <=> memory, rvalue <=> value

2. Precedence has order, but how they are evaluated are undefined

   ```c++
    int i = f1() * f2(); // order unknown
   int i = 0;
   cout << i << " " << ++i << endl; // undefined
   ```

3. The result of an assignment is its left-hand operand, which is an lvalue

4. Use prefix MORE

   Prefix: increments its operand and yields the *changed* object as its result. 

   Postfix: increment the operand but yield a copy of the original, *unchanged* value as its result

   ```c++
   int i = 0, j;
   j = ++i; // j = 1, i = 1: prefix yields the incremented value
   j = i++; // j = 1, i = 2: postfix yields the unincremented value
   ```

5. Suggestion: use `unsigned` for bitwise operators

6.  An overloaded operator has the same precedence and associativity as the built-in version of that operator

7. `,` comma operator guarantees the order in which its operands are evaluated, the result of a comma expression is the value of its right-hand expression

8. Implicit type conversion => see example and rules on pp.160

9. Explicit type conversion

   ```c++
   cast-name<type> (expression);
   // cast-name being static_cast, dynamic_cast, const_cast, OR reinterpret_cast
   No longer use old style
   type (expr); // function-style cast notation 
   (type) expr; // C-language-style cast notation



###### Ch5. Statement

1. `Dangling else` : In C++ the ambiguity is resolved by specifying that each else is matched with the closest preceding unmatched if

2. `Switch` 

   two case labels same value => error

   `case` wrong type or non-constant => error

   If we need to define and initialize a variable for a particular case, we can do so by defining the variable inside a `block`

3. NEVER `goto`

4. try + throw + catch = error exception

5. `runtime_error err`

   `err.what()` returns a C string (`const char*`)

6. <img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-26 at 23.27.33.png" alt="Screenshot 2022-03-26 at 23.27.33" style="zoom:50%;" />

   

###### Ch6. Function

1. In `function` although we know which argument initializes which parameter, we have no guarantees about the order in which arguments are evaluated

2. Local static

   ```c++
   size_t count_calls(){
   		static size_t ctr = 0; // value will persist across calls
       return ++ctr;
   }
   int main() {
   		for (size_t i = 0; i != 10; ++i)
     			cout << count_calls() << endl;
   		return 0; 
   }
   ```

3. **Separate compilation** => lets us split our programs into several files, each of which can be compiled independently

   ```bash
   g++ -c file1.cpp
   g++ -c file2.cpp
   g++ file1.o file2.o -o main
   ```

4. Function parameter initialization works the same way as variable initialization

5. C use pointer in function => C++ use reference

6. multiple return => use reference

7. Because top-level cost is ignored

   ```c++
   void fcn(const int i) { /* fcn can read but not write to i */ } 
   void fcn(int i) { /* . . . */ } // error: redefines fcn(int)
   ```

8. We can initialize an object with a low-level const from a nonconst object but not vice versa => this rule apply to function argument as well

9. Correct way to pass 2d array => pp. 218

10. Command line arguments

    ```c++
    int main(int argc, char *argv[]) { ... }
    prog -d -o ofile data0 // will get
    argv[0] = "prog"; // or argv[0] might point to an empty string argv[1] = "-d";
    argv[2] = "-o";
    argv[3] = "ofile";
    argv[4] = "data0";
    argv[5] = 0;
    ```

11. How to pass varying number of parametres?

    1 ->`initializer_lists ` (can imagine as a `vector` of `const`)

    <img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-27 at 01.42.07.png" alt="Screenshot 2022-03-27 at 01.42.07" style="zoom:50%;" />

    ```c++
    void error_msg(ErrCode e, initializer_list<string> il) {
    cout << e.msg() << ": "; for (const auto &elem : il)
                cout << elem << " " ;
            cout << endl;
    }
    ```

    2 -> `ellipsis` => allow programs to interface to C code that uses a C library facility named `varargs`

    ```c++
    void foo(parm_list, ...);
    void foo(...);
    ```

12. NEVER return a reference/pointer to a local object

13. `error_msg` example in 11 should be using `vector<string>` in `c++11`

14. `main` don't need a `return` (compiler will add one for us), defined in `cstdlib` we have `returnEXIT_FAILURE` and `returnEXIT_SUCCESS`

15. Return type complicated => Make life easier => using `trailing return type` OR `decltype`

    ```c++
    auto func(int i) -> int(*)[10];
    decltype(xxx) *fun();
    // instead of 
    int (*func(int))[10];
    ```

16. Overloaded functions must differ in the number or the type(s) of their parameters

17. It is an error for two functions to differ only in terms of their return types

    See 7 for `const`

    ```c++
    Record lookup(const Account&);
    bool lookup(const Account&); // error: only the return type is different
    // each pair declares the same function
    Record lookup(const Account &acct);
    Record lookup(const Account&); // parameter names are ignored
    typedef Phone Telno;
    Record lookup(const Phone&);
    Record lookup(const Telno&); // Telno and Phone are the same type
    Record lookup(Phone);
    Recordlookup(constPhone); // redeclaresRecordlookup(Phone)
    Record lookup(Phone*);
    Recordlookup(Phone*const);// redeclaresRecordlookup(Phone*)
    // BUT
    // functions taking const and nonconst references or pointers have different parameters 
    // declarations for four independent, overloaded functions
    Record lookup(Account&); // function that takes a reference to Account Recordlookup(constAccount&);// newfunctionthattakesaconstreference
    Record lookup(Account*); // new function, takes a pointer to Account Record lookup(const Account*); // new function, takes a pointer to const
    ```

18. The compiler determines which function to call by comparing the arguments in the call with the parameters offered by each function in the overload set

19. Ordinarily, it is a bad idea to declare a function locally

    ```c++
    string read();
    void print(const string &);
    void print(double); // overloads the print function
    void fooBar(int ival)
    {
    	bool read = false; // new scope: hides the outer declaration of read 
    	string s = read();// error: read is a bool variable, not a function
    	// bad practice: usually it’s a bad idea to declare functions at local scope
      void print(int); // new scope: hides previous instances of print
      print("Value:"); //  error: print(const string &) is hidden
      print(ival); // ok: print(int) is visible
      print(3.14); // ok: calls print(int); print(double) is hidden
    } 
    ```

    In C++, `name lookup` happens before `type checking` !!!

20. `default values` => NOTE: if a parameter has a default argument, all the parameters that follow it must also have default arguments (and can omit only trailing (right-most) arguments)

    ```c++
    string screen(sz ht = 24, sz wid = 80, char backgrnd = ’ ’);
    ```

21. ```c++
    string screen(sz, sz, char = ’ ’);
    string screen(sz, sz, char = ’*’); // error: redeclaration
    string screen(sz = 24, sz = 80, char); // ok: adds default arguments
    ```

22. Names used as default arguments are resolved in the scope of the function declaration

    ```c++
    sz wd = 80;
    char def = ’ ’;
    sz ht();
    string screen(sz = ht(), sz = wd, char = def); 
    stringwindow = screen();// calls screen(ht(),80,’’)
    
    void f2() {
    def = ’*’; // changes the value of a default argument
    sz wd = 100; // hides the outer definition of wd but does not change the default
    window = screen();// callsscreen(ht(),80,’*’) }
    ```

23. `inline` => send a request to compiler (might be ignored) to expand a function in **compilation** time

    Many compilers will not inline a recursive function

    A 75-line function will almost surely not be expanded inline

24. `constexpr function` (need to review pp. 239) => The return type and the type of each parameter in a must be a literal type, and the function body must contain exactly one return statement 

25. `assert` (runtime check), use `NDEBUG` to disable it

    ```bash
    in cpp file use #define NDEBUG or in command line
    g++ -D NDEBUG main.cpp
    ```

    Programs that include the `cassert` header may not define a variable, function, or other entity named assert

26. The compiler defines \_\_func\_\_ in every function. It is a local static array of const char that holds the name of the function

    ```bash
    __FILE__string literal containing the name of the file 
    __LINE__integer literal containing the current line number
    __TIME__string literal containing the time the file was compiled 
    __DATE__string literal containing the date the file was compiled
    ```

27. How compile decide which function when it comes to `override`? => pp. 244

28. When we use the name of a function as a value, the function is **automatically** converted to a pointer, but the return type does not do so



###### Ch7. Class

1. `abstract data type` = `data abstraction` + `encapsulation`

2. Functions defined in the class are implicitly inline (otherwise declare only)

3. Any direct use of a member of the class is assumed to be an implicit reference through `this` (a const pointer)

4.  The compiler generates a default constructor automatically only if a class declares *no* constructors

5. We want this constructor to do exactly the same work as the synthesized version we had been using.

   ```c++
   class_name = default;
   ```

6. Constructor initialiser list

   ```c++
   Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(p*n) { }
   ```

7. classes that manage dynamic memory, generally cannot rely on the synthesized versions of copy, assignment, and destruction (can avoid using `stl`)

8. Access specifiers: `public`, `private` (only difference between `class` and `struct`)

9. `Type Member` (often appear at top)

   ```c++
   public:
   typedef std::string::size_type pos;
   ```

10. Modify data member even inside a `const` member function => mutable

11. Two different classes define two different types even if they define the same members

    ```c++
    struct First {
            int memi;
            int getMem();
        };
        struct Second {
            int memi;
            int getMem();
        };
    First obj1;
    Secondobj2 = obj1;// error:obj1 and obj2 have different types
    ```

12. after declaration before definition => incomplete type ( forward declaration)

13. `friend` to whole class (friend need to be reviewed => pp.282)

    ```c++
    class Screen {
    		// Window_mgr members can access the private parts of class Screen 
    	  friend class Window_mgr;
    		// ...rest of the Screen class
    };
    ```

    `friend` to only one member function

    ```c++
    class Screen {
    		// Window_mgr::clear must have been declared before class Screen 
      	friend void Window_mgr::clear(ScreenIndex);
    		// ...rest of the Screen class
    };
    ```

    (Note: a class must declare as a friend each function in a set of overloaded functions that it wishes to make a friend)

14. Order:  Member function definitions are processed *after* the compiler processes all of the declarations in the class

15. Order of scope first find in member function => then in class (to specify `this->var`) => outside of class `(::var)`

16. Use `constructor initialiser` instead of assignment =>  We *must* use the constructor initializer list to provide values for members that are const, reference, or of a class type that does not have a default constructor

17. In `constructor initialiser` The first member is initialized first, then the next, and so on => It is a good idea to write constructor initializers in the same order as the members are declared => avoid using mem- bers to initialize other members

18. **delegating constructors** => similar to what I used in Swift (use one constructor for another constructor)

    ```c++
    class Sales_data {
    public:
    		// nondelegating constructor initializes members from corresponding arguments 
      	Sales_data(std::string s, unsigned cnt, double price): bookNo(s), 			
      			units_sold(cnt), revenue(cnt*price) { } 
      	// remaining constructors all delegate to another constructor
    		Sales_data(): Sales_data("", 0, 0) {} 
      	Sales_data(std::string s): Sales_data(s, 0,0) {} 
      	Sales_data(std::istream &is): Sales_data() { read(is, *this); }
    		// other members as before }
    ;
    ```

19. In constructor, AT MOST 1 class-type conversion is allowed

20. We can prevent the use of a constructor in a context that requires an implicit con- version by declaring the constructor as **explicit** (only need for constructors with 1 argument) (only used for copy form of constructor, with `=`)

21. **aggregate class** => four conditions pp.298 => BAD

22. An `aggregate class` (§ 7.5.5, p. 298) whose data members are all of literal type is a `literal class`

23. `static` member => similar to static in function => applies to all class objects

    For function, The keyword appears only with the declaration inside the class body

24.  The best way to ensure that the object is defined exactly once is to put the definition of static data members in the same file that contains the definitions of the class non-inline member functions.

25. Trick: `static` member can have the same type as the class type of which it is a member

26. Another difference between static and ordinary members is that we can use a static member as a default argument

    

##### Part2. C++ Library

###### Ch8. IO Library

1. how we can write our own input and output operators => Ch14

2. how to perform random access on files => Ch17

3. **wide characters** => use `w` , e.g. `wcin`, `wcout`, `wcerr`, `wifstream`

4. `ifstream` and `istringstream` inherit from `istream`

5. we cannot copy or assign objects of the IO types:

   ```c++
   ofstream out1, out2;
   out1 = out2; // error: cannot assign stream objects 
   ofstream print(ofstream);// error: can’t initialize the ofstream parameter 
   out2 = print(out2); // error: cannot copy stream objects
   ```

   => Functions that do IO typically pass and return the stream through `const references`

6. `machine-dependent` => `iostate`

   `badbit` = system-level failure, such as an unrecoverable read or write error

    `failbit` = recoverable error, such as reading a character when numeric data was expected

   Reaching end-of-file sets => `eofbit` + `failbit`

   `goodbit` => guaranteed to have the value 0, indicates no failures on the stream

7. <img src="/Users/stevenwong/Library/Application Support/typora-user-images/Screenshot 2022-03-28 at 13.19.20.png" alt="Screenshot 2022-03-28 at 13.19.20" style="zoom:50%;" />

8. the code that is executed when we use a stream as a condition is equivalent to calling `!fail()`

9. First `good` or `fail` => if `fail` `eof` or `bad`

10. Why Buffer? => Using a buffer allows the operating system to combine several output operations from our program into a single system-level write. Because writing to a device can be time-consuming

    Output buffers are not flushed if the program terminates abnormally

11. When buffer happens? => 5 conditions => see pp.314

    1. `endl` (manipulator)

       `flush` flushes the stream but adds no characters to the output

        `ends` inserts a null character into the buffer and then flushes it

    2. `cout << unitbuf;` => all writes will be flushed immediately

    3. When buffer is full

    4. End of main

    5. 

    









###### Ch9. Sequential Containers



###### Ch10. Generic Algorithms



###### Ch11. Associative Containers



###### Ch12. Dynamic Memory



##### Part3. Tools for Class Authors

###### Ch13. Copy Control



###### Ch14. Overloaded Operation & Conversions



###### Ch15. OOP



###### Ch16. Templates & Generic Programming



##### Part4. 

###### Ch17. Specialized Library Facilities



###### Ch18. Tools for Large Programs



###### Ch19. Specialized Tools & Techniques



