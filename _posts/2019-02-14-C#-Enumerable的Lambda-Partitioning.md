---
layout: post
title:  "Lambda-Partitioning"
categories: C#-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

概述：包括`Concat`,`SequenceEqual`,`Zip`,`Append`。






### 零 概述
**“分区”包括`Skip`,`SkipLast`,`SkipWhile`,`Take`,`TakeLast`,`TakeWhile`。使用容易，看方法注释，不在赘述。**

### 一 分区
#### **1.方法签名**

``` js

/// <summary>
/// 绕过序列中指定数量的元素，然后返回其余的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="count">在返回其余元素之前要跳过的元素数。</param>
/// <returns>结果包含输入序列中指定索引之后出现的元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> Skip<TSource>(this IEnumerable<TSource> source, int count);

/// <summary>
/// 绕过序列中指定数量的元素，然后返回其余的元素。(由后往前数)
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="count">在返回其余元素之前要跳过的元素数</param>
/// <returns>结果包含输入序列中指定索引之后出现的元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> SkipLast<TSource>(this IEnumerable<TSource> source, int count);

/// <summary>
/// 只要指定的条件为真，就会绕过序列中的元素，然后返回其余的元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数。</param>
/// <returns>source序列中没有通过predicate函数的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> SkipWhile<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 只要指定的条件为真，就会绕过序列中的元素，然后返回其余的元素。谓词函数的逻辑中使用元素索引。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个源元素的条件的函数;函数的第二个参数表示源元素的索引。</param>
/// <returns>source序列中没有通过predicate函数的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> SkipWhile<TSource>(this IEnumerable<TSource> source, Func<TSource, int, bool> predicate);

/// <summary>
/// 返回从序列的开始处指定数量的连续元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="count">返回的元素数。</param>
/// <returns>返回source前count个元素</returns>
/// <exception cref="T:System.ArgumentNullException">source is null.</exception>
IEnumerable<TSource> Take<TSource>(this IEnumerable<TSource> source, int count);

/// <summary>
/// 返回从序列的结尾处指定数量的连续元素。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="count">返回的元素数</param>
/// <returns>返回source后count个元素</returns>
/// <exception cref="T:System.ArgumentNullException">source is null.</exception>
IEnumerable<TSource> TakeLast<TSource>(this IEnumerable<TSource> source, int count);

/// <summary>
/// 返回序列中的元素，只要指定的条件为真。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数。</param>
/// <returns>返回source中通过predicate测试为true的元素</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> TakeWhile<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 返回序列中的元素，只要指定的条件为真。谓词函数的逻辑中使用元素索引。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数</param>
/// <returns>返回source中通过predicate测试为true的元素</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> TakeWhile<TSource>(this IEnumerable<TSource> source, Func<TSource, int, bool> predicate);


```