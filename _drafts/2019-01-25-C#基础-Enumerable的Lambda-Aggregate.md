---
layout: post
title:  "C#基础-Lambda-Aggregate"
categories: C#基础
tags: C#基础 Enumerable lambda  
author: GHMicoos
---

* content
{:toc}

暂无



**概述：统计部分包括以下几个方面，一般统计(Aggregate)、计数(Count,LongCount)、特殊统计(Sum、Average、Max、Min)。**

### 一 计数(Count,LongCount)
#### **1.计数的含义**
* 该组方法返回序列“包含”的元素个数，其中“包含”可以是一般意义的包括，也可以是满足某个条件的后的元素个数。返回值类型可以是int/long。该组方法的四个重载形式如下：

``` js

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
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
/// <exception cref="T:System.OverflowException">序列中满足条件的元素个数比Int32.MaxValue还大</exception>
int Count<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

/// <summary>
/// 返回序列中元素的个数。返回值类型为Int64。当序列元素总数大于Int32时候，无法使用Count,但可使用LongCount
/// </summary>
/// <typeparam name="TSource">序列元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>序列中元素的数量。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.OverflowException">序列中包含的元素个数比Int64.MaxValue还大</exception>
long LongCount<TSource>(this IEnumerable<TSource> source);


/// <summary>
/// 返回序列中满足条件的元素个数。返回值类型为Int64。当序列元素总数大于Int32时候，无法使用Count,但可使用LongCount
/// </summary>
/// <typeparam name="TSource">序列元素类型</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="predicate">用于测试元素是否满足条件的函数</param>
/// <returns>序列中满足条件的元素个数</returns>
/// <exception cref="T:System.ArgumentNullException">source or predicate is null.</exception>
/// <exception cref="T:System.OverflowException">序列中满足条件的元素个数比Int32.MaxValue还大</exception>
long LongCount<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);

```

* 简单计数：`int Count<TSource>(this IEnumerable<TSource> source);`和`long LongCount<TSource>(this IEnumerable<TSource> source);`。
* 复杂计数： `int Count<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);`和`long LongCount<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);`。

#### **2.简单计数**
* 返回序列的元素个数，Sample如下。

``` js
public void  Aggregate_Count_Simple()
{
    var list = new List<int>() { 2, 4, 1, 5, 2, 1, 9, 55, 78 };
    var nullList = default(List<int>);
    //Int32.MaxValue 边界情况不好测试，一般不会用大那么大的序列。这里不测试
    var greaterInt32List = Enumerable.Range(0, Int32.MaxValue);
    var bbb = greaterInt32List.Append(100);
    //var bbbCount = bbb.LongCount();

    try
    {
        var nullCount = nullList.Count();
    }
    catch (ArgumentNullException e)
    {
        //会进入断点
        Console.WriteLine(e.ToString());
    }

    //一般计数
    var count = list.Count();
}

```

#### **3.复杂计数**
* 返回序列中满足predicate函数的元素个数，sample如下：

``` js

public void Arrageate_Count_Complex()
{
    var list = new List<string>() { "李相赫","姿态","司马老贼","厂长"};

    //测试正常情况
    //序列中元素的长度大于等于3的元素个数
    var count=list.Count(x => x.Length >= 3);
    //count=2
    Console.WriteLine($"count={count}");

    //测试predicate==null
    try
    {
        var predicateCount=list.Count(null);
    }
    catch(Exception e)
    {
        //System.ArgumentNullException: Value cannot be null.
        //Parameter name: predicate
        Console.WriteLine(e.ToString());
    }
}

```

### 二 特殊统计(Sum、Average、Max、Min)
#### **1.含义**
* 该组方法返回序列（数字序列，int、long、decimal、float、double）中的特殊统计，包括Sun(求和)、Average(平均值)、Max(最大值)、Min(最小值)：



