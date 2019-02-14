---
layout: post
title:  "Lambda-Join"
categories: C#基础-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`Join`,`GroupJoin`。






### 零 概述
**“链接”包括`Join`,`GroupJoin`。`Join`：通过一个公共键值联接两个集合，类似于SQL中的内部联接（inner join）。`GroupJoin`:根据键的相等性将两个序列的元素关联起来，并对结果进行分组,类似于SQL中的左外连接类似。**



### 一 链接(Join,GroupJoin)

#### **1.Join**
* 基于匹配键关联两个序列的元素。Join：通过一个公共键值联接两个集合，类似于SQL中的内部联接（inner join）。
* Sample:

``` js

class Person
{
    public string Name { get; set; }
}

class Pet
{
    public string Name { get; set; }
    public Person Owner { get; set; }
}

public void Join_()
{


    Person magnus = new Person { Name = "Hedlund, Magnus" };
    Person terry = new Person { Name = "Adams, Terry" };
    Person charlotte = new Person { Name = "Weiss, Charlotte" };

    Pet barley = new Pet { Name = "Barley", Owner = terry };
    Pet boots = new Pet { Name = "Boots", Owner = terry };
    Pet whiskers = new Pet { Name = "Whiskers", Owner = charlotte };
    Pet daisy = new Pet { Name = "Daisy", Owner = magnus };

    List<Person> people = new List<Person> { magnus, terry, charlotte };
    List<Pet> pets = new List<Pet> { barley, boots, whiskers, daisy };

    // Create a list of Person-Pet pairs where 
    // each element is an anonymous type that contains a
    // Pet's name and the name of the Person that owns the Pet.
    var query =people.Join(pets,
            person => person,
            pet => pet.Owner,
            (person, pet) =>
                new { OwnerName = person.Name, Pet = pet.Name }
            ).ToList();

    
}

/*
 This code produces the following output:

 Hedlund, Magnus - Daisy
 Adams, Terry - Barley
 Adams, Terry - Boots
 Weiss, Charlotte - Whiskers
*/

```


* 方法签名：

``` js

/// <summary>
/// 基于匹配键关联两个序列的元素。
/// 通过“默认的相等比较器”比较键。
/// Join：通过一个公共键值联接两个集合，类似于SQL中的内部联接（inner join）。
/// </summary>
/// <typeparam name="TOuter">第一个序列的元素类型</typeparam>
/// <typeparam name="TInner">第二个序列的元素类型</typeparam>
/// <typeparam name="TKey">键选择器函数返回的键的类型。</typeparam>
/// <typeparam name="TResult">结果类型</typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从两个匹配的元素创建一个结果元素。</param>
/// <returns>通过对两个序列执行内连接而获得的IEnumerable<TResult></returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner, TResult> resultSelector);


/// <summary>
/// 基于匹配键关联两个序列的元素。
/// 通过“特定的相等比较器”比较键。
/// </summary>
/// <typeparam name="TOuter">第一个序列的元素类型</typeparam>
/// <typeparam name="TInner">第二个序列的元素类型</typeparam>
/// <typeparam name="TKey">键选择器函数返回的键的类型</typeparam>
/// <typeparam name="TResult">结果类型</typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从两个匹配的元素创建一个结果元素。</param>
/// <param name="comparer">特定的相等比较器</param>
/// <returns>通过对两个序列执行内连接而获得的IEnumerable</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner, TResult> resultSelector, IEqualityComparer<TKey> comparer);

```


#### **2.GroupJoin**
* 根据键的相等性将两个序列的元素关联起来，并对结果进行分组。类似于SQL中的左外连接类似。

* Sample：

``` js

public static void GroupJoinEx1()
{
    Person magnus = new Person { Name = "Hedlund, Magnus" };
    Person terry = new Person { Name = "Adams, Terry" };
    Person charlotte = new Person { Name = "Weiss, Charlotte" };

    Pet barley = new Pet { Name = "Barley", Owner = terry };
    Pet boots = new Pet { Name = "Boots", Owner = terry };
    Pet whiskers = new Pet { Name = "Whiskers", Owner = charlotte };
    Pet daisy = new Pet { Name = "Daisy", Owner = magnus };

    List<Person> people = new List<Person> { magnus, terry, charlotte };
    List<Pet> pets = new List<Pet> { barley, boots, whiskers, daisy };

    // Create a list where each element is an anonymous 
    // type that contains a person's name and 
    // a collection of names of the pets they own.
    var query =
        people.GroupJoin(pets,
                         person => person,
                         pet => pet.Owner,
                         (person, petCollection) =>
                             new
                             {
                                 OwnerName = person.Name,
                                 Pets = petCollection.Select(pet => pet.Name)
                             });

    foreach (var obj in query)
    {
        // Output the owner's name.
        Console.WriteLine("{0}:", obj.OwnerName);
        // Output each of the owner's pet's names.
        foreach (string pet in obj.Pets)
        {
            Console.WriteLine("  {0}", pet);
        }
    }
}

/*
 This code produces the following output:

 Hedlund, Magnus:
   Daisy
 Adams, Terry:
   Barley
   Boots
 Weiss, Charlotte:
   Whiskers
*/

```


* 方法签名：

``` js

/// <summary>
/// 根据键的相等性将两个序列的元素关联起来，并对结果进行分组。
/// “默认的相等比较器”用于比较键。
/// SQL中的左外连接类似。
/// </summary>
/// <typeparam name="TOuter"></typeparam>
/// <typeparam name="TInner"></typeparam>
/// <typeparam name="TKey"></typeparam>
/// <typeparam name="TResult"></typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从第一个序列中的元素创建结果元素，并从第二个序列中创建匹配元素的集合。</param>
/// <returns>IEnumerable<TResult>，TResult是通过对两个序列执行分组连接而获得的。</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, IEnumerable<TInner>, TResult> resultSelector);

/// <summary>
/// 根据键的相等性将两个序列的元素关联起来，并对结果进行分组。
/// “默认的相等比较器”用于比较键。
/// SQL中的左外连接类似。
/// </summary>
/// <typeparam name="TOuter"></typeparam>
/// <typeparam name="TInner"></typeparam>
/// <typeparam name="TKey"></typeparam>
/// <typeparam name="TResult"></typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从第一个序列中的元素创建结果元素，并从第二个序列中创建匹配元素的集合。</param>
/// <param name="comparer"></param>
/// <returns>IEnumerable<TResult>，TResult是通过对两个序列执行分组连接而获得的。</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, IEnumerable<TInner>, TResult> resultSelector, IEqualityComparer<TKey> comparer);



```

