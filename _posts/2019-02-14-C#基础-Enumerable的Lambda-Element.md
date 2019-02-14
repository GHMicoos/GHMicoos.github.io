---
layout: post
title:  "C#基础-Lambda-Element"
categories: C#基础
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

暂无


### 概述
**“元素”表示获取某个位置的元素，它包括`ElementAt`,`ElementAtOrDefault`,`First`,`FirstOrDefault`,`Last`,`LastOrDefault`,`Single`,`SingleOrDefault`。使用比较容易，看方法注释，不在赘述。**

### 一 元素
#### **1.方法签名**

``` js

/// <summary>
/// 返回序列中指定位置（index）的元素
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="index">位置（序号）</param>
/// <returns>返回序列中指定位置（index）的元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.ArgumentOutOfRangeException">index<0 或者 index>=source.Length</exception>
TSource ElementAt<TSource>(this IEnumerable<TSource> source, int index);

/// <summary>
/// 返回序列中指定位置（index）的元素,或者如果index超出序列范围，则返回默认值
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="index">位置（序号）</param>
/// <returns>返回序列中指定位置（index）的元素,或者如果index超出序列范围，则返回默认值</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
TSource ElementAtOrDefault<TSource>(this IEnumerable<TSource> source, int index);

/// <summary>
/// 返回序列的第一个值
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回序列中的第一个值</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidOperationException">source是empty</exception>
TSource First<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中第一个满足指定条件的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">测试序列每个元素的方法</param>
/// <returns>返回序列中第一个满足指定条件的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate=null</exception>
/// <exception cref="T:System.InvalidOperationException">source是empty；或者序列中没有满足条件的元素</exception>
TSource First<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 返回序列的第一个值,或者当序列是empty时，返回TSource默认值。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回序列的第一个值,或者当序列是empty时，返回TSource默认值</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
TSource FirstOrDefault<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中第一个满足条件的值。或者，当序列是empty与序列中没有满足条件的值时，返回TSource默认值。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">测试序列每个元素的方法</param>
/// <returns>返回序列中第一个满足条件的值。或者，当序列是empty与序列中没有满足条件的值时，返回TSource默认值。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate=null</exception>
TSource FirstOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 返回序列中的最后一个元素
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回序列中的最后一个元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidOperationException">source是empty</exception>
TSource Last<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中最后一个满足指定条件的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">测试序列每个元素的方法</param>
/// <returns>返回序列中最后一个满足指定条件的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate=null</exception>
/// <exception cref="T:System.InvalidOperationException">source是empty；或者序列中没有满足条件的元素</exception>
TSource Last<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);


/// <summary>
/// 返回序列的最后一个值,或者当序列是empty时，返回TSource默认值。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回序列的最后一个值,或者当序列是empty时，返回TSource默认值。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
TSource LastOrDefault<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中最后一个满足条件的值。或者，当序列是empty与序列中没有满足条件的值时，返回TSource默认值。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">测试序列每个元素的方法</param>
/// <returns>返回序列中最后一个满足条件的值。或者，当序列是empty与序列中没有满足条件的值时，返回TSource默认值。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate=null</exception>
TSource LastOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 返回序列中唯一的元素，如果序列中没有确切的元素，则抛出异常。
/// </summary>
/// <typeparam name="TSource">序列中的元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>序列的单个元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidOperationException">序列是空的，或者序列的元素数量超过1个 </exception>
TSource Single<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中唯一满足指定条件的元素，如果存在多个这样的元素，则抛出异常。
/// </summary>
/// <typeparam name="TSource">序列中的元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">用于测试一个元素的条件的函数。</param>
/// <returns>序列中满足条件的单个元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null or predicate</exception>
/// <exception cref="T:System.InvalidOperationException">序列是空的，或者序列中满足条件的元素数量超过1个或者为0个 </exception>
TSource Single<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
///  返回序列中唯一的元素，如果序列中没有确切的元素，则返回默认值。如果序列中元素数量超过一个，那么抛出异常.
/// </summary>
/// <typeparam name="TSource">序列中的元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回序列中唯一的元素，如果序列中没有确切的元素，则返回默认值。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidOperationException">序列的元素数量超过1个 </exception>
TSource SingleOrDefault<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 返回序列中满足条件的唯一元素；或者返回默认值，如果序列没有满足条件的元素。
/// 如果满足条件的元素多于一个，则会抛出异常。
/// </summary>
/// <typeparam name="TSource">序列中的元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">用于测试一个元素的条件的函数。</param>
/// <returns></returns>
/// <exception cref="T:System.ArgumentNullException">source==null or predicate</exception>
/// <exception cref="T:System.InvalidOperationException">序列中满足条件的元素数量超过1个 </exception>
TSource SingleOrDefault<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

```