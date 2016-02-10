---
layout: post
title: "Singleton Design Pattern"
date: 2014-12-30 10:15
description: Introduction to Singleton design pattern
categories: [Programming]
tags: [design pattern, C#]
---

The **Singleton** is one of the simplest and best-known design patterns, which restricts the instantiation of a class to **ONLY ONE** object. A singleton class only allows a single instance of itself to be created, and usually gives simple access to that instance. Most commonly, singletons don't allow any parameters to be specified when creating the instance.

In this article, we would provide different ways of implementing the singleton pattern using C# and discuss the difference in terms of lazily-load, thread-safety, and perfromance.

## Contents
{:.no_toc}

* TOC
{:toc}

---

<span class="anchor" id="difference"></span>

## Singleton and Static Class

Before describing the implementations, we clarify the difference between singletons and static classes. We can make a global single class by using keyword static. Both singleton and static class have only one instance of them. The static classes are usually used for storing global data, all the data in a static class are also static. A singleton is treated as a normal class (without static keyword) with state, which allows you to reuse code and control object state much easier. We can extend classes or implement interfaces with singletons but not with static classes. Aslo, a singleton is allocated in heap and a static class is allocated in stack.

Some tips about the singleton implementation:

- A single constructor, which is private and parameter-less.
This prevents other classes from instantiating it (which would be a violation of the pattern).
Note that it also prevents subclassing - if a singleton can be inherited once, it can be inherited twice,
and if each of those subclasses can create an instance, the pattern is violated.
The factory pattern can be used if you need a single instance of a base type, but the exact type isn't known until runtime.
- The class is sealed. This is unnecessary, strictly speaking, due to the above point, but may help the JIT to optimise things more.
- A static variable which holds a reference to the single created instance, if any.
- A public static means of getting the reference to the single created instance, creating one if necessary.

## Instantiation: Lazy vs Eager

### Lazy Instantiation

The first implementation `Singleton1` is bad because it is not thread-safe.
Two different threads could both pass the if test when the mInstance is null,
then both of them create instances, which violates the singleton pattern.
The advantage of this implementation is the instance is created inside the Instance property method,
the class can exercise additional functionality.
The instantiation is not performed until an object asks for an instance, so called **Lazy Instantiation**,
the lazy instantiation avoids instantiating unnecessary singletons when the application starts.

~~~ cs
// Implementation 1: lazy instantiation, NOT THREAD-SAFE!
public class Singleton1
{
    private static Singleton1 _instance = null;

    // Private constructor
    private Singleton1() { }

    // GetInstance
    public static Singleton1 _instance
    {
        get
        {
            if (_instance==null)
            {
                _instance = new Singleton1();
            }
            return _instance;
        }
    }
} 
~~~

### Eager Instantiation

The implementation `Singleton2` is an **eager initialization**,
which always creates an instance when the application starts.
In this implementation, the instance is created the first time any member of the class is referenced,
the common language runtime takes care of the variable initialization.
The class is marked sealed to prevent derivation, which could add instances.
The instance variable is marked readonly which means that it can be assigned only during static initialization or in a class constructor.
Hence, the *private* constructor ensures that the instance variable can be instantiated only inside the the class and
therefore only one instance can exist in the system.
The downside of this implementation is that you have less control over the mechanics of the instantiation.

~~~ cs
// Implementation 2: eager instantiation, simple thread-safe without using locks
public sealed class Singleton2
{
    private static readonly Singleton2 _instance = new Singleton2();

    // Private constructor
    private Singleton2() { }

    // GetInstance
    public static Singleton2 Instance
    {
        get
        {
            return _instance;
        }
    }
} 
~~~

<span class="anchor" id="threadsafety"></span>

## Thread-safety

### Simple Thread-safety

In the implementation `Singleton3`, all threads share a lock object to ensure that only one thread can create an instance,
because only the first thread entering in the critical section can find the instance variable is null and pass the if check.
The withdraw of this implementation is obvious, the performance suffers as a lock is acquired every time the instance is requested.

~~~ cs
// Implementation 3: thread-safe with locking the shared object
public sealed class Singleton3
{
    private static Singleton3 _instance = null;
    private static readonly object _lock = new object();

    Singleton3() {   }

    public static Singleton3 Instance
    {
        get
        {
            lock (_lock)
            {
                if (_instance == null)
                {
                    _instance = new Singleton();
                }
                return _instance;
            }
        }
    }
}
~~~

The implementation `Singleton4` improve the performance by avoiding the unnecessary lock operator.
To do so, it uses a double null-check to avoid taking out a lock every time.
However, this implementation doesn't work in Java and is easy to get wrong.

~~~ cs
// Implementation 4: double null-check. Bad CODE!
public sealed class Singleton4
{
    private static Singleton4 _instance = null;
    private static readonly object _lock = new object();

    Singleton()
    {
    }

    public static Singleton4 Instance
    {
        get
        {
            if (_instance == null)
            {
                lock (_lock)
                {
                    if (_instance == null)
                    {
                        _instance = new Singleton4();
                    }
                }
            }
            return _instance;
        }
    }
}
~~~

<span class="anchor" id="laziness"></span>

## Laziness

### Fully Lazy Instantiation

To be fully lazy instantiated, the implementation `Singleton5` uses a nested class in the singleton class.
Therefore, the instantiation is triggered the first time the nested class is referenced which could only occur in Instance.
This implementation is fully lazy and has better performance than previous implementations.
However, the code is a bit complicated, because the nested class (`Singleton5`) have access to the enclosing class's private members,
but the reverse it not true, so the `_instance` should be internal in nested class.

~~~ cs
// Implementation5: fully lazy using nested class
public sealed class Singleton5
{
    private Singleton5() {    }

    public static Singleton5 Instance
    {
        get
        {
            return Nested.instance;
        }
    }

    private class Nested
    {
        static Nested()
        {
        }
        // Make the instantiation
        internal static readonly Singleton5 instance = new Singleton5();
    }
}
~~~

### Using .NET4 Lazy<T> type
If you're using .NET 4 or higher, you can use the `System.Lazy<T>` type to make the laziness really simple.
As shown in the implementation Singleton6, all you need to do is pass a delegate to the constructor
which calls the Singleton constructor - which is done most easily with a lambda expression. It's simple and performs well.
Also, you can check whether the instance has been created with the `IsValueCreated` property.

~~~ cs
// Implementation6: fully lazy, .NET 4 or higher only
public sealed class Singleton6
{
    private static readonly Lazy<Singleton> _lazy =
        new Lazy<Singleton>(() => new Singleton6());

    private Singleton() {   }

    public static Singleton Instance
    {
        get
        {
            return _lazy.Value;
        }
    }

    public static bool IsInstanceCreated
    {
        return _lazy.IsValueCreated;
    }
}
~~~

<span class="anchor" id="performance"></span>

## Performance

### Need Lazy Instantiation?

In many cases, you won't actually require full laziness - unless your class initialization does something particularly time-consuming,
or has some side-effect elsewhere, it's probably fine to leave out the explicit static constructor shown above.

If your singleton instance is referenced within a relatively tight loop, this can make a (relatively) significant performance difference.
You should decide whether or not fully lazy instantiation is required, and document this decision appropriately within the class.

Instead of improving the instantiation of the singleton class, try to optimize your code using the singleton,
which is more helpful in the real-world application.

### Balanced Implementation

In the end, `Singleton6` is an extremely simple implementation.
The static constructors in C# are specified to execute only when an instance of the class is created or a static member is referenced,
and to execute only once per AppDomain. See the [referenced article](http://csharpindepth.com/articles/general/singleton.aspx#cctor) for more details.

~~~ cs
public sealed class Singleton6
{
    private static readonly Singleton6 _instance = new Singleton6();

    // Explicit static constructor to tell C# compiler not to mark type as beforefieldinit
    static Singleton6()
    {
    }

    private Singleton6()
    {
    }

    public static Singleton6 Instance
    {
        get
        {
            return _instance;
        }
    }
}
~~~

---

## References:

- [http://csharpindepth.com/articles/general/singleton.aspx](http://csharpindepth.com/articles/general/singleton.aspx)
- [http://msdn.microsoft.com/en-us/library/ff650316.aspx](http://msdn.microsoft.com/en-us/library/ff650316.aspx)
