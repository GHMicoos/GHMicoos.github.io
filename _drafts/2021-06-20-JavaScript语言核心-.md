---
layout: post
title:  "JavaScript语言核心-"
categories: JavaScript语言核心
tags: JavaScript ECMAScript 基础
author: GHMicoos
---

* content
{:toc}

暂无

第二章 词法结构

1.字符集
  JavaScript使用的是Unicode字符集。ECMAScript 3 要求JavaScript的实现必须支持Unicode 2.1及后续版本；
  ECMAScript 5则要求支持Unicode 3及后续版本。

  在有的计算机或者软件里面，无法显示或输入Unicode字符全集。JavaScript定义了一种特殊的序列来表示任意的
  16位Unicode内码————这种转义序列均以\u，其后跟随4个十六进制数（0-9及A-Z大小写均可）。这种转移序列可
  以用到JavaScript字符串中的直接量、正则表达式直接量、标识符中。 
  eg: 字符`é`的转义写法为`\u00E9`,`'café'==='caf\u00e9' //ture`


   

2.JavaScript区分大小写
3.JavaScript中的注释
``` js
//这里是单行注释
/*这里是单行注释*/ //这里是另一个单行注释
/*
*这里是多行注释
*这里可以写更多
* ···
*/
```
4.直接量————就是程序中直接使用的数据值。

5.标识符：就是一个名字，JavaScript中标识符是用来对变量和函数进行命名，或者用作JavaScript代码中某些循环
语句中的跳转位置标记（label）。
  要求：必须以字母、下划线(_)或者美元符号($)开始；后续字符可以是字母、数字、下划线或者美元字符。字母（不仅仅包含
  ASCII中的字符，可以是Unicode字符集中的字母）

``` js
/*
*i
*my_variable_anme
*v13
*_dumy
*$str
*我_fdf
* 这些都是合法的标识符
*/
```
6.保留字：
JavaScript保留了一些标识符用作自己的关键字，这些标识符就叫保留字。保留字不可以当作普通标识符来使用了。

7.可选的分号
JavaScript使用分号将语句分割开，这对增强代码的可读性和整洁性非常重要。
JavaScript可以将分号省略
    1.如果语句各自占一行
    2.程序结尾或者右花括号`}`之前的分号也可以省略
省略分号注意：
需要注意的是，JavaScript并不是在所有换行处都会填补分号：只有在当前语句与下一条语句合并在一起就无法正常解析代码的时候，才会填补分号。
但是有两个例外
    1.涉及`return` `break` `continue`这三个关键字后紧跟着换行，JavaScript则会在换行处填补分号。
    ``` js
    return
    true;
    //JavaScript解析成 `return ;true; `然而本意是 `return true;`
    //也就是说`return` `break` `continue`和随后的表达式之间不能有换行；如果添加了换行，程序只有在极为特殊的情况才会报错，而且程序的调试非常不方便。
    ```
    2.涉及`++` `--`这两个表达式即可以作前缀也可以作后缀，如果用作后缀，那么它和表达式应该在同一行，不然它将会作为后一行代码的前缀。
    ``` js
    x
    ++
    y
    //解释为：`x;++y;`，而不是`x++;y;`
    ```

通常来说，一条语句如果以`（` `[` `/` `+` `-`开始，那么他和前一条语句合在一起解析。

``` js 
var a
a
=
3
console.log(a)
//JavaScript解析成： var a;a=3;console.log(a);
//因为如果不在第一行添加分号，无法解析 var a a

//省略分号，解释器可能会出现意想不到的结果
//以下本来是两个语句
var y=x+f
(a+b).toString()
//JavaScript会解释成 var y=x+f(a+b).toString()

```
  



## 一 什么是JavaScript？








## 一 什么是枚举？

枚举是一种**值类型**。枚举类型是包含一组已命名常量的独特值类型。  
eg：某种情况下，我们需要标识某任务的处理状态，其状态包括 待处理，正在处理，已处理。  
我们可以使用,0,1,2来标识 三个状态，但是我们可以定义为枚举，那样具有更好的可读性，避免硬编码。  
``` js
public enum TaskState : int
{
    WatieToDeal = 0,
    Dealing = 1,
    Dealed = 2,
}
``` 
  
