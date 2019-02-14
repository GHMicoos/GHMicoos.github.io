---
layout: post
title:  "C#基础-Lambda-Generation"
categories: C#基础
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

暂无


### 概述
**“生成”表示生成或者通过某个序列生成新的序列，它包括`DefaultIfEmpty`,`Empty`,`Range`,`Repeat`。使用容易，看方法注释，不在赘述。**

### 一 元素
#### **1.方法签名**

``` js

/// <summary>
/// 如果sourc不为empty，返回该序列中的元素组成的序列。
/// 如果souce为empty，则返回一个序列该序列只包含一个元素(TSource的默认值)。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>如果sourc不为empty，返回该序列中的元素组成的序列。
/// 如果souce为empty，则返回一个序列该序列只包含一个元素(TSource的默认值)。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> DefaultIfEmpty<TSource>(this IEnumerable<TSource> source);

/// <summary>
///  如果sourc不为empty，返回该序列中的元素组成的序列。
/// 如果souce为empty，则返回一个序列该序列只包含一个元素(defaultValue)。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="defaultValue">当sourct是empty时，返回的序列中的唯一值</param>
/// <returns>如果sourc不为empty，返回该序列中的元素组成的序列。
/// 如果souce为empty，则返回一个序列该序列只包含一个元素(defaultValue)。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> DefaultIfEmpty<TSource>(this IEnumerable<TSource> source, TSource defaultValue);

/// <summary>
/// 返回一个empty的指定泛型类型的序列
/// </summary>
/// <typeparam name="TResult">泛型类型参数</typeparam>
/// <returns> 返回一个empty的指定泛型类型的序列</returns>
IEnumerable<TResult> Empty<TResult>();

/// <summary>
/// 生成指定范围内的整数序列。
/// </summary>
/// <param name="start">生成序列的第一个值</param>
/// <param name="count">生成序列元素个数</param>
/// <returns>IEnumerable<int>从start开始直到start+count-1组成的序列</returns>
/// <exception cref="T:System.ArgumentOutOfRangeException">count<0 or start+count-1比Int32.MaxValue还大</exception>
IEnumerable<int> Range(int start, int count);

/// <summary>
/// 生成包含一个重复值的序列。
/// </summary>
/// <typeparam name="TResult">泛型类型参数</typeparam>
/// <param name="element">重复元素</param>
/// <param name="count">重复次数</param>
/// <returns>包含一个重复值的序列。</returns>
/// <exception cref="T:System.ArgumentOutOfRangeException">count<0</exception>
IEnumerable<TResult> Repeat<TResult>(TResult element, int count);

```