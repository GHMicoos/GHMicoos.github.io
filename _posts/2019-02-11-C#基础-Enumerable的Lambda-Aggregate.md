---
layout: post
title:  "LinqToObject-Lambda-Aggregate"
categories: C#基础-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`Aggregate`,`Count`,`LongCount`,`Sum`,`Average`,`Max`,`Min`。





### 零 概述
**“统计部分”包括`Aggregate`,`Count`,`LongCount`,`Sum`,`Average`,`Max`,`Min`。以下几个方面阐述，一般统计(Aggregate)、计数(Count,LongCount)、特殊统计(Sum、Average、Max、Min)。**

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
* 该组方法返回序列（数字序列，int、long、decimal、float、double；或者一般序列，该序列通过转换函数转换序列中的元素为数值）中的特殊统计，包括Sun(求和)、Average(平均值)、Max(最大值)、Min(最小值)：

#### **2.Sum(求和)**
* 该方法组可以求和两种序列，一种是“数值序列”，使用`int Sum(this IEnumerable<int> source);`形式签名；一种是“一般序列”，但是该“一般序列”中的元素可以通过“指定的函数”转换成“数值”，使用`int Sum<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);`的签名。
* 关于求和方法组的各种方法重载，详细参考签名。

``` js

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

```

* 以下是Sample：

``` js

 public void Aggregate_Max()
{
    #region Simple

    var list = new List<int>() { -1,1,2,2,3,3,4,4} as IEnumerable<int>;
    var sumBiggerInt32List = new List<int>() { Int32.MaxValue,Int32.MaxValue} as IEnumerable<int>;

    // 测试正常情况
    try
    {
        //不会引发异常
        var sum = list.Sum();
    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }


    //测试 求和的结果大于Int32.MaxValue，引发异常情况
    try
    {
        //引发OverflowException异常
        var sum = sumBiggerInt32List.Sum();
    }
    catch (OverflowException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

    #endregion

    #region Complex

    var complexList = new List<string>() { "12","0","2356","",null } as IEnumerable<string>;

    //测试正常情况
    try
    {
        var sum = complexList.Sum<string>(x => {
            if (string.IsNullOrEmpty(x)) return 0;
            else return x.Length;
        });
    }
    catch(Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

    //测试因selector==null， 引发异常情况
    try
    {
        var sum = complexList.Sum(null);
            }
    catch (ArgumentNullException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
            }
            catch (Exception e)
            {
        //不进入
        Console.WriteLine(e.ToString());
            }

    //测试 因求和结果大于Int32.MaxValue,引发异常情况
    try
    {
        var sum = complexList.Sum(x => Int32.MaxValue);
    }
    catch (OverflowException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

    #endregion
}

```

#### **3.Average(平均值)**
* 该方法组可以求取序列平均值，数值序列可以直接求平均值，使用`double Average(this IEnumerable<int> source)`形式签名；其他序列可以通过转换函数转换序列中元素为数值，然后求平均，使用`double Average<TSource>(this IEnumerable<TSource> source, Func<TSource, int> selector);`形式签名。

* 平均值方法组与求和方法组类似，这里不在写Sample，但是仍需要注意的是，当序列为空的时候会引发`InvalidOperationException`异常（理解，没有元素求平均值时候分母是0，不合理的数学运算）。

* 方法组签名：

``` js

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

```

### ** 4.Max(最大值)、Min(最小值)
* 该方法组可以求取序列的最大值、最小值。序列可以是“数值序列”，“一般序列”（通过转换函数把序列中元素转换成数值，然后求取最大值、最小值），“元素实现IComparable接口序列”。
* 对于“数值序列”与“一般序列”，与求取序列的平均值一样，这里不详细赘述。但仍需注意的是，如果序列是空的也会出发`InvalidOperationException`异常（理解，没有元素就不能说那个最大，那个最小）。
* “元素实现IComparable接口序列”，方法签名形如：`TSource Max<TSource>(this IEnumerable<TSource> source);`;其实“数值序列”就是个特例，因序列中元素是数值，实现了`IComparable`接口。


* 方法组签名：

``` js

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

```


### 三 一般统计(Aggregate)
#### **1.含义**
* 对集合中的每个元素执行指定的操作，同时将结果向前传送。如`TSource Aggregate<TSource>(this IEnumerable<TSource> source, Func<TSource, TSource, TSource> func);`签名中，参数`func`中第一个`TSource`表示前面已处理元素产生的值，第二个`TSource`表示序列当前值，第三个`TSource`表示`func`返回结果，用于下一次调用的第一个参数。

#### **2.Sample代码**
* 下面是Sample：

``` js

//TSource Aggregate<TSource>(this IEnumerable<TSource> source, Func<TSource, TSource, TSource> func);
public void Aggregate_Aggregate_Simple()
{
    //使用Aggregate实现累加
    var list = new List<int>() { -1, 1, 2, 3, 4, 5 } as IEnumerable<int>;

    //测试正常情况
    try
    {
        var aggregateSum = list.Aggregate((temp, x) => temp + x);
        if (aggregateSum == list.Sum())
            //进入
            Console.WriteLine($"aggregate求和与Sum求和相等。");
        else
            Console.WriteLine($"aggregate求和与Sum求和相等。");

    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

    //测试因 func is null 引发异常的情况
    try
    {
        var aggregateSum = list.Aggregate(null);
    }
    catch (ArgumentNullException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

    //测试因 source是空的，引发异常的情况
    try
    {
        var aggregate = Enumerable.Empty<int>().Aggregate((temp, x) => temp + x);
    }
    catch (InvalidOperationException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进入
        Console.WriteLine(e.ToString());
    }

}

```

``` js

//TAccumulate Aggregate<TSource, TAccumulate>(this IEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func);
public void Aggregate_Aggregate_Seed()
{
    var list = new List<string>() { "1","22","333","4444","55555",null} as IEnumerable<string>;

    //测试正常情况
    var aggregateSum = list.Aggregate<string, int>(0, (temp, x) => {
        var len = x == null ? 0 : x.Length;
        return temp + len;
    });
}

```

``` js
//TResult Aggregate<TSource, TAccumulate, TResult>(this IEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func, Func<TAccumulate, TResult> resultSelector);
public void Aggregate_Aggregate_Seed_Complex()
{
    var list = new List<string>() { "1", "22", "333", "4444", "55555", null } as IEnumerable<string>;

    //测试正常情况,统计list中的元素总长度，然后返回总数为长度，字符串单元为"总数字符串",组成的字符串
    //（如总长度为3，那么结为“333”）。
    var aggregateSum = list.Aggregate<string,int, string>(0
        , (temp, x) => {
            var len = x == null ? 0 : x.Length;
            return temp + len;
        }
        , x => {
            var result = string.Empty;
            if (x >0)
            {
                var str = x.ToString();
                for (int i = 1; i <= x; i++)
                {
                    result += str;
                }
            }
            return result;
        }
    );
}

```

* 方法签名：

``` js

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

```

