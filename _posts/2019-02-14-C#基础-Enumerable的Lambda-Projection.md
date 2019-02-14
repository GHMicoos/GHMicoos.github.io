---
layout: post
title:  "Lambda-Projection"
categories: C#基础-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`Select`,`SelectMany`。






### 零 概述
**“投影”包括`Select`,`SelectMany`。`Select`主要作用是讲序列中的每个元素投射到新表单中。`SelectMany`主要作用将source中的每个元素投射到IEnumerable<TResult>,将得到的序列压扁成一个序列。 将集合扁平化为单个集合(类似于SQL中的交叉连接)**

### 一 投影(Select,SelectMany)
#### **1.Select**
* 该方法组是将序列中的每个元素（可包括序列）投影到新的表单中。
* Sample：

``` js

public void Select_Select()
{
    var list = new List<string>() {"一","二","三" } as IEnumerable<string>;

    //{Name="一"}，{Name="二"}，{Name="三"}
    var select = list.Select(x=>new { Name=x}).ToList();

    //{Id=0,Name="一"}，{Id=1,Name="二"}，{Id=2,Name="三"}
    var selectIndex = list.Select((x, index) =>
    {
        return new { Id = index, Name = x };
    }).ToList();
}

```

* 方法签名：

``` js

/// <summary>
/// 将序列中的每个元素投射到新表单中。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的值的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个元素的转换函数。</param>
/// <returns>IEnumerable<TResult>,其元素是对源的每个元素调用transform函数的结果</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);

/// <summary>
/// 通过合并元素索引，将序列中的每个元素投射到一个新表单中。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的值的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个源元素的转换函数;函数的第二个参数表示源元素的索引。。</param>
/// <returns>IEnumerable<TResult>,其元素是对源的每个元素调用transform函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, int, TResult> selector);

```


#### **2.SelectMany**
* 该组方法是将序列的每个元素投影到 IEnumerable<T> 并将结果序列合并为一个序列。
* Sample：

``` js

public void Select_SelectMany()
{
    PetOwner[] petOwners =
    {
        new PetOwner { Name="Higa",Pets = new List<string>{ "Scruffy", "Sam" } },
        new PetOwner { Name="Ashkenazi",Pets = new List<string>{ "Walker", "Sugar" } },
        new PetOwner { Name="Price",Pets = new List<string>{ "Scratches", "Diesel" } },
        new PetOwner { Name="Hines", Pets = new List<string>{ "Dusty" } }
    };

    // Project the pet owner's name and the pet's name.
    var query = petOwners
        .SelectMany(petOwner => petOwner.Pets, (petOwner, petName) => new { petOwner.Name, PetName=petName })
        .ToList();
    // {Name="Higa",PetName="Scruffy"}
    // {Name="Higa",PetName="Sam"}
    // {Name="Ashkenazi",PetName="Walker"}
    // {Name="Ashkenazi",PetName="Sugar"}
    // {Name="Price",PetName="Scratches"}
    // {Name="Price",PetName="Diesel"}
    // {Name="Hines",PetName="Dusty"}
}

```

* 方法签名：

``` js

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将得到的序列压扁成一个序列。
///  将集合扁平化为单个集合(类似于SQL中的交叉连接)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个元素的转换函数。</param>
/// <returns>返回结果的元素是对输入序列的每个元素调用一对多转换函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, IEnumerable<TResult>> selector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将得到的序列压扁成一个序列。投射函数有索引参数。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个源元素的转换函数;函数的第二个参数表示源元素的索引。</param>
/// <returns>返回结果的元素是对输入序列的每个元素调用一对多转换函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, int, IEnumerable<TResult>> selector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将结果序列扁平化为一个序列，并对其中的每个元素调用结果选择器函数。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TCollection">由collectionSelector收集的中间元素的类型。</typeparam>
/// <typeparam name="TResult">结果序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="collectionSelector">用于输入序列的每个元素的转换函数。</param>
/// <param name="resultSelector">一个变换函数，应用于中间序列的每个元素。</param>
/// <returns>返回结果的元素是在源元素上调用一对多转换函数collectionSelector的中间序列，然后将这些中间序列元素及其对应的源元素映射到结果元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or collectionSelector or resultSelector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IEnumerable<TSource> source, Func<TSource, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将结果序列扁平化为一个序列，并对其中的每个元素调用结果选择器函数。每个源元素的索引以该元素的中间投影形式使用。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TCollection">由collectionSelector收集的中间元素的类型。</typeparam>
/// <typeparam name="TResult">结果序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="collectionSelector">用于输入序列的每个元素的转换函数。</param>
/// <param name="resultSelector">一个变换函数，应用于中间序列的每个元素。</param>
/// <returns>返回结果的元素是在源元素上调用一对多转换函数collectionSelector的中间序列，然后将这些中间序列元素及其对应的源元素映射到结果元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or collectionSelector or resultSelector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IEnumerable<TSource> source, Func<TSource, int, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);


```