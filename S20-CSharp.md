---
title: C# Introduction - Walkthrough
---
C# is a versatile, compiled language. It is supported by [.NET](https://dotnet.microsoft.com/) (pronounced "dot net"), a rich, open runtime environment. Variations of .NET support all major platforms.

* **.NET Core:** Console apps on MacOS, Linux, Windows (and more)
* **.NET MAUI:** (Multi-Platfrom Application User Interface) Rich GUI apps for iOS, Android, macOS, and Windows
* **.NET Xamarin:** iOS, macOS, Android, Windows
* **Unity:** Games on all major consoles (Xbox, PlayStation, Nintendo), iOS, Android, Windows, and many others.
* **ASP.NET:** Web applications hosted on Windows or Linux
* **.NET Nanoframework:** Microcontrollers for embedded systems.
* **.NET Framework:** Windows

## Introduction

For this walkthrough, we will build a simple console application to demonstrate basic language features. This app will run on .NET Core or .NET Framework.

## Development Environment

Most will use [Microsoft Visual Studio](https://visualstudio.microsoft.com/) (the IDE, Integrated Development Environment version) which makes building and debugging C# applications very easy. The Community Edition is free and does everything you need.

You can also install the [.NET SDK](https://docs.microsoft.com/en-us/dotnet/core/sdk) (Software Development Kit) and write your apps using Visual Studio Code or your preferred text editor. In that case, you will build from the command line.

## Hello, World

In **Visual Studio**, create a new project and choose "C# Console Application". The exact steps vary with the version of Visual Studio you are using.

It will automatically create a "Hello, World" console app for you.

If using the command-line SDK, open a text editor and create a new C# file called "Program.cs". Then enter the following code (the same as produced by the IDE).

```c#
using System;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

Similar C and C++, program execution starts with a function called `Main()`. Like all functions in C#, it must belong to a class. It is static meaning that it doesn't require an instance of the class to be created.

The static `Console` class has functions for reading and writing to the console. In this case, we use `WriteLine()` to write the string.

In **Visual Studio** click the "Run" button (Green Arrow) to run the app.

## Memory Management

C# uses a "garbage collected" heap. Like C and C++, you create objects using the `new` keyword. But you don't have to free them. A short time after the last reference to an object goes away the garbage collector, running on a background thread, will free the memory in the heap.

## Variables and Strings

Variables are strongly typed in C#. However, all types inherit from `Object`. So, a variable of type `Object` can accept any type.

A string prefixed with `$` is an "Interpolated String". At runtime, any expression in braces will be replaced with the evaluation of that expression.

```c#
using System;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            string name1 = "Steve";
            string name2 = "Janet";
            Console.WriteLine($"Hello {name1} and {name2}!");
        }
    }
}
```

`null` and empty string are different and have different meanings. C# will throw an exception if you attempt to access the value of a variable set to `null`.

Try this:

```c#
using System;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            string name = "Steve";
            Console.WriteLine($"Length = {name.Length}");
            name = "";
            Console.WriteLine($"Length = {name.Length}");
            name = null;
            Console.WriteLine($"Length = {name.Length}");
        }
    }
}
```

The null reference exception will be caught by your IDE debugger and you have an opportunity to examine the variables.

## C++ Operators

<table>
<tr><th>Operator</th><th>Description</th><th>Example</th></tr>
<tr><td>=</td><td>Assigns a value to a variable.</td><td>i = 6</td></tr>
<tr><td>+</td><td>Adds two values</td><td>i = 5 + 5</td></tr>
<tr><td>-</td><td>Subtracts a value from another</td><td>i = 5 - 4</td></tr>
<tr><td>*</td><td>Multiplies two values</td><td>i = 5 * 8</td></tr>
<tr><td>/</td><td>Divides a value by another</td><td>i = 15 / 3</td></tr>
<tr><td>()</td><td>Parentheses group values</td><td>(5 + 4) / 7</td></tr>
<tr><td>+=</td><td>Adds to a variable</td><td>i += 3</td></tr>
<tr><td>-=</td><td>Subtracts from a variable</td><td>i -= 7</td></tr>
<tr><td>==</td><td>Tests for equality</td><td>i == 5</td></tr>
<tr><td>!=</td><td>Tests for inequality</td><td>i != 7</td></tr>
<tr><td>< <= > >=</td><td>Test for inequality</td><td>i >= 5</td></tr>
<tr><td>&&</td><td>Logical AND</td><td>if (clear && present)</td></tr>
<tr><td>||</td><td>Logical OR</td><td>if (daring || foolish)</td></tr>
<tr><td>+</td><td>Concatenates Strings</td><td>str = "BYU" + "Cougars"</td></tr>
<tr><td>!</td><td>Not: Reverse true and false</td><td>if (!ready)</td></tr>
<tr><td>[]</td><td>Brackets access values in arrays or collections.</td><td>names[4]</td></tr>
<tr><td>.</td><td>Dot separates objects and members</td><td>time.Hour</td></tr>
</table>


## if...else

```c#
static void Main(string[] args)
{
    Console.WriteLine("What is BYU's mascot?");
    string value = Console.ReadLine();
    if (value.ToLower() == "cosmo")
    {
        Console.WriteLine("Correct!");
    }
    else
    {
        Console.WriteLine("Sorry, incorrect.");
    }
}
```

Of course, this should be wrapped in a namespace and a class like the prior examples.

## Prime Number Calculation

This implementation of the Sieve of Eratosthenes demonstrates arrays, looping, and conditionals.

```c#
class Program
{
    const int limit = 100;

    static void Main(string[] args)
    {
        bool[] isPrime = new bool[limit];
        for (int i = 0; i < limit; ++i)
        {
            isPrime[i] = true;
        }

        // Start with the first prime, which is 2.
        for (int i=2; i<limit; ++i)
        {
            if (isPrime[i])
            {
                Console.WriteLine(i);

                // Mark all multiples of i as not prime
                for (int j = i*2; j < limit; j += i)
                {
                    isPrime[j] = false;
                }
            }
        }
    }
}
```

## Collection Classes

C# includes a rich set of collection classes in the `System.Collections.Generic` namespace. You must have a `using` statement to make use of them.

Useful collections:
* `List<T>`: Much like an array but with variable numbers of members and has handy methods like `Sort`
* `Dictionary<T1, T2>`: Creates a mapping from keys to values.
* `HashSet<T>`: A collection of unique values.
* `Stack<T>`: A last-in-first-out collection of values.
* `Queue<T>`: A first-in-first-out collection.

```c#
using System;
using System.Collections.Generic;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var colors = new List<string>() { "blue", "yellow", "red", "green" };
            Console.WriteLine(String.Join(' ', colors));

            colors.Add("fuchsia");
            colors.Remove("red");
            Console.WriteLine(String.Join(' ', colors));

            colors.Sort();
            Console.WriteLine(String.Join(' ', colors));
        }
    }
}
```

## ForEach loop

Loops through the values in a collection.

To the colors code above add the following:

```c#
foreach(string color in colors)
{
    Console.WriteLine($"My favorite color is {color}.");
}
```