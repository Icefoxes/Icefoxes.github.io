---
layout:       post
title:        "AOP Implement in .NET"
subtitle:     "AOP Implement in .NET"
date:         2015-06-04 19:45:00
author:       "Kuo"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - AOP
---

> In computing, aspect-oriented programming (AOP) is a programming paradigm that aims to increase modularity by allowing the separation of cross-cutting concerns.

In Java's world, AOP is a very popular concept and [AspectJ](https://www.eclipse.org/aspectj/) is kind of de facto standards while [PostSharp]('https://www.postsharp.net/') is the de facto standards in .NET's world. However PostSharp is not free and heavy, so a lot of devleopers like me are seeking for framework. I came across a simple and clean one today [MrAdvice](https://github.com/ArxOne/MrAdvice)

## The minimal sample from its homepage
```CSharp
public class MyProudAdvice : Attribute, IMethodAdvice
{
    public void Advise(MethodAdviceContext context)
    {
        // do things you want here
        context.Proceed(); // this calls the original method
        // do other things here
    }
}
```

then just need to mark the method(s) with the attribute and that's it, your aspect is injected!
```CSharp
[MyProudAdvice]
public void MyProudMethod()
{
}
```
I tested on my pc, it works. A simple and clear AOP framework just gets less than 200 stars！ I just have a research on this framework to know how it works. Here's the source code overview. `MrAdvice.Weaver` is an executable and add a build task into target project. `MrAdvice` take advantage of [dnlib](https://github.com/0xd4d/dnlib) to Reads and writes .NET assemblies and modules, so the injection happens at compile time.
```
-----
    |
    |-----MrAdvice
    |
    |-----MrAdvice.Weaver
    |
    |-----Platform Specific
 ```

