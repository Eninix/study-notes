**目录:**

---

# 参数个数不确定的函数

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

