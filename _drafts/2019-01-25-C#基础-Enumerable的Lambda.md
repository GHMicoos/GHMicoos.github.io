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
using System;

namespace EFCoreTest
{
    public interface MyEnumerable
    {

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
        /// 强制转换System.Collections.IEnumerable的元素到指定类型。
        /// </summary>
        /// <typeparam name="TResult">将源元素强制转换为的类型。</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <returns>一个泛型序列，该序列中的元素中是从源序列中的元素转换为特定的类型。</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidCastException">原序列中的某个元素不能转换为TResult</exception>
        IEnumerable<TResult> Cast<TResult>(this IEnumerable source);

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

        /// <summary>
        /// 返回序列中元素的个数
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理序列</param>
        /// <returns>序列中包含的元素个数</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.OverflowException">序列中包含的元素个数比Int32.MaxValue还大</exception>
        int Count<TSource>(this IEnumerable<TSource> source);

        /// <summary>
        /// 返回序列中满足条件的元素个数
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理序列</param>
        /// <param name="predicate">用于测试元素是否满足条件的函数</param>
        /// <returns>序列中满足条件的元素个数</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.OverflowException">序列中包含的元素个数比Int32.MaxValue还大</exception>
        int Count<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

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

        #region  数值序列计算（平均值，最大值，最小值）

        
        /// <summary>
        /// 计算“数字”序列的平均值
        /// </summary>
        /// <param name="source">待处理的序列</param>
        /// <returns>数字序列的平均值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidOperationException">souurce是empty</exception>
        double Average(this IEnumerable<int> source);
        double? Average(this IEnumerable<int?> source);
        double Average(this IEnumerable<long> source);
        double? Average(this IEnumerable<long?> source);
        float Average(this IEnumerable<float> source);
        float? Average(this IEnumerable<float?> source);
        decimal Average(this IEnumerable<decimal> source);
        decimal? Average(this IEnumerable<decimal?> source);
        double Average(this IEnumerable<double> source);
        double? Average(this IEnumerable<double?> source);

