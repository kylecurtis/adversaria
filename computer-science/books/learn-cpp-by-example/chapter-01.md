# Chapter 1: Hello again, C++!:  

book published february 2024
livebook updated March 2024

<br />

---

<br />

### This chapter covers

- Why C++ is relevant
- When C++ is useful
- What you need to know before reading this book
- How this book will bootstrap your knowledge of C++
- What you’ll learn in this book

<br />

---

<br />

C++ is an old but evolving language. In programming, you can use it for almost anything and will find its application in many places. In fact, C++’s inventor, Bjarne Stroustrup, described it as the invisible foundation of everything. Sometimes, C++ might be present deep inside a library of another language because it can be used for performance-critical paths. Furthermore, it can run in small, embedded systems, or it can power video games. Even your browser might be using the language. C++ is almost everywhere.

The language is compiled and targeted at specific architectures such as a PC, mainframe, embedded devices, bespoke hardware, or anything else you can think of. If you need your code to run on different types of machines, you need to recompile it. This has pros and cons. Different configurations give you more to maintain, but compiling to a specific architecture gets you down to the metal, allowing the speed advantage. Whatever platform you target, you will need a compiler. You will also need an editor or integrated development environment (IDE) to write code in C++.

C++ stems from C, which has similar advantages but is a lower-level language. If you recognize ++ as the increment operator, you’ll realize that the language’s very name suggests it is a successor to C. You can avoid the depths of pointers and memory allocations with C++ by writing higher-level code. You can equally drop down to C or even assembly language in C++ code. Although C++ was never intended to take over the world or even replace C, it does provide many new ways to approach coding. For example, you can do a surprising number of things at compile time, using type-safe features rather than preprocessor macros often used in C.

This language underpins diverse technologies, including compilers or interpreters for other languages, and even C++ compilers themselves. You can develop libraries for use in other languages, write games, price financial instruments, and do much more. If you ever typed make at a prompt, you were probably using C++ without knowing it. C++ may power your browser or e-reader if you are reading this book in digital form, or it may have been used to write device drivers for your machine.

This book will give you a firm grounding in a handful of C++ language and library features. Each chapter walks through a small, self-contained project, focusing on one area. Along with each chapter’s main feature, other parts of the language will be covered. For example, if you fill a container, such as a vector or an array, you may also want a way to display and manipulate its contents. Thus, the next chapter focuses on vectors but also introduces ranges and lambdas, as well as using format to display output. By gradually building up your repertoire, you will gain confidence, which will allow you to rediscover the joy of C++. This book will focus on several fundamental parts, showing you various ways in which the language is easier now than it used to be. You will end up with a firm grounding, ready to use and learn more C++.

<br />

---

<br />

## 1.1 Why does C++ matter?