## 二 如何定义枚举？  
枚举使用一种整型值类型作为其基础存储(默认是int)， 并提供离散值的语义含义。  
``` js
public enum TaskState : int
{
    WatieToDeal = 0,
    Dealing = 1,
    Dealed = 2,//
}
```  

## 三 枚举的特点与注意事项？
#### **1. 默认情况下，第一个枚举值0**  
(从技术上说，是0显示转型为基础枚举类型)，后续每一项递增1（从技术上说，是0显示转型为基础枚举类型），后续每一项递增1,但是可以显示为枚举赋值。  

``` js
public enum ConnectionState
{
    Disconnected,//值为 0
    Connecting = 10,
    Connected,//值为 11
    Joined = Connected,//值为 11（将Connected引用的值赋值给Joined）
    Disconnecting//值为12
}
```  

注释：  
* 值 0 可以**隐式转换**为 枚举变量。特别注意值0应该设置为枚举。  
* 如果创建的是值枚举而不是标志枚举，创建 None 枚举常量仍十分有用。  
*（原因是在默认情况下，公共语言运行库会将用于枚举的内存初始化为零。因此，如果不定义值为零的常量，则枚举在创建时将包含非法值。）*   
 
    * 如果明显存在应用程序需要表示的默认情况，请考虑使用值为零的枚举常量表示默认值。  
    * 如果不存在默认情况，请考虑使用值为零的枚举常量（这意味着该情况不由任何其他枚举常量表示）。 


#### **2. 枚举总有一个基础类型**
基础类型包括 `byte`、`sbyte`、`short`、`ushort`、`int`（默认）、`uint`、`long`或 `ulong`，但是可以使用`继承语法`来指定一个基础类型（注释，所以枚举的性能取决于基础类型的性能）

``` js
public eumn ConnectionState:short
{
    //... ...  
}
```

#### **3. 未定义枚举值也能转换成功。**  
**只要值能成功转型为基础类型，就会成功。**  
eg：完全可以将42转型为一个`ConnectionState`，即使当前没有定义对应的`ConnectionState`的枚举值。  

``` js
ConnectionState state = (ConnectionState)42;
```

**优点**：在没有对应的枚举值得前提下也允许转型，优点在于枚举能在未来的API发布中添加新值，而且不会破坏早起版本。   
**缺点**：开发者在编码时必须谨慎，要主动考虑到位命名值得可能性。  
**注释**：  
* 为枚举变量赋值的时候，尽量使用枚举名称赋值；  
如果用值类型变量强制转换赋值，需要考虑值类型变量可能未在枚举中定义名称的情况。  
* 在定义采用枚举常量作为值的方法或属性时，应考虑对该值进行验证。  
原因是即使没有在枚举中定义某个数值，也可以将该数值强制转换为枚举类型。 
    
#### **4. 枚举与基础类型相互转换必须显示转换**  
从**枚举转换成基础类型**，以及从**基础类型转换为枚举类型**，都必须**显示转换**，而不是隐式转换。

``` js
//eg:
//基础类型转枚举
ConnectionState state01 = (ConnectionState)42；//正确
ConnectionState state02 = 42；//错误 编译报错
```
``` js
//枚举转基础类型
int int01=(int)enum02;
int int01=enum02;//错误 编译报错
```
``` js
//特殊的
ConnectionState enum03 = 0;  
```

#### **5. 枚举插入值会顺移**   
在一个枚举中部插入一个枚举值，会使其后的所有枚举值发生顺移。

``` js
//old
public eumn ConnectionState
{
    Disconnected,//0
    Connecting ,//1
}
//new
public eumn ConnectionState
{
    Disconnected,//0
    Connected,//1
    Connecting ,//2 顺移了
}
```

