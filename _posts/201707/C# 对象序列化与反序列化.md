# C# 对象序列化与反序列化 #

----------
## 一 对象序列化的介绍 ##

### 1. .NET支持序列化的几种方式 ###
**XML序列化：** 对象序列化之后的结果是XML形式的，通过使用`XmlSerializer`(在`System.Xml.Serializer`命名空间下)类实现。

**二进制序列化：**

**SOAP序列化：**

**json序列化：**

### 2. 几种序列化的区别 ###
**XML序列化：** XML序列化不能序列化私有数据。

**二进制序列化：**

**SOAP序列化：**

**json序列化：**


## 二 XML序列化对象详解 ##
### 1. XML序列化对象特点及要求 ###

1）使用XML序列化或反序列化时，需要对XML序列化器指定需要序列化对象的类型和其关联类型。
2）XML序列化只能序列化对象的共有属性。
3）被序列化的队形要求有个一无参的构造行数，否则无法反序列化。
4）`[Serializable]`和`[NonSerialized]`特性对XML序列化无效，所以使用XML序列化时不需要对对象增加`[Serializable]`特性。

### 2. XML序列化常用的特性 ###
|特性|功能描述|
|-----|-----:|
|`[XmlRootAttribute]`| 对根节点的描述，在类申明中使用。
|`[XmlType]` ||
|`[XmlElement]` ||
|`[XmlAttribute]` ||
|`[XmlArray]` ||
|`[XmlArrayItem]`| |
|`[XmlIgnore]`| |
|`[XmlText]`| |



