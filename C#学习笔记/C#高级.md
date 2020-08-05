**目录**

[TOC]

---

# 委托

## 委托的定义与使用

```c#
private delegate string GetString(); //定义委托GetString
static void Main(string[] args)
{
    int x = 40;
    GetString GetXString = new GetString(x.ToString); //将x.ToString委托给GetXString
    
    //string str = GetXString(); //通过委托实例去调用x.ToString
    string str = GetXString.Invoke(); //通过Invoke方法去调用GetXString引用的x.ToString
    Console.WriteLine(str); //GetXString()与直接调用x.ToString相同
}
/**输出结果:
40
**/
```
## Action委托

Action是系统内置的一个委托类型,可以指向一个无返回值,无参数的方法  
Action<参数类型>是系统内置的一个委托类型,可以指向一个无返回值,有一个指定类型的参数的方法(最多支持16个参数)

## Func委托

Func<参数类型>中的参数类型是方法的返回值,可以有多个参数类型,但最后一个必是返回值类型

## 多播委托

```c#
static void Test1(){};
static void Test2(){};

main()
{
    Action a = Test1;
    a();
    a += Test2;//添加一个委托的引用
    a();
    a -= Test1;//减少一个委托的引用
    a();
}
/**输出结果:

**/
```