C++ is designed by a committee. Some languages are introduced and developed by a company or an individual. C++ is not. Originally invented by Bjarne Stroustrup, Working Group 21 (WG21) of the International Organization for Standardization (ISO) is now responsible for its new versions. You can find more details at [https://isocpp.org/std](https://isocpp.org/std). There’s been a new ratified standard every three years since 2011, each adding new features and sometimes simplifying ways of doing things. This means there is a lot to learn. Some documentation and descriptions sound as if they were written in legalese, which can be overwhelming. This book will use a few precise definitions to help you get the hang of parsing such explanations. Members of the committee make suggestions, write papers to explain their ideas, and demonstrate how to implement new features or small improvements, which leads to innovations that influence other programming languages too. For example, Java and C# would not have generics if C++ hadn’t introduced templates. Ideas do flow in both directions. C++ also takes on board ideas from other languages, including functional programming idioms, such as lambdas.

These recent standards injected new life into C++, causing lots of excitement. Companies that have been using C++ for years may previously have relied on in-house libraries to support features that are now part of the core language. Upgrading to a newer standard can be hard work, but it means more people will be able to work on the code base without spending time getting up to speed with a niche implementation. In addition to changes in the technology stack in businesses, there are now several conferences devoted to C++, along with podcasts and blogs, as a new cohort gets involved. C++ has a reputation for being very hard-core, with geeks arguing about difficult stuff and being mean to newbies (and each other). This is partially unfair, but the IncludeCpp group ([https://www.includecpp.org/](https://www.includecpp.org/)) tries to be inclusive and welcoming. They have a discord group and tend to have a stall at C++ conferences, so if you go alone, you can head straight to them and say hi. Recent changes have made several parts of C++ easier to explain and use but have introduced more edge cases and complications. This book will tend to stick with commonly available features that make your life easier, but it’s worth knowing a bit about some new, less widely supported features.

If you knew C++ before C++11, you might be intimidated by the changes. In fact, if you spent time catching up and blinked, you've still missed a lot. Fear not. Although C++ may resemble riding a bicycle (it hurts if you fall off), C++ doesn’t have to be painful. This book will stop you from falling down the rabbit hole. You can have fun and learn many approaches and paradigms, from object-oriented programming to functional approaches. A grounding in C++ will make other languages and approaches easier to understand too. Furthermore, C++ is so pervasive that it will never go away, so it’s useful to understand a little. You’ll never know all of it. Even Bjarne himself is reputed to have said he’d rate himself at seven out of ten on C++ knowledge, so fear not. To be a good programmer, you don’t have to know every detail of the language. Knowing enough as a basis to learn more is important. If you bootstrap your understanding now, you will also find it easier to keep up.

C++ has grown over time. Initially, C++ was C with classes, introducing the keyword new (along with delete and class) and the idea of constructors and destructors. These are functions that run automatically when an object is created and when it goes out of scope or is deleted. Unlike garbage-collected languages, such as C# and Java, you have control over an object’s lifetime. Proponents of garbage-collected languages sometimes deride C++, claiming it’s all too easy to end up with memory leaks. Now, you don’t need to use new and delete, and C++ has smart pointers to help with memory management. The language evolved over time, adding various new features. It still remains relatively compact, although it has grown since it began. The language, like all others, is what you make of it. You can write terrible code in any language. You can also write beautiful code in any language, but you need to learn how. By trying out code as you read this book, you will end up with some small programs to play with. They will cover various aspects of the language, giving you a firm grounding. You will see how C++ can be awesome.

There are many rivals to C++, yet C++ has staying power. It consistently remains at the top of the TIOBE index (https://www.tiobe.com/tiobe-index/#2022) and was ranked among the top three in 2022. You could use C instead, but you will see stars (pointers being represented with an \* character). If you want a data structure beyond an array, you’ll have to roll your own. You could use High Performance Fortran for extremely fast computation. The UK Met Office uses Fortran for their weather modeling because they have a vast amount of data to crunch in a very short time. Fortran also loiters in many academic institutions, so you may have seen or used it if you are an academic or student. However, it is a little niche. You are more likely to come across some C++ code in the wider world.

Various new languages have been invented, aiming to deal with C++ defects or annoyances. D feels similar to C++ because of the C-like syntax and high-level constructs, and it was invented to deal with the aspects of C++ the creators didn’t like. Meanwhile, C++ continues to evolve, but it always aims to remain backward compatible, so it is constrained by historical decisions. New languages don’t have a legacy and thus have more freedom. Go, Objective C, Swift, Rust, and recently Carbon also rival C++ in some areas. That’s fine, and learning several languages and thinking about what might make a programmer’s life easier is a good thing. Many times, new ideas introduced into the latest C++ standards are based on insights from other languages. As new languages have been introduced, C++ still remains prevalent and often takes on board any challenges they present. C++ isn’t going away anytime soon. You can get involved and submit bug reports or suggestions too if you like. The committee consists of volunteers, who work hard to improve the language. ISOCpp provides details on how to get involved ([https://isocpp.org/std/meetings-and-participation](https://isocpp.org/std/meetings-and-participation)).

If you learn C++, you will have a solid foundation for other languages. The similarity to other languages can help you quickly pick up how to use them. You will get familiar with some data structures and algorithms, as well as various paradigms ranging from functional programming to object-oriented code. Even if you don’t end up on the standards committee or inventing your own programming language, you will be well placed to continue a journey of lifelong learning and understand what is happening under the hood.

<br />

---

<br /> 

## 1.2 When should you use C++?

You can use C++ for anything, but some use cases are more sensible than others. To prototype some machine learning or run a statistics calculation, it might be quickest to start in Python and use existing libraries. Of course, those libraries may include some C++. If you feel confident enough to look at the source for a library to figure out why a bug happens, you have a head start on other programmers. If someone needs a program with a frontend, be that a website or local program with a graphical user interface (GUI), you could build the whole thing in C++, but it might be easier to split up the software. C++ doesn’t support GUIs in the core language, unlike, say, C#, so the frontend would require an external library, such as the cross-platform C++ library Qt ([https://www.qt.io/](https://www.qt.io/)). You could also write the frontend in something completely different and call the C++ code as a service or library. So, given that you might start in another language to try out an idea or build part of your application in another tool chain, when should you use C++?

If you want a first-person shooter-style game, you could try to write it in JavaScript, but using a language that compiles to the hardware is more sensible. An interpreted language will be slower than a compiled language. C++ is therefore frequently used to write the game engine, render the graphics, work out the physics, detect collisions, and provide sound and artificial intelligence for bots. A scripting language might call into this engine, but the engine’s power and speed often come from C++, squeezing every inch of power out of a top-end graphics card or another component of an expensive gaming rig. This also makes C++ suitable for high-performance computing (HPC), financial applications, embedded devices, and robotics.

Because C++ takes you close to the metal, you can break things. It’s possible to brick an embedded device if you are not careful, rendering the machine inoperable. You’re unlikely to manage that if you write a program to run on your laptop or computer. It might crash, proudly announcing a segmentation fault or similar on the way out. An embedded device without an operating system is different. If it’s only running one program without an operating system, and that goes wrong, bad things can happen. That’s okay too. Bjarne Stroustrup once said, “If you never fail, you aren't trying hard enough” ([https://www.stroustrup.com/quotes.html](https://www.stroustrup.com/quotes.html)). Although the language allows you to use raw pointers and potentially step over memory bounds or invoke undefined behavior, this book will steer you away from danger. Just remember, it has been said that with great power comes great responsibility. With enough of a solid foundation, you can program responsibly, learn lots, and have fun.

Although C++ doesn’t support several things natively, such as unit testing, GUI coding, or even networking (that nearly made it into C++23 and might make it into a future standard), you can do these things using a suitable third-party library. What the core C++ language does provide is a large and thought-through standard library. If you were using C and wanted a normal distribution of random numbers, you’d need to dust off a mathematics book or read what Donald Knuth has to say on the matter. If you need a lookup table, you can use C++’s standard map. In C, you’d have to write your own. In fact, you get stacks, queues, heaps, and almost every data structure you can think of in C++ out of the box, along with a vast number of algorithms. This means learning C++ provides a solid foundation for understanding other languages.

If you need to do a lot of number crunching quickly, C++ is a great choice. Modern language versions also support a variety of random number distributions, as you will see in this book, making it relatively easy to set up a variety of complicated simulations. Even without using the latest and greatest parts of the language, you can build some serious applications in C++. For example, the MRC Centre for Global Infectious Disease Analysis, affiliated with Imperial College in the United Kingdom, open sourced their COVID-19 simulation model ([https://github.com/mrc-ide/covid-sim](https://github.com/mrc-ide/covid-sim)). These models were used to decide public policy in the United Kingdom during the pandemic. C++ does the heavy lifting, and some scripts, written in R, are provided to display the results.

C++ is often described as a multi-paradigm language. It supports object-oriented programming, but you are allowed to write free functions too. You can write low-level procedural code if you want, but you can also use generics (i.e., templates) and functional-style programming. You can even do template meta-programming (TMP), making the compiler do calculations for you. This was an accidental discovery, presented by Erwin Unruh at a C++ committee meeting in 1994. He demonstrated a program that didn’t compile but rather printed out the prime numbers in the compiler error messages. Playing with TMP can be fun to explore and push to extremes, but simpler cases can give faster runtimes with type-safe, compiler-evaluated constants. If you learn how to use some C++, you will have a stable foundation for many other languages and know a great variety of different programming paradigms.

<br />

---

<br />

## 1.3 Why read this book?

As the language evolves, people are writing books for each new standard and more general-purpose style guides. The style guides won’t make any sense if you don’t know the new features, and the new features build on previous changes, so the full details can be overwhelming. Where do you start in the face of a moving target? Where you are. You need a way to bootstrap your learning. This book will take you through some central changes via small projects so you have something to experiment with. By using some of the new features, you’ll be better able to recognize what modern C++ code is doing and know where to keep an eye out for further changes and developments.

Instead of reading a list of all the changes you may have missed, the ISOCpp website has a FAQ section ([https://isocpp.org/wiki/faq](https://isocpp.org/wiki/faq)) that provides an overview of some recent changes and big-picture questions. This website is run by the Standard C++ Foundation, a not-for-profit organization whose purpose is to support the C++ software developer community and promote the understanding and use of modern Standard C++. The site even has a section for people with a background in other languages who want to learn C++. It doesn’t have a section for “Learning C++ if you already knew C++ a while ago.” This book plugs that gap. You don’t need a long list of every feature that’s been introduced over the years. You need just enough to get your confidence back.

You can keep an eye on the myriad and excellent resources online to stay aware of what has been and is changing in the language. ISOCpp will help you do this. However, you do need to stop and try things out to learn. Spending time experimenting will pay off, and this book will guide you through some useful experiments. Trying out features in bite-sized chunks will help you crystalize ideas and concepts. You will see alternative approaches from time to time. By seeing two ways to put items in a vector, you will learn a new feature (the emplace methods) and recall an old feature (push\_back). This will help you read other people’s code and not be wrong-footed by unfamiliar approaches. You will learn how to think through alternatives, becoming aware of advice from different places, which sometimes conflicts. This book will take a pragmatic approach while encouraging you to think about alternatives.

<br />

---

<br />

## 1.4 How does this book teach C++?

This book covers a subset of features introduced into C++, from C++11 onward. At the time of writing, C++23 is in feature-freeze, making it ready for a new standard. Each chapter focuses on one main feature, although it introduces and uses other modern features and idioms as well. Some people who used to know C++ well are put off by how many new things they will have to learn to start using it again, and beginners often get frightened off quickly. It doesn’t have to be so hard. Getting up to speed now will make it easier to keep track as C++ continues to change and evolve. If you haven’t used C++ for a long time and have seen other books going through an extensive list of all the new features and idioms, but you don’t know where to start or how to use them, this book will help you focus on some important parts, enabling you to dive into gnarly edge cases and thorough explanations elsewhere afterward.

This book focuses on self-contained projects using various parts of C++. You will try out some ideas and learn language features on the ride, rather than plow through each part of the language’s syntax and standard libraries using one-line examples. If you have gone rusty, this book will give you a chance to practice and rediscover the joy of using C++. As you probably realize, writing a whole program gives you more practice than playing around with one or two lines. This book will therefore help you teach yourself.

<br />

### 1.4.1 Who this book is for

This book is aimed at people who have used a little, or even a lot, of the language and lost track of recent changes. If you recognize the syntax and want to try to learn more, you will gain something from this book. If you know what int x \= 5;int & y=x; do, have used an std::vector<int> before, and recognize std::cout << x, you will be able to follow. If you’ve seen int x{1}; before, you’re part way there. If not, don’t panic. The curly braces are a new way to initialize almost everything, which you’ll soon get the hang of. If you used to know all the gnarly edge cases and quote chapter and verse of a previous standard, this book will help you focus on a handful of new features to get you back in the driving seat. Once you’ve finished reading this book, you will know where to get an up-to-date compiler and how to keep an eye on upcoming changes, and you’ll be able to read and write modern C++. Let’s look at some code now to get a feel for a few new ways of writing the language.

### 1.4.2 Hello, again, C++!

It’s conventional to start learning a language with a “Hello, World!” program, so let’s do just that. The following code prints a greeting onscreen.

#### Listing 1.1 Hello, World

```cpp
#include <iostream>
 
auto main() -> int {
    std::cout << "Hello, world!\n";
}
```

<br />

If you save this to a file called hello_world.cpp, you can compile it. For example, using the GNU compiler collection (gcc; see [https://gcc.gnu.org/](https://gcc.gnu.org/)), use g++ supporting C++11 with

```sh
g++ hello_world.cpp -o ./hello.out
```

<br />

This book assumes you recognize the include statement, the scope resolution operator::, and the stream insertion operator <<. The code inserts the greeting to standard (std) cout inside the main function, the usual entry point for executable code. You knew that, however, the *trailing return type* \-> at the end of a function name may be unfamiliar, together with the keyword *auto* at the start of the line. You can write int main() here instead, as you always used to, but when C++11 introduced this feature, many people started using it everywhere for consistency. It becomes useful when you want to deduce the type a function returns. Our hello program doesn’t need the trailing return. Furthermore, main is special in that it returns 0 by default, so it does not need a return statement even though it returns an int. Without a trailing return type, some template functions can be very tricky to specify. Let’s consider an example that uses a template function.

You can use the + operator easily enough to add numbers. For example, auto x = 1 + 1.23. There’s our friend auto again. We’re trying to sum an integer (1) and a double (1.23), so the result is a double due to *integer* *promotion*. If you want a general-purpose addition function, you could attempt to write overloads for every possible pair of parameters or, more sensibly, write a template function. Even better, you can use one that is already written for you. The functional header includes a definition of plus. In fact, this header contains two definitions, one of which sums two parameters of the same type, which we create by saying std::plus<int> to add two integers. Since C++14, a version that deduces the template argument types was introduced. Using std::plus<> picks the new specialization, which works out the types for us. If you try the first version, 1.23 gets converted to an int, so you get 1 + 1, which some compilers warn about, whereas the second version adds the int 1 and the double 1.23 to get 2.23. Try it out!

#### Listing 1.2 Adding two numbers

```cpp
#include <iostream>
#include <functional>
 
auto main() -> int {
    std::cout << std::plus<int>{}(1, 1.23) << '\n';
    std::cout << std::plus<>{}(1, 1.23) << '\n'; 
}
```

You are used to functions starting with the return type and then having a name and parameters, such as int main(). The return type is given first. To specify the return type, plus needs to express the addition operation of the two function arguments. This is much easier to do with parameter names, but those are not visible to the usual return type. The trailing return type makes using parameter names to specify the return type possible. You need to say auto at the start and indicate what type is returned after the trailing \->.

Let’s look at a simplified version of the operator() for the plus<> specialization. Remember, we want to declare a function that takes two things and returns the sum of them. We’re going to use a template with two typenames, allowing two different types to be summed. The addition itself is the easy part and simply uses the + operator. The declaration has auto at the start and a type at the end.

<br />

#### Listing 1.3 A function to add two different types

```cpp
template<typename T, typename U>
auto simple_plus(T lhs, U rhs) -> decltype(lhs + rhs)
{
   return lhs + rhs;
}
```

<br />

The operator function is a template using two different types, T and U, for the left-hand side (lhs) and right-hand side (rhs) of the binary operation, respectively. The return type is declared using decltype specifier and the expression lhs + rhs. If you squint, you can see how that’s similar to the syntax for the main function we saw earlier. Put them side by side and have a look:

```cpp
auto main() -> int
auto simple_plus(T& lhs, U& rhs ) -> decltype(lhs + rhs)
```

<br />

You can see the auto followed by the function name and parameters, then the arrow and the trailing return type in both cases. When we add 1 and 1.23, the parameter types are deduced to be an integer and a double. The trailing return type uses the expression (1 + 1.23) to get the return type of a double.

If you already recognize these new features, great. There are plenty more new things to learn. If you’ve never seen any of them before, concentrate on the main point here, which you saw when you tried out “Hello, World!”: the trailing return type. You’ve learned something already.

<br />

### 1.4.3 What you’ll learn from reading this book

You’ll learn how to use some new elements of the language, from ranges to random numbers, and learn several other simpler ways of doing things on the journey. This book starts with a vector and builds up from there. Vectors are a good way to revise and then learn new features, including ranges, views, functors, and lambdas. Once you’re comfortable filling, displaying, querying, and manipulating a vector using ranges and algorithms, you’ll be ready to use other parts of the standard library, including time (chrono), random numbers, and, finally, coroutines.

Range-based for loops introduced in C++11 made the language simpler. You can use them to walk over a container without needing to dive into iterators first. Over time, full-blown ranges have become standard too, providing convenience and avoiding the direct use of iterators, as well as offering more unified lookup and extra safety. Previously, it was possible to pass the start of one container and the end of another to an algorithm and not realize this until something horrible happened at runtime. Ranges avoid that problem. You’ll become familiar with using ranges to view and copy the contents of a container.

You’ll find out how and why you don’t need so much boilerplate code in a class by using the default keyword for constructors and operators. You’ll learn how to use the new random number distributions. If you’re used to calling C’s rand function, the new approach might seem complicated at first, but it’s powerful, and when used properly, it helps you avoid mistakes people often make, for example, when simulating rolling dice or shuffling a deck of cards.

By using self-contained projects in each chapter, you’ll get the chance to use all kinds of new and old features. You’ll get to the point where you understand new features, knowing when and why to use them in an idiomatic way. Sometimes opinions on the best way to do things differ. You saw the trailing return type early: auto main() \-> int. Some people love it and use it everywhere, but some people hate it. The language’s evolution has taken us beyond arguing about brace placement (sorry in advance if you don’t like my approach) and given us lots more to argue about. This book will give alternatives, firmly sitting on the fence when it comes to such discussions so that you can concentrate on trying to write some code and experiment with new ways of expressing yourself.

<br />

---

<br />

## 1.5 Some pro tips

It’s possible to get lost or overwhelmed when learning, especially if you are trying to tackle a big topic. If you bear in mind the following few tips, you’ll be able to find your way.

First, many of the new features are *syntactic sugar*, and second, many elements of code use punctuation, which is hard to look up. If you wanted to find out what the \-> symbol was doing in the main function given previously, where would you start? One very useful tool is Andreas Fertig’s C++ Insights ([https://cppinsights.io/](https://cppinsights.io/)) website. C++ Insights transforms code to show the details behind some newer C++ features. It is based on Clang ([https://clang.llvm.org/](https://clang.llvm.org/)) and Andreas’ understanding of C++ ([https://cppinsights.io/about.html](https://cppinsights.io/about.html)). If you type in the plus code we looked at in listing 1.2, C++ Insights will transform the code for you.

#### Listing 1.4 C++ Insights output

```cpp
#include <iostream>
#include <functional>
 
int main()
{
  std::operator<<
    (
        std::cout.operator<<
        (
        std::plus<int>{{}}.operator()(1, static_cast<const int>(1.23))
        ),
        '\n'
    );
  std::operator<<
    (
        std::cout.operator<<
        (
        std::plus<void>{}.operator()(1, 1.23)
        ),
        '\n'
    );
  return 0;
}
```

Try it out directly ([https://cppinsights.io/s/508b2063](https://cppinsights.io/s/508b2063)). The insight may show lots of details, and the generated code is based on Clang, so it may not always work on other compilers, but listing 1.4 shows the transformed trailing return symbol \->, along with the std::plus<int> and std::plus<void> structures being used. If you can’t understand a function you come across, try out C++ Insights for clues.

The next thing to bear in mind is that not all compilers support all the new features, so you might need more than one. At the very least, you might need to use the option /std:c++latest in Visual Studio or \--std=c++20 for g++. If you can’t face having to set up another tool, you can always try out C++ code in various compilers online via Matt Godbolt’s Compiler Explorer ([https://godbolt.org/](https://godbolt.org/)). It supports a huge variety of different compilers, allowing you to see how each behaves. This book will try to stick to common parts, but if you want to explore more, this is a great resource, along with C++ Insights. Each has a link to the other, so why not use both? Before spending time getting a toolchain setup, CppReference has a list of compiler support for each of the new features ([https://en.cppreference.com/w/cpp/compiler\_support](https://en.cppreference.com/w/cpp/compiler_support)) to help you decide which version you need. This is another great resource for checking function signatures or simply finding which standard header file you need to include to use a feature.

Finally, if you get stuck, don’t panic. The compiler may well still give you several errors if you forget a semicolon deep inside some template code. Newer compilers might pinpoint the actual problem, though, rather than giving pages of errors to wade through. Most modern compilers do try to be slightly more helpful, so if you had pain previously and gave up, things might be easier now. Nonetheless, you will get incomprehensible errors from time to time. If you can’t figure them out, ask someone for help or try starting at the first error. If that doesn’t work, try starting at the last error, or at least find one pointing at your code, rather than library code. If that doesn’t work either, comment it all out and add your code back in slowly. Or, even better, consider using version control and reverting to what worked. This book won’t take you through all the details of how to set up a sensible working environment but will point you to useful tools and things to consider along the way.

<br />

---

<br />

## Summary

- C++ is everywhere and can be used for almost anything.
- C++ is evolving, with a new standard every three years, decided on by WG21 of ISO.
- C++ is a multi-paradigm language.
- You need a compiler that supports your chosen platform.
- Other similar languages are available, but C++ gives you a solid grounding in a variety of techniques and idioms.
- No single compiler currently supports every feature of the latest version of the language, but you can use Godbolt and C++ Insights to try out short snippets to check whether they compile.
- Coding a whole program is a great way to learn, and you’ll do just that in the rest of this book.