---
layout: post
title:  "LinqToObject-Lambda-Conversion"
categories: C#基础-LinqToObject
tags: Enumerable Lambda LinqToObject
author: GHMicoos
---

* content
{:toc}

包括：`AsEnumerable`、`Cast`、`OfType`、`ToArray`、`ToList`、`ToDictionary`、`ToLookup`。






### 零 概述
**“转换部分”包括以下几个方面，`AsEnumerable`、`Cast`、`OfType`、`ToArray`、`ToList`、`ToDictionary`、`ToLookup`。下面分为两组来讨论,“一般转换”包括`AsEnumerable`、`Cast`、`OfType`；“特例转换”包括`ToArray`、`ToList`、`ToDictionary`、`ToLookup`。**

### 一 一般转换(AsEnumerable,Cast,OfType)
#### **1.AsEnumerable**
* 该方法的作用：当自定义序列对类实现了与Enumerable扩展方法中的相同签名，但是在某种情况下需要隐藏自己的实现，转为使用Enumerable扩展方法中的方法，可使用这个方法。
* 详细可看[CSDN](https://docs.microsoft.com/zh-cn/dotnet/api/system.linq.enumerable.asenumerable?view=netcore-2.2)
``` js

// Custom class.
class Clump<T> : List<T>
{
    // Custom implementation of Where().
    public IEnumerable<T> Where(Func<T, bool> predicate)
    {
        Console.WriteLine("In Clump's implementation of Where().");
        return Enumerable.Where(this, predicate);
    }
}

static void AsEnumerableEx1()
{
    // Create a new Clump<T> object.
    Clump<string> fruitClump =
        new Clump<string> { "apple", "passionfruit", "banana", 
            "mango", "orange", "blueberry", "grape", "strawberry" };

    // First call to Where():
    // Call Clump's Where() method with a predicate.
    IEnumerable<string> query1 =fruitClump.Where(fruit => fruit.Contains("o"));

    Console.WriteLine("query1 has been created.\n");

    // Second call to Where():
    // First call AsEnumerable() to hide Clump's Where() method and thereby
    // force System.Linq.Enumerable's Where() method to be called.
    IEnumerable<string> query2 =fruitClump.AsEnumerable().Where(fruit => fruit.Contains("o"));

    // Display the output.
    Console.WriteLine("query2 has been created.");
}

// This code produces the following output:
//
// In Clump's implementation of Where().
// query1 has been created.
//
// query2 has been created.

```

* 方法签名：

``` js

/// <summary>
/// 强制转换为IEnumerable<TSource>
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>强制转换为IEnumerable<TSource></returns>
IEnumerable<TSource> AsEnumerable<TSource>(this IEnumerable<TSource> source);

```

#### **2.Cast**
* 该方法的作用：把`IEnumerable`强制转换为`IEnumerable<TResult>`
* Sample:

``` js

 public void Conversion_Cast()
{
    var list = new List<int>() { 1, 2, 3 } as IEnumerable;
    var hasCanotConvertList = new List<object>() {1,2,3,null } as IEnumerable;

    //测试正常情况
    var intList = list.Cast<int>().ToList();

    //测试因为 >序列中的元素不能强制转换为类型TResult，引发的InvalidCastException异常
    try
    {
        var longList = list.Cast<long>().ToList<long>();
    }
    catch (InvalidCastException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进
        Console.WriteLine(e.ToString());
    }
}

```

* 方法签名：

``` js

/// <summary>
/// 强制转换source到指定泛型类型的IEnumerable<TResult>
/// </summary>
/// <typeparam name="TResult">将源元素强制转换为的类型。</typeparam>
/// <param name="source">包含要转换为类型TResult的元素的IEnumerable。</param>
/// <returns>结果包含的元素为源序列中的每个元素转换为指定类型的结果元素</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
/// <exception cref="T:System.InvalidCastException">序列中的元素不能强制转换为类型TResult。</exception>
IEnumerable<TResult> Cast<TResult>(this IEnumerable source);

```

#### **3.OfType**
* 该方法的作用：基于TResult类型筛选source中的元素
* Sample:

``` js

public void Conversion_OfType()
{
    var hasCanotConvertList = new List<object>() { 1, 2, 3, null } as IEnumerable;

    //测试正常情况
    var ofTypeList = hasCanotConvertList.OfType<int>().ToList();
    //1,2,3
}

```

* 方法签名：

``` js

/// <summary>
/// 基于TResult类型筛选source中的元素
/// </summary>
/// <typeparam name="TResult">用于筛选序列上的元素的类型。</typeparam>
/// <param name="source">要过滤其元素的IEnumerable。</param>
/// <returns>IEnumerable<TResult>包含source中的TResult类型元素。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
IEnumerable<TResult> OfType<TResult>(this IEnumerable source);

```

### 二 特例转换(ToArray,ToList,ToDictionary,ToLookup)

#### **1.ToArray、ToList**
* 该方法的作用：从source 创建数组，从IEnumerable<TSource>创建List<Tsource>
* 这两个方法比较简单，不在详细赘述。

* 方法签名：

``` js

/// <summary>
/// 从source 创建数组
/// </summary>
/// <typeparam name="TSource">泛型类型参数</typeparam>
/// <param name="source">待处理的序列</param>
/// <returns>包含输入序列元素的数组。</returns>
/// <exception cref="T:System.ArgumentNullException">source==null</exception>
TSource[] ToArray<TSource>(this IEnumerable<TSource> source);

/// <summary>
/// 从IEnumerable<TSource>创建List<Tsource>
/// </summary>
/// <typeparam name="TSource">泛型类型</typeparam>
/// <param name="source">待处理序列</param>
/// <returns>返回List<TSource>,其包含的元素来自于source</returns>
/// <exception cref="T:System.ArgumentNullException">source is null.</exception>
List<TSource> ToList<TSource>(this IEnumerable<TSource> source);

```

#### **2.ToDictionary**
* 方法作用：通过`IEnumerable<TSource>`序列创建字典`Dictionary<TKey, TElement>`。

* `Dictionary<TKey, TSource> ToDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);` Sample:

``` js

class Person
{
    public int ID { get; set; }
    public string Name { get; set; }
    public double Weight { get; set; }

}

public void Conversion_ToDictionary_Simple()
{
    var list = new List<Person>()
    {
        new Person(){ ID=1,Name="小一",Weight=70},
        new Person(){ ID=2,Name="小二",Weight=80},
        new Person(){ ID=3,Name="小三",Weight=90},
        new Person(){ ID=4,Name="小四",Weight=100},
    };
    var sameIdList = new List<Person>()
    {
        new Person(){ ID=1,Name="小一",Weight=70},
        new Person(){ ID=1,Name="小一",Weight=70},
    };

    //测试正常情况
    var dic = list.ToDictionary<Person, int>(x => x.ID);

    // keySelector produces a key that is null.，引发的 ArgumentNullException 异常
    try
    {
        var nullKeyDic = list.ToDictionary<Person, int?>(x => null);
    }
    catch (ArgumentNullException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进
        Console.WriteLine(e.ToString());
    }


    //测试因为keySelector为两个元素生成重复的键，引发的ArgumentException异常
    try
    {
        var sameKeyDic = sameIdList.ToDictionary<Person, int>(x => x.ID);
    }
    catch (ArgumentException e1)
    {
        //进入
        Console.WriteLine(e1.ToString());
    }
    catch (Exception e)
    {
        //不进
        Console.WriteLine(e.ToString());
    }
}

```

* `Dictionary<TKey, TSource> ToDictionary<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey> comparer);` Sample:

``` js

class StringEqualityComparer<T>:IEqualityComparer<T>
{
    public bool Equals(T x, T y)
        => this.GetHashCode(x) == this.GetHashCode(y);

    public int GetHashCode(T obj)
        => obj.GetHashCode();
}

public void Conversion_ToDictionary_Simple_KeyComparer()
{
    var list = new List<Person>()
    {
        new Person(){ ID=1,Name="小一",Weight=70},
        new Person(){ ID=2,Name="小二",Weight=80},
        new Person(){ ID=3,Name="小三",Weight=90},
        new Person(){ ID=4,Name="小四",Weight=100},
    } as IEnumerable<Person>;
    
    //测试正常情况
    //如果IEqualityComparer<T>为null，不会抛异常，那么直接使用你的事TKey的GetHashCode()方法，判断是否相同。
    var dic = list.ToDictionary<Person, string>(x => x.Name.Substring(1, 1), new StringEqualityComparer<string>());
    var nullComparerDic = list.ToDictionary<Person, string>(x=>x.Name.Substring(1,1));
}

```

* `Dictionary<TKey, TElement> ToDictionary<TSource, TKey, TElement>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector);` Sample:

``` js

class Student
{
    public int ID { get; set; }
    public string Name { get; set; }
}

public void Conversion_ToDictionary_ElementSelector()
{
    var list = new List<Person>()
    {
        new Person(){ ID=1,Name="小一",Weight=70},
        new Person(){ ID=2,Name="小二",Weight=80},
        new Person(){ ID=3,Name="小三",Weight=90},
        new Person(){ ID=4,Name="小四",Weight=100},
    } as IEnumerable<Person>;

    //测试正常情况
    var dic = list.ToDictionary<Person, string, Student>(x => x.Name.Substring(1, 1), x => new Student() { ID=x.ID,Name=x.Name});
}

```

* 方法签名：

``` js

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

```


#### **3.ToLookup**
* 方法作用：将集合转换为按键分组的查找。和Dictionary类似，只是Dictionary是一个Key对应一个Value;ILookup是一个Key，对应多个Vlaue。

* `ILookup<TKey, TSource> ToLookup<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector);` 与 `ILookup<TKey, TSource> ToLookup<TSource, TKey>(this IEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey> comparer);`，Sample：

``` js

class EqualityComparerForLoopup<T> : IEqualityComparer<T>
{
    public bool Equals(T x, T y)
        => true;
    //=> this.GetHashCode(x) == this.GetHashCode(y);

    public int GetHashCode(T obj)
        => 0;
    //=> obj.GetHashCode();
}

public void Conversion_ToLookup()
{
    var list = new List<Person>()
    {
        new Person(){ ID=1,Name="小一",Weight=70},
        new Person(){ ID=1,Name="小二",Weight=80},
        new Person(){ ID=2,Name="小三",Weight=90},
        new Person(){ ID=2,Name="小四",Weight=100},
    } as IEnumerable<Person>;

    var lookup = list.ToLookup<Person, int>(x=>x.ID);
    //只有一个key：一；value：4个元素。这个和ToDictionary不一样，如果是ToDictionary
    //且compare返回true 则会引起异常；然而ToLookup不会引起异常，因为本来就可以允许一个key多个value。
    var comparerLookup = list.ToLookup<Person, string>(x => x.Name.Substring(1, 1), new EqualityComparerForLoopup<string>());
}

```


* 方法签名：

``` js

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

```
