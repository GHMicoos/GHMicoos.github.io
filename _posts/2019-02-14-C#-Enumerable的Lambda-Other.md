---
layout: post
title:  "Lambda-Other"
categories: C#-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

概述：包括`Concat`,`SequenceEqual`,`Zip`,`Append`。






### 零 概述
**“其他”包括`Concat`(链接两个序列),`SequenceEqual`(比较两个序列是否相等),`Zip`(通过两个序列生成一个新的序列)，`Append`(序列的尾部添加新的元素)。**

### 一 “其他”

### **1.Zip**
* 签名：`IEnumerable<TResult> Zip<TFirst, TSecond, TResult>(this IEnumerable<TFirst> first, IEnumerable<TSecond> second, Func<TFirst, TSecond, TResult> resultSelector);`
* 这里提供一个Zip的Sample：

``` js

public void Other_Zip()
{
    var listOne = new List<int>() { 1, 2, 3 } as IEnumerable<int>;
    var listTwo = new List<int>() { 1,2,3,4,5} as IEnumerable<int>;

    var zip = listOne.Zip(listTwo, (one, two) => {
        return one + two;
    }).ToList();//1,2,3 表示只会处理最短序列长度的元素，多与的元素不处理
}

```



#### **2.方法签名**

``` js

/// <summary>
/// 链接两个序列
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列，该序列将链接到第一个序列后面</param>
/// <returns>返回一个新的序列，该序列包含了第一个序列和第二序列的元素</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TSource> Concat<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 通过对元素的类型使用默认的相等比较器来比较两个序列是否相等。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <returns>如果两个源序列的长度相等，且根据其类型的默认相等比较器，它们对应的元素相等，则为true;否则,false。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
bool SequenceEqual<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 通过指定的comparer比较两个序列的元素，确定它们是否相等。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <param name="comparer">元素比较器</param>
/// <returns>如果两个源序列长度相等，且对应的元素根据comparer比较相等，则为true;否则,false。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
bool SequenceEqual<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);

/// <summary>
/// 将指定的函数应用于两个序列的对应元素，生成结果序列。
/// </summary>
/// <typeparam name="TFirst"></typeparam>
/// <typeparam name="TSecond"></typeparam>
/// <typeparam name="TResult"></typeparam>
/// <param name="first">要合并的第一个序列。</param>
/// <param name="second">要合并的第二个序列。</param>
/// <param name="resultSelector">一个函数，用于指定如何合并两个序列中的元素。</param>
/// <returns>包含两个输入序列的合并元素的序列。只会处理最短序列长度的元素，多与的元素不处理</returns>
/// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
IEnumerable<TResult> Zip<TFirst, TSecond, TResult>(this IEnumerable<TFirst> first, IEnumerable<TSecond> second, Func<TFirst, TSecond, TResult> resultSelector);

/// <summary>
/// 把传入的element插入到序列末尾
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="element">带插入元素</param>
/// <returns>返回一个新的序列</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TSource> Append<TSource>(this IEnumerable<TSource> source, TSource element);


```