虽然允许在代码未来的版本中为一个枚举添加额外的值，但是这样做时候要注意，原来的值可能会顺移。  
避免发生这类情况，可以在**枚举末尾插入新的枚举值**，或者**显示地进行赋值(常用)**。

``` js
//较好的方法 01
public eumn ConnectionState
{ 
    Disconnected,//0
    Connecting ,//1 
    Connected,//2 插入到枚举末尾
}
//较好的方法 02
public eumn ConnectionState
{ 
    Connected=2,//2 显示赋值后，不考虑 枚举间的位置
    Disconnected=0,//0
    Connecting=1 ,//1 
}
```

## 四 枚举的优势
* 代码更容易维护
* 代码更清晰，可读性
* 枚举也较容易键入

## 五 字符串与枚举值转换
#### **1. 枚举值转换为枚举值标识符**
使用`ToString()`方法。  
  

``` js
enum ConnectionState : short
{
    Disconnected,
    Connecting = 10,
    Connected,
    Joined = Connected,
    Disconnecting,
}

class Program
{
    static void Main(string[] args)
    {
        ConnectionState enum01 = ConnectionState.Disconnected;
        Console.WriteLine(enum01.ToString());
        //打印 Disconnected

        ConnectionState enum02 = (ConnectionState)44;
        Console.WriteLine(enum02.ToString());
        //打印 44
        Console.WriteLine("键入任何键退出... ...");
        Console.ReadKey();
    }
}
```
  
#### **2. 字符串转换为枚举** 
使用：`Enum.Parse()`, `Enum.TryParse()`, `Enum.TryEnum<T>()` 
 
``` js
string str = "Joined";
//Enum.Parse() 要抛异常 需处理
ConnectionState enmu01 = (ConnectionState)Enum.Parse(typeof(ConnectionState), str);
Console.WriteLine(enmu01);//打印 Connected

//Enum.TryParse() 不抛异常
ConnectionState enum02;
bool success02 = Enum.TryParse<ConnectionState>(str, out enum02);
Console.WriteLine(enum02);//打印 Connected
Console.WriteLine(success02);//打印 true

```
  
## 六 `Flag`属性 ##

C#位域主要用于.Net里面对于某一个事物有多种混合状态时使用，如，权限管理，文件读写权限等等  
* 对枚举使用 `FlagsAttribute` 自定义属性。 
* 用 2 的幂（即 1、2、4、8 等）定义枚举常量。这意味着组合的枚举常量中的各个标志都不重叠。
* 对枚举(数值)执行按位运算（AND、OR、XOR）,常用的有 | & ^ &~
具体含义：
`|` 表示两边求并集（元素相加，相同元素只出现一次）  
`&` 表示两边是否其中一个是另外一个的子集，如果是返回子集，否则返回0（如果其中一个包含另外一个，返回被包含的，否则返回0）  
`^` 表示从两者的并集中去除两者的交集（把两个的元素合并到一起，如果两个中有公共元素，要将这个公共元素从合并的结果中去除）  
`&~` A &~ B 表示：A集合中去掉A与B的交集部分。与^区别是，^去掉交集部分后，还会合并B中不是交集的部分。  
* 优点：数据库也支持位操作

``` sql
select 1|2 as '|' ,(1|2)&1 as '&',(1|2)^(2|4) as '^' ,(1|2)&~(2|4) as '&~'
--3,1,5, 1
```

