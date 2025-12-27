# Chapter 2: Containers, iterators, and ranges 

book published february 2024

livebook updated March 2024

---

### This chapter covers

- Filling and using containers, with a focus on a vector of numbers
- Range-based for loops and auto
- Using a container with standard algorithms
- Using format to display output
- Ranges, views, and lambdas

---

Containers and algorithms have been a fundamental part of C++ for a long time. Containers have included sequences (e.g., `vector`), associative containers (e.g., `map`), and, since C++ 11, unordered associative containers (e.g., `unordered_map`). The containers manage the storage for their elements. The separation of data structures and algorithms offers great flexibility, allowing one algorithm to be applied to various containers. The addition of ranges to the core language provides simplified ways to access and manipulate containers. To explore these features, in this chapter, we are going to construct Pascal’s triangle, which is made by adding up adjacent numbers from the preceding row, starting with a single 1 in the first row. The entries can be used to count the number of event combinations and more. We will use vectors to store the values, starting with the first row, to practice using a vector and writing out to the screen. We’ll then generate and display more rows, learning how to use vectors differently. Finally, we’ll discuss some of the triangle’s properties. This will help us think about testing our code later.

You’ll need a compiler and editor or an IDE if you want to code along. A list of free resources is available at [https://isocpp.org/get-started](https://isocpp.org/get-started). I’m using a mixture of Vim with GNU Compiler Collection (GCC) in the Windows Subsystem for Linux (WSL) and Visual Studio 2022 community edition, with /std:c++latest in the C++ command line properties.

---

## 2.1 Creating and displaying a vector

First, we will create a vector containing a single number and display it. This will be the first row of the triangle. Vectors are the most commonly used containers, so starting with them is handy. We can then practice putting different elements in vectors, including other vectors, and using them with algorithms. We will also employ several other C++ features as we code.

It’s a good idea to keep your code outside the `main` entry point function so you can add tests easily or build a library to make reusing the code straightforward. That said, we’ll put everything in one file, called `main.cpp`, to keep things simple, and we’ll make a function that we’ll call from `main`. We start by making a vector containing a number and displaying the contents as follows.

#### Listing 2.1 Filling and displaying a container

```cpp
#include <iostream>
#include <vector>
 
void generate_triangle()
{
    std::vector<int> data{ 1 };
    for (auto number : data)
    {
        std::cout << number << ' ';
    }
    std::cout << '\n';
}
 
int main()
{
    generate_triangle();
}
```

If you’re playing along, compile and run your code. For the GCC tools, compile like this:

```sh
g++ -Wall --std=c++2a -o main.out main.cpp
```

We don’t need to say which `std` we are using here, as long as the compiler supports at least C++11, and we are checking for any warnings, with `all` to the warning flag `-W`. The `-o` flag names our output, which we can run by typing `./main.out` once it has built the single `main.cpp` file. If you are using an IDE, find your Build button, and then find the Run button. You should get a single digit on your screen.

The code contains a few newer C++ features. At the top, we’ve included two headers: `iostream` for input and output streams and `vector`. This should be familiar. We then have a function to generate and display the first line of our triangle using a `vector` for storage. The `vector` is initialized with the single number 1:

```cpp
std::vector<int> data{ 1 };
```

Notice we’re using curly braces, called the uniform initialization syntax. If we say

```cpp
std::vector<int> data( 1 );
```

instead, we get a vector with one value, which is 0. A vector has various constructors. The second version using `(1)` treats the number 1 as a count of elements. The first version, using curly braces `{1}`, is using an *initializer list.* The list can have more than one item, and the vector is created with the contents of the initializer list. Trying to use an initializer list of `{1, 2.3}` would generate a compiler error. This requires a *narrowing conversion* because `2.3` is a `double`, and we want a `vector` of `int`. We can even use `{}` to initialize a single number: `int x{ 42 }`. As the brace initialization can be used in many places, it is called *uniform initialization*. ISOCpp suggests preferring brace initialization ([http://mng.bz/n1m5](http://mng.bz/n1m5)) because it avoids narrowing and allows consistency. Initialization is a big topic and can get complicated. For example, Nicolai Josuttis has talked about “The Nightmare of Initialization in C++” ([https://www.youtube.com/watch?v=7DTlWPgX6zs](https://www.youtube.com/watch?v=7DTlWPgX6zs)). The important thing to note here is that we can use an initializer list to construct a vector.

Armed with our container of data, we can show its contents on the standard output stream (cout). We employ a *range-based* for *loop* to walk over the container, using the insertion operator << to stream out elements from the container. Generally, a range-based for loop has the for, parentheses, and a colon, as we saw in listing 2.1:

```cpp
for (auto number : data)
```

This is a more succinct syntax than traditional `for` loops. On the left side of the colon, there are a type and a variable name. We can be lazy and get the compiler to figure out the type by using `auto`. To the right of the colon, there is a container, array, or similar. We can think of the range-based `for` loop as syntactic sugar to make our lives easier. We do not need to spell out the stopping conditions or how to step through the items. The range-based `for` loop does this for us.

If we try the code out in C++ Insights ([https://cppinsights.io/](https://cppinsights.io/)), we see the loop transformed into a traditional C-style `for` loop with three parts: a beginning, a stopping condition, and an increment. Every container has a beginning and an end, and the range-based `for` loop uses these to walk through the elements. C++ Insights shows all the gory details but gives code equivalent to

```cpp
for (auto position = data.begin(); position!=data.end(); ++position)
```

With the C-style `for` loop, we use a position that is an *iterator* into the vector, which we need to *dereference*, using operator \**,* when we want to print the value:

```cpp
std::cout << *position << ' ';
```

The range-based `for` loop is much easier to use and means we can code at a higher level without having to think about iterators.

Let’s look at `auto` in more detail. The keyword tells the compiler to deduce the type. If you’re using an IDE, a mouse hover over the word `auto` might tell you the number’s deduced type. Visual Studio says it’s using `std::vector<int>>::iterator::value_ type`, which is `int`. Typing `int` instead of `auto` makes little apparent difference in our case, but there are advantages to almost always using auto (AAA). This phrase was coined by Herb Sutter on his Guru of the Week blog ([http://mng.bz/vPpp](http://mng.bz/vPpp)). In more complicated cases, `auto` will save a lot of typing, while tending to keep the code type safe. If we change the container’s type to use `double` instead, we do not need to change the loop as well, so the code is less brittle when we use `auto`. Using `auto` can also pick up subtleties that can be easily missed. If we make the data constant

```cpp
const std::vector<int> data{ 1 };
```

the loop variable’s type automatically changes to a `const_iterator`, so we do not need to remember to make the change there. In fact, we can even declare our container using `auto`:

```cpp
auto data = std::vector<int>{ 1 };
```

Because `data` is a vector of `int` constructed with an initializer list containing an `int`, it is deduced to be `vector<int>`.

More significantly, `auto` can help us avoid implicit conversions, including narrowing conversions, and force us to initialize our variables. We can say `auto variable = init`, or if we want a specific type, we can say `auto variable = type{init}`. In both cases, we are forced to spell out how to initialize the variable. We cannot say `auto variable;` because we will get a compiler error. If we try something like `auto x = int{ 1.5 }`, we will also get a compiler error because we are trying to use a narrowing conversion. If we say `int x = 1.5` instead, we might get a warning, but some people ignore warnings. Not a good idea, but it happens. Using `auto` would stop the potential error.

Back to our vector. We can make one more small change to how we create our vector. We told the compiler to put an integer in the vector, so surely it can figure out the type of the elements in the vector. Yes, it can now. Since C++17, we can simply say `std:: vector data{ 1 }`. Notice we haven’t specified the template type. Instead, we are relying on *class template argument deduction* (CTAD). If we decided to use `auto` (almost) everywhere, we could even change our declaration to `auto data = std::vector{ 1 }`. Now, if we want an empty vector, the type cannot be deduced because `auto data = std:: vector{}` does not have a way to deduce the type of the elements, so it fails to compile. CTAD is another new feature that saves us some typing.

We can now display the first row of Pascal’s triangle. This may seem like a small step, but we have seen a handful of C++ features and can build on this. Next, we will add more rows to the triangle and learn more C++ along the way.

---

## 2.2 Creating and displaying Pascal’s triangle

We now have the first row and will use it to make the next few rows, displaying what we get. We will use C++20’s range library to print out the results. Ranges are one of the bigger features introduced in C++20 that go beyond shorter syntax. Once we have several rows, we will then think about some properties of Pascal’s triangle, which will help us test our code and practice using our vectors. Let’s start with a recap of how to build Pascal’s triangle.

### 2.2.1 A reminder of Pascal’s triangle

Pascal’s triangle contains several useful number sequences. One common use is to find the number of ways events can combine. If you toss a coin once, you can get either heads or tails. If you toss it twice, you can get heads twice, tails twice, or one of each, in two ways: heads and then tails or tails and then heads. For three tosses, you might get all heads, two heads, one heads, or none, but how many combinations are there for a given number of heads? Pascal’s triangle will tell us.

The triangle starts with a number 1, by definition. If we are looking for the number of possible combinations when tossing a coin, there is one result for zero coin tosses. Each subsequent row then starts and ends with 1, by definition. This corresponds to the combination of events. For one coin toss, we can get a single heads in one way or a single tails, again in one way. The second row is therefore two 1s. For the third row, again, we start and end with a 1 because we can get all heads in one way or all tails in one way. The next number is the sum of the two numbers in the preceding row, laying out the rows as shown in figure 2.1.

<br/>

![[figure-2-1.png]]

> Figure 2.1 The figure shows the first few rows of Pascal’s triangle.

<br/>

To generate the fourth row, we start with a 1, then sum the first two numbers from the last row, getting 1 + 2 = 3; next, we sum the second and the third number, getting 2 + 1 = 3. We have used up the previous row, so we add a final 1. For three coin tosses, the fourth row is telling us how many combinations we can have: 1, 3, 3, 1. In other words, there is one way to get all heads; three ways to get two heads (HHT, HTH, THH); three ways to get one heads (HTT, THT, TTH); and finally, one way to get no heads (TTT).

We continue with the next row, starting with 1, adding adjacent pairs in the previous row, and ending with a final 1. We could do this forever on paper, but code is a different matter. An integer will have a maximum value, which will vary between machines and compilers. If we include the numeric header, we can find out what a platform gives by calling `std::numeric_limits<int>::max()`. I get 2,147,483,647, which is 1 less than 2 to the power of 31, 2 <sup>31</sup> - 1. That’s plenty of space for a few rows.

### 2.2.2 Coding Pascal’s triangle

There are several equivalent ways to generate the triangle; however, let’s write code based on the definition we just looked at. We saw how to build a new row using numbers from the previous row, so let’s build a function taking the last row and returning the next row. In the last section, we sent data straight to the screen, but that made code difficult to test, so it makes sense to return the data instead and write a separate display function. Single-responsibility functions are sensible after all. We made a `vector` of integers for the first row, so we will continue using a `vector<int>` for each row. The next listing shows our function, adding a 1 at the start and end of the next row and doing some adding in between.

#### Listing 2.2 The next row of Pascal’s triangle using the previous row

```cpp
std::vector<int> get_next_row(const std::vector<int> & last_row)
{
    std::vector next_row{ 1 };
    if (last_row.empty())
    {
        return next_row;
    }
    for (size_t idx = 0; idx+1 < last_row.size(); ++idx)
    {
        next_row.emplace_back(last_row[idx] + last_row[idx + 1]);
    }
    next_row.emplace_back(1);
    return next_row;
}
```

We initialized our first row using curly braces because there was a specific value. Now we want to calculate values and add them to a vector. There is more than one way to do this. To add to the end of the vector, we can use push\_back or emplace\_back. To add items inside elsewhere, we can use insert or emplace. The emplace versions send in data to create the item in place, while push\_back or insert take a fully formed item that they copy, as illustrated in figure 2.2.

<br/>

![[figure-2-2.png]]

> Figure 2.2 emplace (left) takes parameters to construct an item in place, while push\_back takes a fully constructed item copied into the vector.

<br/>

For our integers, both methods amount to the same. Sometimes, the `emplace` version is quicker because it constructs the element directly in place in the vector; however, `push_back` can be safer sometimes. The `emplace` version will find a constructor for us, which might not be what we would have chosen ourselves. Jason Turner discusses the pros and cons on C++ Weekly ([https://www.youtube.com/watch?v=jKS9dSHkAZY](https://www.youtube.com/watch?v=jKS9dSHkAZY)). The bottom line is that we are likely to see both being used.

We can now calculate the values in each row, but we need to store them somewhere. A vector is a sensible choice, giving us an `std::vector<std::vector<int>>`. That’s a mouthful, and the compiler can figure this out for us. This means we can use `auto` as the return type when we write a function to create the triangle to save typing out `std::vector<std::vector<int>>` in full. For a more complicated function, we might need to help the compiler figure out what type is being returned, but the compiler can cope in this example. How many rows do we want? We can postpone that decision if we accept the required number as a parameter. All we need to do is call our `get_next_row` function to populate the vector we return, starting with an empty data row.

#### Listing 2.3 Generating several rows of Pascal’s triangle

```cpp
auto generate_triangle(int rows)             #1
{
    std::vector<int> data;
    std::vector<std::vector<int>> triangle;
    for (int row = 0; row < rows; ++row)
    {
        data = get_next_row(data);           #2
        triangle.push_back(data);            #3
    }
    return triangle;
}
```

We could stop here, display our triangle, and move on to a new chapter. However, this approach is not particularly efficient. We can do better.

### 2.2.3 Move semantics and perfect forwarding

A vector has many different constructors. We used the version taking an initializer list in listing 2.1 when we made the first row of our triangle:

```cpp
std::vector<int> data{ 1 };
```

To generate the triangle, we _default construct_ each data row as

```cpp
std::vector<int> data;
```

and assign it after the function call:

```cpp
data = get_next_row(data);
```

We then push the data to the back of the triangle:

```cpp
triangle.push_back(data);
```

This is not as efficient as it could be. We create the row data, and a copy is pushed back to the vector of vectors. If we make a couple of small changes, in effect by using a different constructor, we can avoid the copy. Let’s see how to do this using what is referred to as *perfect forwarding*.

We saw earlier that `vector` supports `push_back` and `emplace_back`. The former takes a fully formed item, which we have here, while the latter constructs an object in place. There are two versions of `push_back`. The first takes an item by reference:

```cpp
void push_back( const T& value );
```

That version will be called by our code. It takes our `data` and makes a copy at the end of the triangle. We can avoid that copy if we use the second overload of `push_back`. The signature uses `&&` to indicate an *rvalue reference*:

```cpp
void push_back( T&& value );
```

What is an rvalue reference? Any expression has a *value category*, such as an rvalue or an lvalue. There are other categories too, but we will not go into all of them here. Instead, we will concentrate on avoiding the copy. CppReference gives the full details ([http://mng.bz/468R](http://mng.bz/468R)) in case you want to take a deeper dive.

C uses the idea of lvalues and rvalues. If we say

```
int x = 42;
```

the variable `x` is on the left of an expression and is therefore called an lvalue, whereas `42` is on the right and is called an rvalue. The lvalue has a name, while the rvalue does not. When we call `get_next_row`, we have an rvalue. This is a temporary unnamed vector, which we copied previously to the lvalue `data`. This is wasteful. Rather than keeping a copy of the data, we can use the `back` method to get the last row of the triangle. Thus, we need to initialize the triangle with the first row so that there is an element at the back. We can now write our function as shown in the next listing.

#### Listing 2.4 Moving a temporary

```cpp
auto generate_triangle(int rows)
{
    std::vector<std::vector<int>> triangle{ {1} };
    for (int row = 1; row < rows; ++row)
    {
        triangle.push_back(get_next_row(triangle.back()));
    }
    return triangle;
}
```

We no longer have a copy of data. The `push_back( const T& value )` version initializes a new element with a copy of the value, but the version taking an rvalue reference, `push_back( T&& value )`, can move the temporary into the triangle for us, avoiding the copy. The vector has various constructors, including one taking an rvalue reference, called a *move constructor*. Its signature has the `&&` we saw earlier:

```cpp
vector( vector&& other );
```

The `push_back` method taking `T` `&&` value can utilize this constructor by calling `std::move`, referred to as *move semantics*. The `push_back` `&&` overload can be, and often is, implemented as

```cpp
void push_back( T&& value ) {
   emplace_back(std::move(value));
}
```

Inside the `push_back` method, the rvalue has a name (`value`), so it becomes an lvalue. By calling `std::move(value)`, the value is cast back to an rvalue so that the rvalue constructor is picked. In effect, C++’s `move` operation does not actually move anything. It casts a value to an rvalue. This allows an overload taking an rvalue to be called, referred to as *perfect forwarding*. Once move has been called and an rvalue passed to a function, the value is in a valid but unspecified state. Since it’s been moved, it’s not of much use to us anymore. Without the move, the other vector would be passed as an lvalue, and the copy constructor would be called. This involves unnecessary copies, so it would forward the value imperfectly.

Move semantics and perfect forwarding are big topics, and we have only scratched the surface. Thomas Becker wrote an excellent blog post back in 2013 that walks through the details ([http://mng.bz/QRE6](http://mng.bz/QRE6)). An rvalue reference, &&, might be an lvalue or an rvalue. If it has a name, it is an lvalue, but calling `std::move` casts it to an rvalue, allowing perfect forwarding. In fact, we could call `emplace_back` directly ourselves with the rvalue or temporary:

```cpp
triangle.emplace_back(get_next_row(triangle.back()));
```

How does the move constructor avoid a copy? A vector stores items contiguously, so we can access elements using iterators as well as indexing. We don’t need to know the number of elements at compile time because a vector can resize dynamically. When a vector runs out of space, it allocates more memory. We can think of it as a container pointing to some items, as shown in figure 2.3.

<br/>

![[figure-2-3.png]]

> Figure 2.3 A vector is pointing to its elements.

<br/>

There’s more to a vector than a pointer to its elements, but focusing on this will reveal how the move constructor avoids copying. A copy constructor or assignment will need to copy over each element, so we would have the original vector of, say, four elements, as shown in figure 2.3, along with an identical copy, also pointing at four elements. A move constructor can effectively steal the elements from the rvalue by pointing to the rvalue’s items, rather than allocating copies, as shown in figure 2.4.

<br/>

![[figure-2-4.png]]

> Figure 2.4 A move-constructed vector can steal the rvalue’s elements.

<br/>

Nothing else can try to use the nameless temporary’s elements afterwards, so this is fine. Furthermore, nothing has actually moved, but rather, the move constructor took ownership of the temporary’s data, and no elements needed copying.

We’ve seen two ways to generate the vector. The second version is more efficient because it doesn’t make unnecessary copies. We now need a way to display our triangle.

### 2.2.4 Using ranges to display the vector

Previously, we sent our vector with a single element straight to the screen, but if we write something more general, we can send it to a file or any other stream. We do this by overloading the `operator <<` for our triangle. We have a row, which is a `vector`, which contains a vector of integers. Rather than writing a `for` loop within a `for` loop to write out each element, we can use the `ranges` library to copy the elements to the provided stream. If your compiler doesn’t support `ranges::copy` yet, you can use `std::copy` instead. We can use an output stream iterator (`std::ostream_iterator`) to copy to and indicate we want a space between each number; otherwise, they will be unreadable. Include `<algorithm>` for `std::copy` and the `< iterator>` header for `std::ostream_ iterator`. Then add a new function as indicated in the following listing.

#### Listing 2.5 Sending the contents to a stream

```cpp
#include <algorithm>                                                #1
#include <iterator>
template<typename T>
std::ostream& operator << (std::ostream & s,                        #2
    const std::vector<std::vector<T>>& triangle)
{
    for (const auto& row : triangle)                                #3
    {
        std::ranges::copy(row, std::ostream_iterator<T>(s, " "));   #4
        s << '\n';
    }
    return s;
}
```

Note we are now using a constant reference to each row in the triangle by using `const auto& row` in the `for` loop. This should be familiar. If we used `auto row:v` instead, we would copy the entire contents of the row into `data`. The reference avoids the copy, and `const` means we cannot change the contents. The CPP core guidelines ([http://mng.bz/Xqn9](http://mng.bz/Xqn9)) encourage us not to make expensive copies of a loop variable in a range based `for` loop, as pointed out in the expressions and statements (ES) section, “ES.71: Prefer a range-for-statement to a for-statement when there is a choice.” These guidelines are curated by Bjarne Stroustrup and Herb Sutter, along with many other contributors, and contain lots of sensible advice. You will see more of them from time to time in this book.

The `for` loop gives us a reference to each row. We send this to the stream using a range algorithm. As with the range-based `for` loop we used in listing 2.1, the range’s copy figures out where to start and end from our data vector. A range is conceptually anything that allows iteration by providing a `start` iterator and `end` sentinel. The older algorithms took a `begin` and an `end` of the same type. A sentinel is a recent addition generalizing the idea of the `end` iterator, similar to the idea of using a null character to indicate the end of a `char` array. We could write our own sentinel to stop when a negative number is encountered or any other custom logic. However, our vector has a `begin` and `end`, which is all we need here. We could use the non-range copy algorithm from the same header, but we’d need to specify `begin` and `end` ourselves:

```cpp
std::copy(data.begin(), data.end(), std::ostream_iterator<T>(s, " "));
```

Either version of `copy` is fine, but the range version is a little less wordy. This is one of many range versions of standard algorithms. Ranges provide considerably more than succinct syntax. We can also take views of ranges, allowing composition and filtering without copying data. Views are evaluated on demand; in other words, they support lazy evaluation. We will use a few more ranges later in this chapter.

Now we can call our code to generate the triangle and see what we get. If we ask for a large number of rows, it won’t fit on the screen, and we might overflow our int, so let’s try 16.

#### Listing 2.6 Main code to generate and display the triangle

```cpp
int main()
{
   auto triangle = generate_triangle(16);
   std::cout << triangle;
}
```

The `<<` operator finds our new function and generates a left-justified triangle, as shown in figure 2.5.

<br/>

![[figure-2-5.png]]

> Figure 2.5 The first few rows of Pascal’s triangle

<br/>

---

#### WARNING!

> Defining `operator` `<<` for common types, such as `vector<vector< int>>`, is generally a bad idea because a large system may end up with clashes if two different libraries or components try to do the same thing. It’s okay for your own classes. Writing a named function is better. We’ll do that shortly.

---

If we try generating many rows, say, 36, the last few rows won’t fit on the screen, and we’ll start seeing the integer wrap around and become negative. Printing each row out starting at the left was easy enough, but it gives us an unconventional output. We can do better if we center-justify the output. This also gives us the opportunity to learn about the new `format` library.

### 2.2.5 Using format to display output

When we saw how to generate the triangle, figure 2.1 showed the rows center-justified, which is the conventional way to show the triangle. Sticking with 16 rows gives numbers up to four digits long, so if we center each number in six spaces and add enough spaces at the start of each line, we will have what we want. We can use the `std::format` tools to do this by including the `format` header. `format` started life as the Victor Zverovich’s open source `fmt` library ([https://fmt.dev/latest/index.html](https://fmt.dev/latest/index.html)). Some compilers do not fully support `format` yet, so you may need to use this library instead. There are various ways to install the library, but the simplest is to download from the main page and unzip the download. Instead of including the standard `format` header in the code that follows, use `fmt/core.h;` it is simplest to use the header only:

```cpp
#define FMT_HEADER_ONLY
#include <fmt/core.h>
```

You also need to use `fmt::format` instead of `std::format` in the code, and you need to tell the compiler the additional `include` path, using the `-I` switch:

```cpp
-I/[path_to_unzipped_fmt_download]/include
```

At the time of writing, the open source library contained more features than currently supported in standard C++, but we will stick with commonly supported features here.

---

#### TIP!

> If your compiler doesn’t support `format` yet, you can use the open source `fmt` library ([https://fmt.dev/latest/index.html](https://fmt.dev/latest/index.html)). Alternatively, the `fmt` library includes a link to Godbolt ([https://godbolt.org/z/Eq5763](https://godbolt.org/z/Eq5763)), which includes the `fmt` library so you can try out code in the compiler explorer.

---

The `format` library is similar to C’s `printf` function but is often faster, simpler, and safer to use. The syntax uses curly braces inside a string as placeholders. The placeholder can be empty, take a format specifier (such as d for decimal), or give the index of an argument from the values. If we don’t specify which value to use where via an index, they are placed in order. The format specifiers are very similar to Python’s. If we asked for a number with the d format specifier but passed a string

```cpp
auto does_not_compile = std::format("I am not a number {:d}", "ten");
```

we would get a compiler error, making `format` safer to use than `printf`. For numbers, we might want a plus or minus signs shown, so we can indicate that after the colon by using `{:+d}`. If we don’t specify it, we get the default of a minus sign for negative numbers and no sign otherwise. After the colon, we say if we want decimal `(d)`, binary `(b)`, and so on.

Looking back at figure 2.5, the largest number in the last row is 6435. Because our numbers are therefore no more than four digits long, we can center each element in a block of six, giving at least one space on each side. The specifier for center is `^`, for left is `<`, and for right is `>`, so we format the elements using

```cpp
std::format("{: ^6}", element);
```

Notice the placeholder `{}` with a colon. We aren’t using an index, so put nothing before the colon. We then have " `^6` ", meaning pad with spaces to a length of 6, and center the value. In fact, we could vary the length by adding more curly braces inside the placeholder to send the 6 to, inside the placeholder, like this:

```cpp
std::format("{: ^{}}", element, 6);
```

This gives us a *nested replacement field*. This way, we can calculate how much space each number needs. We will not do that here, but take time to experiment with `format`.

We also need spaces at the start of each row to get a symmetric triangle. If we work out how long the last row is, we can halve its length to determine where to put the 1 from the first row. Let’s think this through for a couple of rows. We noted that the largest number on the last row is 6435, and it has four digits. If we add a space on each side, we need a block of six for each number. The second row will require two blocks of 6, giving twelve characters. To place our first number in the middle, we need three spaces at the start to make the first block sit on the two numbers in the next row. Because we told `format` to center the values, the first one will be in the middle of that block. Figure 2.6 shows this for the first two rows, using `1234` to indicate any four digits.

<br/>

![[figure-2-6.png]]

> Figure 2.6 If we add three spaces at the start of the first row, as indicated by the dashes, we can make the triangle more symmetrical.

<br/>

Calling `back().size()` on the vector of rows tells us how many blocks of six we will use for a final row. To put the first row in the middle, we need three spaces per row that we add; thus, we need padding of three times `back().size()` initially. For each row, we also shrink our padding by three at each step to make the triangle shape.

Pulling together our format and spaces calculation, we can write the following function to display our triangle.

#### Listing 2.7 Center-justified output

```cpp
void show_vectors(std::ostream& s,
    const std::vector<std::vector<int>>& v)
{
    size_t final_row_size = v.back().size();
    std::string spaces(final_row_size * 3, ' ');
    for (const auto& row : v)
    {
        s << spaces;
        if (spaces.size() > 3)
            spaces.resize(spaces.size()-3);
        for (const auto& data : row)
        {
            s << std::format("{: ^{}}", data, 6);
        }
        s << '\n';
    }
}
```

We can then call our new function instead of our previous operator in the main function.

#### Listing 2.8 main function to generate and display the triangle

```cpp
int main()
{
    auto triangle = generate_triangle(16);
    show_vectors(std::cout, triangle);
}
```

This generates and displays our triangle center-justified, as shown in figure 2.7. The output looks about right, but we need to think about how to test our results. We will learn more C++ on the way.

<br/>

![[figure-2-7.png]]

> Figure 2.7 A center-justified triangle
 
<br/>

## 2.3 Properties of the triangle

We have already seen some patterns in the triangle. We know each row starts and ends with a 1, so we can start by adding a check for this property. We will then consider the number of elements we expect in each row, as well as the sum of the elements. Finally, we will see how many rows we can safely generate before the numbers get too big to fit into an integer. We will build these properties into a suite of tests.

Unfortunately, C++ does not come with a testing framework. Rather than spending time setting up and learning such a framework, we will use the `assert` function defined in the `cassert` header. The letter `c` at the start tells us we are pulling in code from the C standard library. `assert` is a macro, so the preprocessor copies the contents verbatim. It will abort our program if the expression in the assertion is false. Some setups only use the `assert` in a debug build by determining whether the `NDEBUG` macro is defined. Without it, the assertions do something, but if `NDEBUG` is defined, they do nothing. Check your setup. The simplest way is to check whether `assert(0)` halts the program.

#### Listing 2.9 Starting with a failing test

```cpp
#include <cassert>
#include <vector>
void check_properties(const std::vector<std::vector<int>> & triangle)
{
    assert(0);
}
 
int main()
{
    check_properties({});
}
```

Using g++ in Ubuntu on the WSL, we see the message Aborted along with a line number, function name, and the message

```
test.out: main_assert.cpp:5: void check_properties(const std::vector<std::vector<int> >&): Assertion \`0' failed.
Aborted
```

If we run this from Visual Studio, we get a dialog box, with details including the line number where the assertion failed. Beginning with a failing test is a good way to start testing code. At the very least, it proves we will get some feedback if an assertion fails. This means we are ready to test our triangle generation.

---

#### NOTE!

> Using `assert` and checking properties from `main` is a pragmatic way to start testing; however, it’s worth taking time to learn a proper unit-testing framework. Several C++ testing frameworks, including Catch2, Google Test, and Boost, are available.

---

Now we have a function to add properties to. Remove the `assert(0)`, and we are ready to add properties to check whether we have the right numbers in our triangle.

### 2.3.1 Checking the first and last elements of each row

We know the first and last numbers in each row must be 1, so we will test that first. We need to add two assertions to our `properties` function to test our expectations, passing in the rows as in the following listing.

#### Listing 2.10 Ensuring the first and last elements are 1

```cpp
#include <cassert>
void check_properties(
    const std::vector<std::vector<int>>& triangle
)
{
    for (const auto & row : triangle)
    {
        assert(row.front() == 1);
        assert(row.back() == 1);
    }
}
```

We can call this from `main` after we have generated the triangle. Our single test was successful, so we are ready to add more. Be warned: because a failing assert calls `abort`, if one thing fails, we will not check any further properties. You can avoid this by stacking up failure messages and asserting the error messages are empty. Try this out, or even better, try to write the tests in a proper framework.

### 2.3.2 Checking the number of elements in each row

Pascal’s triangle has several other properties. The *n* -th row has *n* numbers. Why? We know the first row is a solitary 1. The second row is two 1s. The third starts with 1, then sums the 1s from the previous row to get the number 2, then has another 1 at the end, giving us three numbers. There are four numbers in the fourth row, and this pattern continues. Look back at the triangle in figure 2.5 if you are not convinced. We can add another `assert` to check this if we keep track of the row number.

##### Listing 2.11 Ensuring each row has the expected number of elements

```cpp
size_t row_number = 1;
for (const auto & row : triangle)
{
    assert(row.front() == 1);
    assert(row.back() == 1);
    assert(row.size() == row_number++);
}
```

We should now check the contents. If we check each entry, we need to find another way to generate each number in the row; otherwise, we will duplicate the code we are trying to test. This trap is all too easy to fall into, and trying to think in terms of properties can help us avoid such problems.

### 2.3.3 Checking the sum of the elements in a row

The sum of the numbers in each row also follows a pattern. Table 2.1 demonstrates that these are powers of 2, starting with 0. Remember, anything to the power of 0 is 1. This gives us another property to check.

<br/>

![[table-2-1.png]]

> Table 2.1 The sum of the numbers in each row of the triangle is a power of 2. (view table figure)

<br/>

We need to find the sum for the numbers in each row to check this property. Rather than writing out a `for` loop, we can use the *standard template library* (STL). Herb Sutter and Andrei Alexandrescu suggested preferring algorithm calls to handwritten loops in their book *C++ Coding Standard: 101 Rules, Guidelines and Best Practices* (Addison-Wesley Professional, 2004). The STL also contains many algorithms for use with generic containers, including an `accumulate` method that lives in the `numeric` header, which is exactly what we need. We noted earlier that some algorithms support ranges, but some, including `accumulate`, do not. We, therefore, need to explicitly find `begin` and `end` ourselves.

The `accumulate` function has two versions. They both take a first and last iterator of some range or container, along with an initial value. The first version applies the `operator+` to each element and the current accumulation value, starting with the provided initial value. If we use an initial value of 0, we will obtain the sum of all the elements, which is exactly what we need. The second version allows us to provide our own *binary* operator. That can be any function taking two arguments. The first argument starts with the given initial value, so it must be of the same type, or the initial value must be convertible to the type of this parameter. The second parameter takes the values from the iterator; thus, it also needs to be of a suitable type. CppReference ([http://mng.bz/yZGp](http://mng.bz/yZGp)) gives full details, including the signatures. For the first version, which we will use, we have

```cpp
template< class InputIt, class T >
T accumulate( InputIt first, InputIt last, T init);
```

Notice the initial value, `init`, has type T. So does the return value. If we use an `int`, we will get an `int` back, even for a container of doubles. Our container has `ints`, so we are fine, but we would need to use `0.0` if we were to use doubles. The `accumulate` function is very flexible. The second version takes a binary operator:

```cpp
template< class InputIt, class T, class BinaryOperation >
T accumulate( InputIt first, InputIt last, T init, BinaryOperation op );
```

We could use `operator*` to find the product of all our numbers, provided we start with an initial value of 1. The more general second form is sometimes called a *left fold*. If you want to revise algorithms, looking through what is in the `algorithm` and `numeric` headers is a good starting point.

Now we can include the check for the sum of the rows in our property test function. Starting with an expected total of 1 and doubling each time, we can check that the sum of a row is the power of 2 we expect. Adding the expected total to our properties function and using the accumulate function, along with the `numeric` header, we have the following new check.

#### Listing 2.12 Ensuring each row has the expected sum of elements

```cpp
int expected_total = 1;
for (const auto & row : triangle)
{
    assert(std::accumulate(row.begin(),
                       row.end(),
                       0) == expected_total);
    expected_total *= 2;
}
```

If we run our code, all the assertions pass. These properties do not prove we are correct, but they do give us some confidence in the generation code. We will now look at one final property of the triangle and then round off with another pattern, just for fun. Again, we will practice more C++ on the way.

### 2.3.4 How many rows can we generate correctly?

As each number is the sum of two previous numbers and we started with positive numbers, we should never get negative numbers. To the uninitiated and mathematicians, adding positive numbers should always give positive numbers. However, numbers do surprising and sometimes annoying things on computers, such as overflowing. We will start by setting up a test and then see if we can break it. If we keep adding `ints`, we will eventually overflow the maximum possible size. The standard tells us this is undefined behavior:

If during the evaluation of an expression, the result is not mathematically defined or not in the range of representable values for its type, the behavior is undefined. ([https://eel.is/c++draft/expr](https://eel.is/c++draft/expr))

We could do some mathematics to find the maximum number of rows we can fit into our chosen numeric type. However, if we see what happens when we try to keep adding rows, we can learn some more C++ on the way. Although we are relying on undefined behavior, in Visual Studio, an integer wraps around so we can find the maximum number of rows we can safely generate.

There are various ways to check that the values are not negative. We can check whether every number is positive or try to find any negative numbers. We could write our checks in a for loop, but we will heed the advice to use algorithms where we can. In fact, we will try a few approaches, getting a bit more practice with algorithms, and we will learn more about ranges too.

The `algorithm` header provides several *non-modifying sequence* operations. Many of these find or search for elements. We can use `all_of` to check that all elements are positive. We could also use either `none_of` or `any_of`, which do what we might expect. All three take a *unary predicate*, which is a function that takes a value and returns a `bool`. The values come from a container or range.

We want to check that all our numbers are greater than zero. This seems more positive than saying none of them are negative. We could write a function, but we can also use an anonymous function, known as a *lambda*. The syntax looks very much like a normal function, but it has no name and has a *capture list* at the start indicated by square brackets:`[]`. This allows us to capture local variables by reference or as a copy. We would say `[&]` to capture anything used in the body of the lambda expression by reference, and `[=]` to capture anything used by value. We could also specify specific variables by saying `[=,&x]`, so that x is captured as a reference, and anything else is captured by value. We could also explicitly name `y` as captured by value, in which case, we do not need the equals sign: `[y,&x]`. We don’t need to capture anything in our case. We only need to check whether any integer is greater than or equal to zero:

```cpp
[](int x) { return x >= 0; }
```

A named function would look like this:

```cpp
bool non_negative(int x){ return x >= 0; }
```

The syntax for each is similar, with a parameter list and the body in curly braces. The named function must specify a return type. The lambda can use a trailing return type, which we saw in chapter 1, but the return type is deduced if none is provided. Lambda expressions construct *closures*, borrowing a term from functional programming. We will look at this in more detail in the next chapter.

We use our lambda in `std::all_of` like this:

```cpp
std::all_of(row.begin(), row.end(),  { return x >= 0; })
```

Using a named function is absolutely fine too, but for small functions, it can be easier to see what is happening if everything is in one place. Now, we have explicitly stated `int` as the parameter type. We know our `vector` contains integers. However, we were told to almost always use `auto` earlier, and we can do that here too:

```cpp
std::all_of(row.begin(), row.end(),  { return x >= 0; })
```

If we were to change the type contained in the vector, we wouldn’t need to change this code as well. In fact, we can also use a range to check all the rows like the following:

```cpp
assert(std::ranges::all_of(row,  { return x >= 0; }));
```

We’ve used a range-based `for` loop several times now and used `ranges::copy` in listing 2.5 to send a row to the screen. We know some of the standard algorithms, such as `all_of`, have a `ranges` equivalent, although not all of the algorithms have equivalents. Where they do exist, they save us from typing out `begin` and `end`. Ranges offer far more, though. Containers and algorithms are a part of the STL. The two abstractions are useful but rely on iterators. Writing your own can be cumbersome, and if you want to compose algorithms, you need to track where the end is after each call. Notoriously, the `remove_if` algorithm does not remove anything. Instead, it shunts the elements you do not want removed to the start of the collection and returns an iterator to the first of the unneeded elements, which you can use instead of `end` if you want to do something further without these elements. The following code shows what happens if we forget to track the new end.

#### Listing 2.13 Using remove\_if

```cpp
auto v = std::vector{ 0, 1, 2, 3, 4, 5 };
auto new_end = std::remove_if(v.begin(), v.end(),
     { return i < 3; });
std::cout << '\n';
for (int n : v) {
    std::cout << n << ' ';
}
for (auto it = v.begin(); it != new_end; ++it) {
    std::cout << *it << ' ';
}
```

The first loop prints out `3,4,5,3,4,5` in Visual Studio because the elements less than 3 have been removed. Other compilers might give different results. However, we now have three elements beyond the new end. The second loop displays `3,4,5` as required. Things get out of hand quickly if you need to filter and transform several times over.

Ranges avoid this problem. They allow us to take a read-only `view` of a container and filter or transform the elements in the view, without needing to keep track of iterators. We can access the `ranges` view using `std::view`. This is a convenient shorthand for `std::ranges::views`, defined in the `ranges` header. If we want to skip over initial elements less than 3, we can use `drop_while`, which may be familiar from various other programming languages:

```cpp
for (int n : std::views::drop_while(v,  { return i < 3; })) {
   std::cout << n << ' ';
}
```

If your compiler does not support ranges yet, try this out on the Compiler Explorer ([https://godbolt.org/z/YrnsTGbfx](https://godbolt.org/z/YrnsTGbfx)). We can also use the pipe character `'|'` to apply `drop_while` to our container. The pipe character is an operator allowing us to chain together multiple algorithms, which is neat and powerful. If we want to compose several views, the first approach would end up with several calls nested deeply inside brackets, whereas separating the steps with the pipe operator makes code easier to read. You may be familiar with the pipe characters used in Unix to send the output from one command to another. We only want one filter for this example. We can rewrite the version sending the vector to the `drop_while` function using the pipe operator like this:

```cpp
for (int n : v | std::views::drop_while( { return i < 3; })) {
   std::cout << n << ' ';
}
```

If we run it, we see 3, 4, 5 without having to concentrate on remembering which iterator is pointing where.

We can use views to make sure that we have no negative numbers in our triangle’s rows. Rather than skipping initial elements using `drop_while`, we want to filter out any negative numbers, so we use the `filter` function.

#### Listing 2.14 Making sure there are no negative numbers by using a view

```cpp
auto negative =  { return x < 0; };
auto negatives = row | std::views::filter(negative);
assert(negatives.empty());
```

As with the `drop_while` example, we have the form

```cpp
v | function(lambda)
```

We can add this check to our tests for negative numbers. Everything is fine if we stick with generating 16 rows. If we try 35 rows, however, the assertion fails. When we learned how to generate rows in the triangle, we noted that we would run out of numbers eventually. We found our largest possible entry using `std::numeric_limits<int>::max()`, which is likely to be 2,147,483,647, depending on your compiler. The maximum value in the 34 <sup>th</sup> row is 1,166,803,110. We then get double this amount in the next row because we add adjacent values, which would give 2,333,606,220. This number overflows an int, and the behavior is undefined by the standard as we saw. On some systems, this wraps around to the minimum value, -2147483648, and then counts up again. That is why our test fails. An unsigned would give us more space: it would still wrap around after 4,294,967,295, but to 0. This would make the error harder to spot.

The core guidelines tell us we should not try to avoid negative values by using `unsigned` ([http://mng.bz/M9VQ](http://mng.bz/M9VQ)). We can assign a negative value to an unsigned, for example, `unsigned int u1 = -2`. Annoyingly, this compiles and gives us a large positive number. With a signed integer, we can check that the value is not negative. With an unsigned integer, we cannot check anymore. We know how many rows we can safely generate. Let’s test one final property of the triangle.

### 2.3.5 Checking whether each row is symmetric

Every row is symmetric. The first and last numbers are both 1s, which is symmetric, and we checked this. We can go further and check all the entries for symmetry. This is like checking that a word is a palindrome, meaning that it reads the same backward as forward. CppReference uses checking for palindromes as an example of ranges’ `equal` method ([http://mng.bz/amej](http://mng.bz/amej)). We can repurpose this to check our vector. We need to make sure that the first half of a row matches the second half reversed. Ranges provide a view of a container. Views have a `take` method, which walks over as many elements as we ask for. We need the first half, that is, `v.size()/2`. We compare this with the second half, reversed, using `ranges::equal` method.

#### Listing 2.15 Checking for symmetry

```cpp
bool is_palindrome(const std::vector<int>& v)
{
    auto forward = v | std::views::take(v.size() / 2);
    auto backward = v | std::views::reverse
                      | std::views::take(v.size() / 2);
    return std::ranges::equal(forward, backward);
}
```

Note that we have chained together views with the pipe operator and do not need to focus on which iterators are needed where. We can add one final assertion to our tests using the palindrome function:

```cpp
assert(is_palindrome(row));
```

We now have a useful set of tests and have used a handful of methods in the ranges’ library. There are many other patterns in the triangle, but as this chapter is nearly done, we will only look at one more pattern to pull together what we’ve learned.

### 2.3.6 Highlighting odd numbers in a row

If we highlight the odd numbers in the triangle, we will see another pattern. Looking back at our code to show the triangle in listing 2.7, we can transform each row before we print it using another tool from the `ranges` library. Every odd number is one more than a multiple of two, so we can check `x % 2` to find odd numbers. We will display them with a star to see the pattern. Otherwise, we display a single space. We will use the view’s transform method to apply action to each row:

```cpp
auto odds = row |
   std::views::transform( { return x % 2 ? '*' : ' '; });
```

We can use our transformation code to give something similar to listing 2.7, where we showed the actual values in the triangle. Figure 2.8 illustrates the resulting pattern.

<br/>

![[figure-2-8.png]]

> Figure 2.8 An approximation to the Sierpinski triangle obtained by printing an \* for an odd number and a blank space for an even number

<br/>

#### Listing 2.16 Showing odd numbers as stars

```cpp
void show_view(std::ostream& s,
   const std::vector<std::vector<int>>& v)
{
   std::string spaces(v.back().size(), ' ');
   for (const auto& row : v)
   {
       s << spaces;
       if (spaces.size())
           spaces.resize(spaces.size() - 1);
       auto odds = row | std::views::transform(                           { return x % 2 ? '*' : ' '; });
       for (const auto& data : odds)
       {
           s << data << ' ';
       }
       s << '\n';
   }
}
```

We expected the symmetry. The repeating triangles might be a nice surprise. This approximates the Sierpinski triangle, which is a triangle shape recursively divided into smaller triangles. If we made an equilateral triangle, folded the corners over each other, and drew lines where we made the folds, we would get the blank triangle in the middle of figure 2.6, along with a triangle at the top, one on the bottom left, and one on the bottom right. We can then do the same with the three triangles on the corners. This triangle is fractal because it repeats as you zoom in. We could keep dividing up the triangles forever in theory, showing this fractal property. We could also try out even numbers instead or a different modulus, and we would see other patterns.

In this chapter, we have learned a fair bit about how to use vectors. We have not covered everything, but we have done enough to recognize various C++ features and test our code.

- Containers are part of the STL, and the compiler can sometimes deduce the type of expressions for us.
- We can initialize objects using an initializer list {value1,value2,...} when we want to provide values directly.
- We can use emplace\_back or emplace when we want to create an object in place directly in a container, or push\_back or insert when we already have an object.
- The range-based for loop is a common way to walk over a container, avoiding iterators or indices.
- Use auto almost always, including relying on class template argument deduction when using containers.
- std::move casts a value to an rvalue, allowing perfect forwarding.
- Some versions of standard algorithms take begin and end, while others in the std::ranges namespace now support ranges.
- Views and filters can be chained with the pipe operator.
- Lambdas are unnamed functions that can capture variables and form closures.
- Use format to align text or set the width or precision of numbers for speed and type safety.
