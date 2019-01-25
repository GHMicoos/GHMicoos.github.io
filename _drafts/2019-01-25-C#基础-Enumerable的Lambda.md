---
layout: post
title:  "C#基础-Lambda"
categories: C#基础
tags: C#基础 Enumerable lambda  
author: GHMicoos
---

* content
{:toc}

暂无




### 二 All Any
#### **1.All**
``` js
///

/// <summary>
        /// 确定序列的所有元素是否都满足条件。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="predicate">用于测试每个元素的条件的函数</param>
        /// <returns>序列中的每个元素调用predicate后都返回true，或者序列是空的（未包含任何元素），则返回true。否则返回false</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 predicate==null </exception>
        bool All<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

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

        /// <summary>
        /// 把传入的element插入到序列末尾
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="element">带插入元素</param>
        /// <returns>返回一个新的序列</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        IEnumerable<TSource> Append<TSource>(this IEnumerable<TSource> source, TSource element);

        /// <summary>
        /// 计算“数字”序列的平均值
        /// 序列的元素类型可以是：int int? long long? float float? decimal decimal? double double?
        /// 注意：Average有多个重载方法
        /// </summary>
        /// <param name="source">待处理的序列</param>
        /// <returns>
        /// 数字序列的平均值
        /// 序列类型 => 返回类型： 
        /// float/float? => float/float?
        /// decimal/decimal? => decimal/decimal?
        /// int/int? long/long? double/double? => double/double?
        /// </returns>
        double Average(this IEnumerable<int> source);

        /// <summary>
        /// 计算序列的平均值
        /// 通过selector处理每个元素返回一个数字，然后 数字累加结果/元素个数
        /// 注意：Average有多个重载方法
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">通过selector处理每个元素返回一个数字</param>
        /// <returns>
        /// 返回参数 取决于selector的返回类型
        /// 序列类型 => 返回类型： 
        /// float/float? => float/float?
        /// decimal/decimal? => decimal/decimal?
        /// int/int? long/long? double/double? => double/double?
        /// </returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或 selector==null</exception>
        /// <exception cref="T:System.InvalidOperationException">序列是空的</exception> 
        /// <exception cref="T:System.OverflowException">序列的length超过System.Int64.MaxValue</exception> 
        double Average<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);
```