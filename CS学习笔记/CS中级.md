***目录:***

[TOC]

---

+ [参数个数不确定的函数(params)](#参数个数不确定的函数)
+ [异常](#异常)
+ [out和ref](#out和ref)
+ [泛型](#泛型)
+ [List](#List)

---

# 参数个数不确定的函数

(params)

```c#
public static void UseParams(params int[] list)
{
    for (int i = 0; i < list.Length; i++)
    {
        Console.Write(list[i] + " ");
    }
    Console.WriteLine();
}
public static void UseParams2(params object[] list)
{
    for (int i = 0; i < list.Length; i++)
    {
        Console.Write(list[i] + " ");
    }
    Console.WriteLine();
}

static void Main()
{
    UseParams(1, 2, 3, 4);
    UseParams2(1, 'a', "test");
    
    int[] myIntArray = { 5, 6, 7, 8, 9 };
    UseParams(myIntArray);
    
    object[] myObjArray = { 2, 'b', "test", "again" };
     UseParams2(myObjArray);
     
     //以下调用不会导致错误，但是整个整数数组将成为params数组的第一个元素。
     UseParams2(myIntArray);
}
/*
Output:
    1 2 3 4
    1 a test
    5 6 7 8 9
    2 b test again
    System.Int32[]
*/
```

# 异常

```c#
try
{
    
}
catch(<异常类型/exceptionType> e)//省略括号可以捕获所有类型异常
{
    
}
finally
{
    
}
/**
catch可以有0个或多个
finally可以有0个或多个
catch和finally必须要有一个
**/
```

# out和ref

```c#
/**
ref和out都可以让参数进行引用传递
但是ref修饰的参数必须是已经实例化
然而out修饰的参数必须是没有实例化(若已经实例化,则会失去已实例化的值)
**/
```

# 泛型

```c#
public class Test<T> //<T, T2, T3, T4, ...>也可以,但是必须全部指定类型
{
    void Add(T input) { }
}
```

# List

```c#
//1.创建list
var l = new List<int>();
/**
List<int> l = new List<int>(){1, 22, 66}创建时赋初始值(不常用)
List<int> l = new List<int>();
()中为初始容量
**/

//2.添加数据 Add()
l.Add(11);
l.Add(22);
l.Add(11)

//3.取得数据 [index]
l[0];
l[1];

//4.获取和设置容量 Capacity
l.Capacity = 100;

//5.获取列表元素个数 Count
l.Count;

//6.插入数据 Insert(index, value)
l.Insert(1, -1);

//7.移除指定位置元素 RemoveAt(index)
l.RemoveAt(1);

//8.取得元素的索引位置 IndexOf(value) LastIndexOf(index)
l.IndexOf(11);    //0
l.LastIndexOf(11) //2

//9.排序 Sort()
l.Sort();
```

