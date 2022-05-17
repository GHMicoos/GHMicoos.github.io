---
layout: post
title:  "Lambda-Restriction"
categories: C#-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

概述：包括`Where`。






### 零 概述
**“限制”包括`Where`。**

### 一 限制(Where)
#### **1.All**
* 基于函数（函数签名或包括序列索引）筛选值序列。
* 方法签名：

``` js

/// <summary>
/// 基于谓词筛选值序列。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个元素的条件的函数。</param>
/// <returns>结果包含来自满足条件的输入序列的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> Where<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 基于谓词筛选值序列。谓词函数的逻辑中使用每个元素的索引。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="predicate">用于测试每个源元素的条件的函数;函数的第二个参数表示源元素的索引。</param>
/// <returns>结果包含来自满足条件的输入序列的元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
IEnumerable<TSource> Where<TSource>(this IEnumerable<TSource> source, Func<TSource, int, bool> predicate);

```

