---
layout: post
title:  "Lambda-Quantifiers"
categories: C#-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

概述：包括`All`,`Any`,`Contains`。






### 零 概述
**“量词”包括`All`,`Any`,`Contains`。**

### 一 量词(All,Any,Contains)
#### **1.All**
* 确定序列的所有元素是否都满足条件。这里要注意的是空序列返回true。
* 方法签名：

``` js

/// <summary>
/// 确定序列的所有元素是否都满足条件。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数</param>
/// <returns>序列中的每个元素调用predicate后都返回true，或者序列是空的（未包含任何元素），则返回true。否则返回false</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate==null </exception>
bool All<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

```

#### **2.Any**
* 确定序列中是否包含（或满足条件的）任意一个元素。这里需要注意的是空序列返回false。

* 方法签名：

``` js

/// <summary>
/// 确定序列是否包含任何元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>序列包含任何元素，则返回true；否则返回false。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
bool Any<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 确定序列中的任何元素是否满足条件。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数</param>
/// <returns>序列不能为空，且序列中的任一元素调用predicate后都返回true，则返回true。否则返回false</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate==null </exception>
bool Any<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

```


#### **3.Contains**
* 使用默认的（或者特定的）比较器，确定序列是否包含特定的元素

* 方法签名：

``` js

/// <summary>
/// 使用默认的比较器，确定序列是否包含特定的元素
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="value">特确定元素</param>
/// <returns>序列中包含特定的元素返回true，否则返回false。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
bool Contains<TSource>(this IEnumerable<TSource> source, TSource value);

/// <summary>
/// 使用特定的比较器，确定序列是否包含特定的元素
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="value">特确定元素</param>
/// <param name="comparer">用于比较值的相等比较器。</param>
/// <returns>序列中包含特定的元素返回true，否则返回false。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
bool Contains<TSource>(this IEnumerable<TSource> source, TSource value, IEqualityComparer<TSource> comparer);

```