``` js
//使用方法：以权限为列
[Flags]
enum MyPermission
{
    None = 0,
    Select = 0x1 << 0,
    Download = 0x1 << 1,
    Delete = 0x1 << 2,
    Update = 0x1 << 3,//int 最大值可以写到30
    User = Select | Download,//值为 1+2=3
    Admin = Select | Download | Update,//值为 1+2+8=11
    SystemAdmin = Admin | Delete,//值为 11+4=15
}

static void Main(string[] args)
{
    /// | 表示两边求并集（元素相加，相同元素只出现一次）
    ///添加权限（列子含义）
    // MyPermission.Select | MyPermission.Download | MyPermission.Delete
    MyPermission permission01 = MyPermission.Select | MyPermission.Download | MyPermission.Delete | MyPermission.Select;
    MyPermission permission02 = MyPermission.Select | MyPermission.Download | MyPermission.Select;//MyPermission.User

    /// & 表示两边是否其中一个是另外一个的子集，如果是返回子集，否则返回0（如果其中一个包含另外一个，返回被包含的，否则返回0）
    ///拥有权限判断（列子含义）
    MyPermission permission03 = MyPermission.User & MyPermission.Download;//MyPermission.Download

    /// ^ 表示从两者的并集中去除两者的交集（把两个的元素合并到一起，如果两个中有公共元素，要将这个公共元素从合并的结果中去除）
    /// 删除权限（列子含义）
    MyPermission permission04 = MyPermission.User ^ MyPermission.Download;//MyPermission.Select
    MyPermission permission05 = MyPermission.User ^ MyPermission.Download ^ MyPermission.Select;//0
    //MyPermission permission06 = 0 ^ MyPermission.Select;//MyPermission.Select 

    /// ~ 表示取反，返回的结果我还不知道应该是什么，以后再查一下。用法主要和“&”一起使用，例如：去除其中的某个元素
    /// A &~ B 表示：A集合中去掉A与B的交集部分。与^区别是，^去掉交集部分后，还会合并B中不是交集的部分。
    MyPermission permission06 = MyPermission.User & ~MyPermission.Download;//MyPermission.Select
    MyPermission permission07 = MyPermission.User & ~(MyPermission.Download | MyPermission.Update);//MyPermission.Select
    MyPermission permission08 = MyPermission.User & ~(MyPermission.Download & MyPermission.Update);//MyPermission.User

    ///注释：对于0 这个值来说，我们把它考虑为空集合，空集合是任何集合的子集，空集合中一个元素都没有。
    //空集合 不包含任何元素
    MyPermission permission09 = MyPermission.User | MyPermission.None;//MyPermission.User
                                                                          //空集合是任何集合的子集
    MyPermission permission010 = MyPermission.User & MyPermission.None;//MyPermission.None
                                                                           //空集合 不包含任何元素
    MyPermission permission011 = MyPermission.User ^ MyPermission.None;//MyPermission.User
                                                                           //空集合 不包含任何元素
    MyPermission permission012 = MyPermission.User & ~MyPermission.None;//MyPermission.User
}
```

## 七 `Descripttion` 特性
在项目中通常用到的是在枚举变量上面加上Description。
枚举的值一般为int在数据库中占用空间比较小，枚举的变量用于给数据库中的字段赋值，
那么如果要显示字段就需要考虑到Descripttion特性，显示特定名称
``` js
[Flags]
public enum MyPermission
{
    [Description("无任何权限")]
    None = 0,
    [Description("查询")]
    Select = 0x1 << 0,
    [Description("下载")]
    Download = 0x1 << 1,
    [Description("删除")]
    Delete = 0x1 << 2,
    [Description("更新")]
    Update = 0x1 << 3,//int 最大值可以写到30
    [Description("查询+下载")]
    User = Select | Download,//值为 1+2=3
    [Description("查询+下载+更新")]
    Admin = Select | Download | Update,//值为 1+2+8=11
    [Description("所有权限=查询+下载+更新+删除")]
    SystemAdmin = Admin | Delete,//值为 11+4=15

    }

public static class ExtensionMethods
{
    public static string GetDescripetion<T>(this T value) where T : struct
    {
        string result = "";
        try
        {
            if (value is Enum)
            {
                string str = value.ToString();
                FieldInfo field = value.GetType().GetField(str);
                object[] objs = field.GetCustomAttributes(typeof(DescriptionAttribute), false);
                if (objs != null && objs.Length != 0)
                {
                    DescriptionAttribute da = objs[0] as DescriptionAttribute;
                    if (string.IsNullOrWhiteSpace(da.Description))
                    {
                        
                    }
                }
            }
        }
        catch { }
        return result;
    }
}
```