        /// <summary>
        /// 序列的每个元素通过转换函数获取数值，然后把这些数值求平均值。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">通过selector处理每个元素返回一个数字</param>
        /// <returns>序列通过转换函数，求取值得平均数</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或 selector==null</exception>
        /// <exception cref="T:System.InvalidOperationException">序列是空的</exception> 
        /// <exception cref="T:System.OverflowException">序列的length超过System.Int64.MaxValue</exception> 
        double Average<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);
        double? Average<TSource>(this IEnumerable<TSource> source, Func<TSource, int?> selector);
        double Average<TSource>(this IEnumerable<TSource> source, Func<TSource, long> selector);
        double? Average<TSource>(this IEnumerable<TSource> source, Func<TSource, long?> selector);
        float Average<TSource>(this IEnumerable<TSource> source, Func<TSource, float> selector);
        float? Average<TSource>(this IEnumerable<TSource> source, Func<TSource, float?> selector);
        decimal Average<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal> selector);
        decimal? Average<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal?> selector);
        double Average<TSource>(this IEnumerable<TSource> source, Func<TSource, double> selector);
        double? Average<TSource>(this IEnumerable<TSource> source, Func<TSource, double?> selector);



        /// <summary>
        /// 序列求和
        /// </summary>
        /// <param name="source">待处理的序列</param>
        /// <returns>序列求和</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.OverflowException">求和的结果大于Int32.MaxValue</exception>
        int Sum(this IEnumerable<int> source);
        int? Sum(this IEnumerable<int?> source);
        long Sum(this IEnumerable<long> source);
        long? Sum(this IEnumerable<long?> source);
        float Sum(this IEnumerable<float> source);
        float? Sum(this IEnumerable<float?> source);
        decimal Sum(this IEnumerable<decimal> source);
        decimal? Sum(this IEnumerable<decimal?> source);
        double Sum(this IEnumerable<double> source);
        double? Sum(this IEnumerable<double?> source);

        /// <summary>
        /// 序列的每个元素通过转换函数获取数值，然后把这些数值求和。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">转换函数</param>
        /// <returns>投影值求和</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 selector==null</exception>
        /// <exception cref="T:System.OverflowException">求和的结果大于Int32.MaxValue</exception>
        int Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);
        int? Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, int?> selector);
        long Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, long> selector);
        long? Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, long?> selector);
        float Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, float> selector);
        float? Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, float?> selector);
        decimal Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal> selector);
        decimal? Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal?> selector);
        double Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, double> selector);
        double? Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, double?> selector);



        /// <summary>
        /// 返回序列中的最大值
        /// </summary>
        /// <param name="source">待处理的序列</param>
        /// <returns>序列中的最大值
        /// 返回类型与序列类型一致，并支持一下类型：
        /// int,int?,long,long?,float,float?,decimal,decimal?,double,double?
        /// </returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        int Max(this IEnumerable<int> source);
        int? Max(this IEnumerable<int?> source);
        long Max(this IEnumerable<long> source);
        long? Max(this IEnumerable<long?> source);
        float Max(this IEnumerable<float> source);
        float? Max(this IEnumerable<float?> source);
        decimal Max(this IEnumerable<decimal> source);
        decimal? Max(this IEnumerable<decimal?> source);
        double Max(this IEnumerable<double> source);
        double? Max(this IEnumerable<double?> source);

        /// <summary>
        /// 在序列的每个元素上调用转换函数，并返回最大值。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">转换函数</param>
        /// <returns>序列中通过转换函数后的最大值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 selector=null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        int Max<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);
        int? Max<TSource>(this IEnumerable<TSource> source, Func<TSource, int?> selector);
        long Max<TSource>(this IEnumerable<TSource> source, Func<TSource, long> selector);
        long? Max<TSource>(this IEnumerable<TSource> source, Func<TSource, long?> selector);
        float Max<TSource>(this IEnumerable<TSource> source, Func<TSource, float> selector);
        float? Max<TSource>(this IEnumerable<TSource> source, Func<TSource, float?> selector);
        decimal Max<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal> selector);
        decimal? Max<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal?> selector);
        double Max<TSource>(this IEnumerable<TSource> source, Func<TSource, double> selector);
        double? Max<TSource>(this IEnumerable<TSource> source, Func<TSource, double?> selector);

        /// <summary>
        /// 返回序列中的最大值
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数,TSource必须实现IComparable接口</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <returns>序列中的最大值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        TSource Max<TSource>(this IEnumerable<TSource> source);

        /// <summary>
        /// 在泛型序列的每个元素调用转换函数，并返回转换出来的值中的最大值。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数,TSource必须实现IComparable接口</typeparam>
        /// <typeparam name="TResult">转换函数返回的类型</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">一个转换函数</param>
        /// <returns>序列中的最大值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 selector=null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        TResult Max<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);


        /// <summary>
        /// 返回序列中的最小值
        /// </summary>
        /// <param name="source">待处理的序列</param>
        /// <returns>序列中的最小值
        /// 返回类型与序列类型一致，并支持一下类型：
        /// int,int?,long,long?,float,float?,decimal,decimal?,double,double?
        /// </returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        int Min(this IEnumerable<int> source);
        int? Min(this IEnumerable<int?> source);
        long Min(this IEnumerable<long> source);
        long? Min(this IEnumerable<long?> source);
        float Min(this IEnumerable<float> source);
        float? Min(this IEnumerable<float?> source);
        decimal Min(this IEnumerable<decimal> source);
        decimal? Min(this IEnumerable<decimal?> source);
        double Min(this IEnumerable<double> source);
        double? Min(this IEnumerable<double?> source);

        /// <summary>
        /// 在序列的每个元素上调用转换函数，并返回最小值。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">转换函数</param>
        /// <returns>序列中通过转换函数后的最小值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 selector=null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        int Min<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);
        int? Min<TSource>(this IEnumerable<TSource> source, Func<TSource, int?> selector);
        long Min<TSource>(this IEnumerable<TSource> source, Func<TSource, long> selector);
        long? Min<TSource>(this IEnumerable<TSource> source, Func<TSource, long?> selector);
        float Min<TSource>(this IEnumerable<TSource> source, Func<TSource, float> selector);
        float? Min<TSource>(this IEnumerable<TSource> source, Func<TSource, float?> selector);
        decimal Min<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal> selector);
        decimal? Min<TSource>(this IEnumerable<TSource> source, Func<TSource, decimal?> selector);
        double Min<TSource>(this IEnumerable<TSource> source, Func<TSource, double> selector);
        double? Min<TSource>(this IEnumerable<TSource> source, Func<TSource, double?> selector);

        /// <summary>
        /// 返回序列中的最小值
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数,TSource必须实现IComparable接口</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <returns>序列中的最小值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        TSource Min<TSource>(this IEnumerable<TSource> source);

        /// <summary>
        /// 在泛型序列的每个元素调用转换函数，并返回转换出来的值中的最大值。
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数,TSource必须实现IComparable接口</typeparam>
        /// <typeparam name="TResult">转换函数返回的类型</typeparam>
        /// <param name="source">待处理的序列</param>
        /// <param name="selector">一个转换函数</param>
        /// <returns>序列中的最大值</returns>
        /// <exception cref="T:System.ArgumentNullException">source==null 或者 selector=null</exception>
        /// <exception cref="T:System.InvalidOperationException">source是empty</exception>
        TResult Min<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);




        #endregion

        #region 通过序列中的位置 返回值

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

        #endregion

        #region GroupBy

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


        #endregion

        #region 集合运算

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
        /// </summary>
        /// <typeparam name="TSource">泛型类型参数</typeparam>
        /// <param name="first">第一个序列</param>
        /// <param name="second">第二个序列</param>
        /// <param name="comparer">相等比较器</param>
        /// <returns>通过特殊相等比较器，返回集合R=A-B；即就是第一个序列中，去掉第二个序列中的元素。</returns>
        /// <exception cref="T:System.ArgumentNullException">first==null 或者 second==null</exception>
        IEnumerable<TSource> Except<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);

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

        #endregion 
    }
}

```