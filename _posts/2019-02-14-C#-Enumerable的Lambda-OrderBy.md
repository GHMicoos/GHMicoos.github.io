---
layout: post
title:  "Lambda-Orderby"
categories: C#-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`OrderBy`,`OrderByDescending`,`ThenBy`,`ThenByDescending`，`Reverse`。






### 零 概述
**“排序”表示对集合进行排序。它包括`OrderBy`,`OrderByDescending`,`ThenBy`,`ThenByDescending`，`Reverse`。使用容易，看方法注释，不在赘述。**

### 一 排序
#### **1.方法签名**

``` js

//不难看出OrderBy的方法重载是两两分组的，后一个只比前一个多一个comparer。

/// <summary>
/// 根据键将序列中的元素按升序排序。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数。</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> OrderBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 使用指定的比较器按升序对序列中的元素进行排序。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数。</param>
/// <param name="comparer">键比较函数。</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> OrderBy<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey> comparer);

/// <summary>
/// 根据键将序列中的元素按降序排序。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> OrderByDescending<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 使用指定的比较器按降序对序列中的元素进行排序。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <param name="comparer">键比较函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> OrderByDescending<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey> comparer);

/// <summary>
/// 根据键按升序对序列中的元素执行后续排序。(也是升序)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> ThenBy<TSource, TKey>(this IOrderedEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 使用指定的比较器按升序对序列中的元素执行后续排序。(也是升序)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <param name="comparer">键比较函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> ThenBy<TSource, TKey>(this IOrderedEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey> comparer);

/// <summary>
/// 根据键按降序对序列中的元素执行后续排序。(也是降序)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> ThenByDescending<TSource, TKey>(this IOrderedEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 使用指定的比较器按降序对序列中的元素执行后续排序。(也是降序)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从元素中提取键的函数</param>
/// <param name="comparer">键比较函数</param>
/// <returns>IOrderedEnumerable<TSource>的元素是根据键排序的。</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
IOrderedEnumerable<TSource> ThenByDescending<TSource, TKey>(this IOrderedEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey> comparer);

/// <summary>
/// 反转序列中元素的顺序。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>一个序列，其元素与输入序列的元素以相反的顺序对应。</returns>
/// <exception cref="T:System.ArgumentNullException">source is null.</exception>
IEnumerable<TSource> Reverse<TSource>(this IEnumerable<TSource> source);


```