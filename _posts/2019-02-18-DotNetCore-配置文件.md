---
layout: post
title:  ".Net Core-配置文件"
categories: .Net Core
tags: .NetCore 配置文件
author: GHMicoos
---


* content
{:toc}

包括：`Distinct`,`Except`,`Intersect`,`Union`。






### 零 概述
**复元素。**

``` js

public void Set_()
{
    var listOne = new List<int>() { 1, 1, 2, 2, 3, 3, 4, 4 };
    var listTwo = new List<int>() { 3, 3, 4, 4, 5, 5, 6, 6 };

    //1,2,3,4
    var distinct = listOne.Distinct().ToList();

    //1,2
    var except = listOne.Except(listTwo).ToList();

    //3,4
    var intersect = listOne.Intersect(listTwo).ToList();

    //1,2,3,4,5,6
    var union = listOne.Union(listTwo).ToList();
}

```

### 一 集合(Distinct,Except,Intersect,Union)

#### **1.Distinct**
* 通过默认的（或者特定的）“相等比较器”，从序列中返回唯一的元素组成的序列（从序列中去掉重复的元素）。
* 方法签名：

``` js

/// <summary>
/// 通过默认的“相等比较器”，从序列中返回唯一的元素组成的序列（从序列中去掉重复的元素）。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待移除重复的序列</param>
/// <returns>从序列中返回唯一的元素组成的序列</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> Distinct<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 通过特定的“相等比较器”，从序列中返回唯一的元素组成的序列（从序列中去掉重复的元素）。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待移除重复的序列</param>
/// <param name="comparer">特定的“相等比较器”</param>
/// <returns>从序列中返回唯一的元素组成的序列（从序列中去掉重复的元素）。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> Distinct<TSource>(this IEnumerable<TSource> source, IEqualityComparer<TSource> comparer);

```

#### **2.Except**
* 使用默认（或者特定的）“相等比较器”生成两个序列的差集。即就是第一个序列中，去掉第二个序列中的元素。
* 方法签名：

``` js

/// <summary>
/// 使用默认相等比较器生成两个序列的差集。即就是第一个序列中，去掉第二个序列中的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <returns>返回集合R=A-B；即就是第一个序列中，去掉第二个序列中的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TSource> Except<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 使用特殊的相等比较器生成两个序列的差集。即就是第一个序列中，去掉第二个序列中的元素。
/// 从一个集合中删除存在于另一个集合中的所有元素,并且第一个序列中去掉重复元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <param name="comparer">相等比较器</param>
/// <returns>通过特殊相等比较器，返回集合R=A-B；即就是第一个序列中，去掉第二个序列中的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TSource> Except<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);

```

#### **3.Intersect**
* 使用默认的（或特定的）“相等比较器”生成两个序列的交集。即就是第一个序列与第二个序列都包含的元素。
* 方法签名：

``` js

/// <summary>
/// 使用默认的相等比较器生成两个序列的交集。即就是第一个序列与第二个序列都包含的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <returns>包含构成两个序列的集合交集的元素的序列。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TSource> Intersect<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 使用特殊的相等比较器生成两个序列的交集。即就是第一个序列与第二个序列都包含的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <param name="comparer">相等比较器</param>
/// <returns>包含构成两个序列的集合交集的元素的序列。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TSource> Intersect<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);

```

#### **4.Union**
* 使用默认的（或特定的）“相等比较器”生成两个序列的并集。即就是组合两个集合并删除重复的元素。
* 方法签名：

``` js

/// <summary>
/// 使用默认的相等比较器生成两个序列的集合并集。即就是组合两个集合并删除重复的元素。
/// </summary>
/// <typeparam name="TSource">序列的元素类型</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <returns>包含来自两个序列的元素，不包括重复项。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null or second==null</exception>
IEnumerable<TSource> Union<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 使用“特定的相等比较器”生成两个序列的集合并集。即就是组合两个集合并删除重复的元素。
/// </summary>
/// <typeparam name="TSource">序列的元素类型</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <param name="comparer"></param>
/// <returns>包含来自两个序列的元素，不包括重复项。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null or second==null</exception>
IEnumerable<TSource> Union<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);

```