``` js


#region Aggregate(Aggregate未完成,Average,Count,LongCoung,Max,Min,Sum)

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
/// <typeparam name="TSource">泛型类型参数,TResult必须实现IComparable接口</typeparam>
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


/// <summary>
/// 在序列上应用累加器函数。
/// </summary>
/// <typeparam name="TSource">枚举类型参数</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="func">要在每个元素上调用的累加器函数。</param>
/// <returns>最后的累加器值。</returns>
/// <exception cref="T:System.ArgumentNullException">source or func is null.</exception>
/// <exception cref="T:System.InvalidOperationException"> source contains no elements.</exception>
TSource Aggregate<TSource>(this IEnumerable<TSource> source, Func<TSource, TSource, TSource> func);

/// <summary>
/// 在序列上应用累加器函数。指定的种子值用作初始累加器值。
/// </summary>
/// <typeparam name="TSource">源元素的类型。</typeparam>
/// <typeparam name="TAccumulate">累加器值的类型。</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="seed">初始累加器值。</param>
/// <param name="func">要在每个元素上调用的累加器函数。</param>
/// <returns>最后的累加器值。</returns>
/// <exception cref="T:System.ArgumentNullException">source or func is null.</exception>
TAccumulate Aggregate<TSource, TAccumulate>(this IEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func);

/// <summary>
/// 在序列上应用累加器函数。使用指定的种子值作为初始累加器值，使用指定的函数选择结果值。
/// </summary>
/// <typeparam name="TSource">源元素的类型</typeparam>
/// <typeparam name="TAccumulate">累加器值的类型</typeparam>
/// <typeparam name="TResult">结果值的类型。</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="seed">初始累加器值</param>
/// <param name="func">要在每个元素上调用的累加器函数</param>
/// <param name="resultSelector">将最终累加器值转换为结果值的函数。</param>
/// <returns>转换后的最终累加器值。</returns>
/// <exception cref="T:System.ArgumentNullException">source or func or resultSelector is null.</exception>
TResult Aggregate<TSource, TAccumulate, TResult>(this IEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func, Func<TAccumulate, TResult> resultSelector);



#endregion

#region Conversion(AdEnumerable,Cast,OfType,ToArray,ToDictionary,ToList,ToLookup)

/// <summary>
/// 强制转换为IEnumerable<TSource>
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>强制转换为IEnumerable<TSource></returns>
IEnumerable<TSource> AsEnumerable<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 强制转换source到指定泛型类型的IEnumerable<TResult>
/// </summary>
/// <typeparam name="TResult">将源元素强制转换为的类型。</typeparam>
/// <param name="source">包含要转换为类型TResult的元素的IEnumerable。</param>
/// <returns>结果包含的元素为源序列中的每个元素转换为指定类型的结果元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidCastException">序列中的元素不能强制转换为类型TResult。</exception>
IEnumerable<TResult> Cast<TResult>(this IEnumerable source);

/// <summary>
/// 基于TResult类型筛选source中的元素
/// </summary>
/// <typeparam name="TResult">用于筛选序列上的元素的类型。</typeparam>
/// <param name="source">要过滤其元素的IEnumerable。</param>
/// <returns>IEnumerable<TResult>包含source中的TResult类型元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TResult> OfType<TResult>(this IEnumerable source);

/// <summary>
/// 从source 创建数组
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>包含输入序列元素的数组。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
TSource[] ToArray<TSource>(this IEnumerable<TSource> source);


/// <summary>
/// 创建字典,根据指定的键选择器函数
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <returns>包含键和值的Dictionary</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null. -or- keySelector produces a key that is null.</exception>
/// <exception cref="T:System.ArgumentException">keySelector为两个元素生成重复的键。</exception>
Dictionary<TKey, TSource> ToDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);

/// <summary>
/// 创建字典,根据指定的键选择器函数和键比较器
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <param name="comparer">比较键值的函数</param>
/// <returns>包含键和值的Dictionary</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null. -or- keySelector produces a key that is null.</exception>
/// <exception cref="T:System.ArgumentException">keySelector为两个元素生成重复的键。</exception>
Dictionary<TKey, TSource> ToDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey> comparer);


/// <summary>
/// 创建字典,根据指定的键选择器函数和键比较器
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <typeparam name="TElement">elementSelector返回的值的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <param name="elementSelector">转换函数，用于从每个元素生成结果元素值。</param>
/// <returns>包含键和值的Dictionary</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector or elementSelector is null.
/// -or- keySelector produces a key that is null.</exception>
/// <exception cref="T:System.ArgumentException">keySelector为两个元素生成重复的键。</exception>
Dictionary<TKey, TElement> ToDictionary<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector);


/// <summary>
/// 创建字典,根据指定的键选择器函数和键比较器
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <typeparam name="TElement">elementSelector返回的值的类型</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <param name="elementSelector">转换函数，用于从每个元素生成结果元素值。</param>
/// <param name="comparer">比较键值的函数</param>
/// <returns>包含键和值的Dictionary</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector or elementSelector is null.
/// -or- keySelector produces a key that is null.</exception>
/// <exception cref="T:System.ArgumentException">keySelector为两个元素生成重复的键。</exception>
Dictionary<TKey, TElement> ToDictionary<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey> comparer);


/// <summary>
/// 将集合转换为按键分组的查找。
/// </summary>
/// <typeparam name="TSource">泛型类型</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型。</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <returns>键和值得查找</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
ILookup<TKey, TSource> ToLookup<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);


/// <summary>
/// 将集合转换为按键分组的查找。使用特定的键比较器。
/// </summary>
/// <typeparam name="TSource">泛型类型</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="keySelector">从每个元素中提取键的函数</param>
/// <param name="comparer">键相等比较器</param>
/// <returns>键和值得查找</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector is null.</exception>
ILookup<TKey, TSource> ToLookup<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey> comparer);


/// <summary>
/// 根据指定的键选择器和元素选择器函数将集合转换为按键分组的查找
/// </summary>
/// <typeparam name="TSource">泛型类型</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <typeparam name="TElement">elementSelector返回的值的类型。</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="keySelector">从每个元素中提取键的函数。</param>
/// <param name="elementSelector">转换函数，用于从每个元素生成结果元素值。</param>
/// <returns>键和值得查找</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector or elementSelector is null.</exception>
ILookup<TKey, TElement> ToLookup<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector);


/// <summary>
/// 根据指定的键选择器函数、比较器和元素选择器函数 将集合转换为按键分组的查找
/// </summary>
/// <typeparam name="TSource">泛型类型</typeparam>
/// <typeparam name="TKey">键选择器返回的键的类型</typeparam>
/// <typeparam name="TElement">elementSelector返回的值的类型。</typeparam>
/// <param name="source">待处理序列</param>
/// <param name="keySelector">从每个元素中提取键的函数</param>
/// <param name="elementSelector">转换函数，用于从每个元素生成结果元素值。</param>
/// <param name="comparer">键相等比较器</param>
/// <returns>键和值得查找</returns>
/// <exception cref="T:System.ArgumentNullException">source or keySelector or elementSelector is null.</exception>
ILookup<TKey, TElement> ToLookup<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey> comparer);

#endregion

#region Element(ElementAt,ElementAtOrDefault,First,FirstOrDefault,Last,LastOrDefault,Single,SingleOrDefault)

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

#endregion

#region Generation(DefaultIfEmpty,Empty,Range,Repeat)

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

#region Join

/// <summary>
/// 基于匹配键关联两个序列的元素。
/// 通过“默认的相等比较器”比较键。
/// Join：通过一个公共键值联接两个集合，类似于SQL中的内部联接（inner join）。
/// </summary>
/// <typeparam name="TOuter">第一个序列的元素类型</typeparam>
/// <typeparam name="TInner">第二个序列的元素类型</typeparam>
/// <typeparam name="TKey">键选择器函数返回的键的类型。</typeparam>
/// <typeparam name="TResult">结果类型</typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从两个匹配的元素创建一个结果元素。</param>
/// <returns>通过对两个序列执行内连接而获得的IEnumerable<TResult></returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner, TResult> resultSelector);


/// <summary>
/// 基于匹配键关联两个序列的元素。
/// 通过“特定的相等比较器”比较键。
/// </summary>
/// <typeparam name="TOuter">第一个序列的元素类型</typeparam>
/// <typeparam name="TInner">第二个序列的元素类型</typeparam>
/// <typeparam name="TKey">键选择器函数返回的键的类型</typeparam>
/// <typeparam name="TResult">结果类型</typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从两个匹配的元素创建一个结果元素。</param>
/// <param name="comparer">特定的相等比较器</param>
/// <returns>通过对两个序列执行内连接而获得的IEnumerable</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner, TResult> resultSelector, IEqualityComparer<TKey> comparer);


/// <summary>
/// 根据键的相等性将两个序列的元素关联起来，并对结果进行分组。
/// “默认的相等比较器”用于比较键。
/// SQL中的左外连接类似。
/// </summary>
/// <typeparam name="TOuter"></typeparam>
/// <typeparam name="TInner"></typeparam>
/// <typeparam name="TKey"></typeparam>
/// <typeparam name="TResult"></typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从第一个序列中的元素创建结果元素，并从第二个序列中创建匹配元素的集合。</param>
/// <param name="comparer"></param>
/// <returns>IEnumerable<TResult>，TResult是通过对两个序列执行分组连接而获得的。</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, IEnumerable<TInner>, TResult> resultSelector, IEqualityComparer<TKey> comparer);

/// <summary>
/// 根据键的相等性将两个序列的元素关联起来，并对结果进行分组。
/// “默认的相等比较器”用于比较键。
/// SQL中的左外连接类似。
/// </summary>
/// <typeparam name="TOuter"></typeparam>
/// <typeparam name="TInner"></typeparam>
/// <typeparam name="TKey"></typeparam>
/// <typeparam name="TResult"></typeparam>
/// <param name="outer">连接的第一个序列</param>
/// <param name="inner">连接第一个序列的序列</param>
/// <param name="outerKeySelector">一个函数，用于从第一个序列的每个元素中提取联接键。</param>
/// <param name="innerKeySelector">一个函数，用于从第二个序列的每个元素中提取联接键。</param>
/// <param name="resultSelector">一个函数，用于从第一个序列中的元素创建结果元素，并从第二个序列中创建匹配元素的集合。</param>
/// <returns>IEnumerable<TResult>，TResult是通过对两个序列执行分组连接而获得的。</returns>
/// <exception cref="T:System.ArgumentNullException">outer,inner,outerKeySelector,innerKeySelector,resultSelector 其中一个为null</exception>
IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IEnumerable<TOuter> outer, IEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, IEnumerable<TInner>, TResult> resultSelector);

#endregion

#region Ordering(OrderBy,OrderByDescending,Reverse,ThenBy,ThenByDescending)

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

#endregion

#region Other(Concat,SequenceEqual,Zip，Append)

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
/// <returns>包含两个输入序列的合并元素的序列</returns>
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

#endregion

#region Partitioning(Skip,SkipWhile,Take,TakeWhile)

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

#endregion

#region Projection(Select,SelectMany)

/// <summary>
/// 将序列中的每个元素投射到新表单中。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的值的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个元素的转换函数。</param>
/// <returns>IEnumerable<TResult>,其元素是对源的每个元素调用transform函数的结果</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, TResult> selector);

/// <summary>
/// 通过合并元素索引，将序列中的每个元素投射到一个新表单中。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的值的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个源元素的转换函数;函数的第二个参数表示源元素的索引。。</param>
/// <returns>IEnumerable<TResult>,其元素是对源的每个元素调用transform函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> Select<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, int, TResult> selector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将得到的序列压扁成一个序列。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个元素的转换函数。</param>
/// <returns>返回结果的元素是对输入序列的每个元素调用一对多转换函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, IEnumerable<TResult>> selector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将得到的序列压扁成一个序列。投射函数有索引参数。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TResult">选择器返回的序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="selector">用于每个源元素的转换函数;函数的第二个参数表示源元素的索引。</param>
/// <returns>返回结果的元素是对输入序列的每个元素调用一对多转换函数的结果。</returns>
/// <exception cref="T:System.ArgumentNullException">source or selector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TResult>(this IEnumerable<TSource> source, Func<TSource, int, IEnumerable<TResult>> selector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将结果序列扁平化为一个序列，并对其中的每个元素调用结果选择器函数。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TCollection">由collectionSelector收集的中间元素的类型。</typeparam>
/// <typeparam name="TResult">结果序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="collectionSelector">用于输入序列的每个元素的转换函数。</param>
/// <param name="resultSelector">一个变换函数，应用于中间序列的每个元素。</param>
/// <returns>返回结果的元素是在源元素上调用一对多转换函数collectionSelector的中间序列，然后将这些中间序列元素及其对应的源元素映射到结果元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or collectionSelector or resultSelector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IEnumerable<TSource> source, Func<TSource, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);

/// <summary>
/// 将source中的每个元素投射到IEnumerable<TResult>,将结果序列扁平化为一个序列，并对其中的每个元素调用结果选择器函数。每个源元素的索引以该元素的中间投影形式使用。
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <typeparam name="TCollection">由collectionSelector收集的中间元素的类型。</typeparam>
/// <typeparam name="TResult">结果序列元素的类型。</typeparam>
/// <param name="source">待处理的序列</param>
/// <param name="collectionSelector">用于输入序列的每个元素的转换函数。</param>
/// <param name="resultSelector">一个变换函数，应用于中间序列的每个元素。</param>
/// <returns>返回结果的元素是在源元素上调用一对多转换函数collectionSelector的中间序列，然后将这些中间序列元素及其对应的源元素映射到结果元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source or collectionSelector or resultSelector is null.</exception>
IEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IEnumerable<TSource> source, Func<TSource, int, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);

#endregion

#region Quantifiers(量词：All,Any,Contains)

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

#endregion 

#region Restriction(where)

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

#endregion

#region Set(集合运算)

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
/// 从一个集合中删除存在于另一个集合中的所有元素,并且第一个序列中去掉重复元素。
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

/// <summary>
/// 使用默认的相等比较器生成两个序列的集合并集。即就是组合两个集合并删除重复的元素。
/// </summary>
/// <typeparam name="TSource">序列的元素类型</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <returns>包含来自两个序列的元素，不包括重复项。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null or second==null</exception>
IEnumerable<TSource> Union<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second);

/// <summary>
/// 使用“特定的相等比较器”生成两个序列的集合并集。即就是组合两个集合并删除重复的元素。
/// </summary>
/// <typeparam name="TSource">序列的元素类型</typeparam>
/// <param name="first">第一个序列</param>
/// <param name="second">第二个序列</param>
/// <param name="comparer"></param>
/// <returns>包含来自两个序列的元素，不包括重复项。</returns>
/// <exception cref="T:System.ArgumentNullException">first==null or second==null</exception>
IEnumerable<TSource> Union<TSource>(this IEnumerable<TSource> first, IEnumerable<TSource> second, IEqualityComparer<TSource> comparer);


#endregion




```