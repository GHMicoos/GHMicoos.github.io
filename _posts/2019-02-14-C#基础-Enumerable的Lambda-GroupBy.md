---
layout: post
title:  "Lambda-GroupBy"
categories: C#基础-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`GroupBy`。






### 零 概述
**“分组”表示按键将集合的元素投射到组中。它包括`GroupBy`。使用容易，看方法注释，不在赘述。**

### 一 元素
#### **1.方法签名**

``` js

//不难看出Group的方法重载是两两分组的，后一个只比前一个多一个comparer。

/// <summary>
/// 根据指定的“键选择器函数”对序列的元素进行分组。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">键选择器函数</param>
/// <returns>一个序列IEnumerable<IGrouping<TKey, TSource>>，每个元素包含了一个key和一个序列的TSource</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 keySelector==null</exception>
IEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 根据指定的“键选择器函数”对序列进行分组，并且通过指定的“比较器”对键进行比较
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">键选择器函数</param>
/// <param name="comparer">键比较器</param>
/// <returns>一个序列IEnumerable<IGrouping<TKey, TSource>>，每个元素包含了一个key和一个序列的TSource</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 keySelector==null
/// 。备注如果comparer==null是不抛出异常的</exception>
IEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey> comparer);

/// <summary>
/// 根据指定的“键选择器函数”对序列的元素进行分组,
/// 并使用指定的函数为每个组投射元素。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TElement">方法返回结果IGrouping中的第二个类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="elementSelector">一个函数，用于把每个源元素映射到TElement。</param>
/// <returns>一个序列IEnumerable<IGrouping<TKey, TElement>>，每个元素包含了一个key和一个序列的TElement</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or keySelector=null </exception>
IEnumerable<IGrouping<TKey, TElement>> GroupBy<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector);

/// <summary>
/// 根据指定的“键选择器函数”对序列的元素进行分组,
/// 并且通过指定的“比较器”对键进行比较
/// 并使用指定的函数为每个组投射元素。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TElement">方法返回结果IGrouping中的第二个类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="elementSelector">一个函数，用于把每个源元素映射到TElement。</param>
/// <param name="comparer">键比较器</param>
/// <returns>一个序列IEnumerable<IGrouping<TKey, TElement>>，每个元素包含了一个key和一个序列的TElement</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or keySelector=null </exception>
IEnumerable<IGrouping<TKey, TElement>> GroupBy<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey> comparer);


/// <summary>
/// 根据指定的键选择器函数对序列的元素进行分组,
/// 并从每个组及其键创建结果值。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TResult">结果选择器返回的结果类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="resultSelector">一个函数，用于从每个组创建结果值。</param>
/// <returns>TResult类型的元素集合，其中每个元素表示一个组和它的键通过resultSelector的投影。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or keySelector=null or resultSelector=null</exception>
IEnumerable<TResult> GroupBy<TSource, TKey, TResult>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TKey, IEnumerable<TSource>, TResult> resultSelector);

/// <summary>
/// 根据指定的键选择器函数对序列的元素进行分组,
/// 并且通过指定的“比较器”对键进行比较，
/// 并从每个组及其键创建结果值。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TResult">结果选择器返回的结果类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="resultSelector">一个函数，用于从每个组和key创建结果值。</param>
/// <param name="comparer">键比较器</param>
/// <returns>TResult类型的元素集合，其中每个元素表示一个组和它的键通过resultSelector的投影。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or keySelector=null or resultSelector=null </exception>
IEnumerable<TResult> GroupBy<TSource, TKey, TResult>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TKey, IEnumerable<TSource>, TResult> resultSelector, IEqualityComparer<TKey> comparer);

/// <summary>
/// 根据指定的键选择器函数对序列的元素进行分组，
/// 并且通过指定的“比较器”对键进行比较，
/// 并使用指定的函数为每个组投射元素，
/// 并从每个组及其键创建结果值。
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TElement">元素选择器的返回类型</typeparam>
/// <typeparam name="TResult">resultSelector结果选择器返回的结果类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="elementSelector">一个函数，用于把每个源元素映射到TElement。</param>
/// <param name="resultSelector">一个函数，用于从每个组和key创建结果值。</param>
/// <returns>TResult类型的元素集合，其中每个元素表示一个组和它的键通过resultSelector的投影。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or keySelector=null or elementSelector or resultSelector=null </exception>
IEnumerable<TResult> GroupBy<TSource, TKey, TElement, TResult>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, Func<TKey, IEnumerable<TElement>, TResult> resultSelector);

/// <summary>
/// 根据指定的键选择器函数对序列的元素进行分组
/// 并使用指定的函数为每个组投射元素。
/// 并从每个组及其键创建结果值
/// </summary>
/// <typeparam name="TSource">source序列中的元素类型</typeparam>
/// <typeparam name="TKey">键选择函数返回的类型</typeparam>
/// <typeparam name="TElement">元素选择器的返回类型</typeparam>
/// <typeparam name="TResult">resultSelector结果选择器返回的结果类型</typeparam>
/// <param name="source">待分组序列</param>
/// <param name="keySelector">一个函数，用于提取每个元素的键。</param>
/// <param name="elementSelector">一个函数，用于把每个源元素映射到TElement。</param>
/// <param name="resultSelector">一个函数，用于从每个组和key创建结果值。</param>
/// <param name="comparer">键比较器</param>
/// <returns>TResult类型的元素集合，其中每个元素表示一个组和它的键通过resultSelector的投影。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or elementSelector==null or keySelector=null or resultSelector=null </exception>
IEnumerable<TResult> GroupBy<TSource, TKey, TElement, TResult>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, Func<TKey, IEnumerable<TElement>, TResult> resultSelector, IEqualityComparer<TKey> comparer);


```