---
layout:       post
title:        "Abstract Class In C#"
subtitle:     "Abstract Class In C#"
date:         2016-04-25 12:00:00
author:       "Kuo"
header-img:   "img/post-bg-android.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - .NET
---
#### Abstract Class

1. Abstract class can contains abstract method and non-abstract method, and abstract method cannot be private. Meanwhile, `sealed` can not be declared with `abstract`
```C#
public abstract class Wheel
{
    [error]abstract void Go();

    public void Fix()
    {
        Console.WriteLine("This Wheel is Fixed");
    }
}
```
2. class derives from abstract class have to implement all abstract method, if derived class is declared as abstract, then it can avoid implementing all the abstract methods
```C#
public class ConcreteWheel : Wheel
{
    public override void Go()
    {
        throw new NotImplementedException();
    }
}
```
3. abstract class can inherit non-abstract class
```C#
public abstract class Wheel: ConcreteWheel
{
    abstract public void Go();

    public void Fix()
    {
        Console.WriteLine("This Wheel is Fixed");
    }
}

public class ConcreteWheel 
{
}
```