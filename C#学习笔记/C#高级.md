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
static void Test1(){Console.WriteLine("test1")};
static void Test2(){Console.WriteLine("test2")};

main()
{
    Action a = Test1;
    a();//test1
    a += Test2;//添加一个委托的引用
    a();//test1和test2
    a -= Test1;//减少一个委托的引用
    a();//test2
}
/**输出结果:
test1
test1
test2
test2
**/
```

# 重写一个类的ToString

ToString()方法代表直接输出这个类的对象时会怎样去输出

```c#
public override string ToString()
{
	return string.Format("id:{0}\tname:{1}\tage:{2}  \tmengpai:{3}\tkongfu:{4}  \tlever:{5}", this.id, this.name, this.age, this.mengpai, this.kongfu, this.lever);
}
```

# LINQ

## 基础查询

```c#
var masterList = new List<MartialArtsMaster>()
{
	new MartialArtsMaster(){Id = 1, Age = 100, Kongfu = "葵花宝典", Name = "张三", Lever = 100, Mengpai = "厚大法考"},
	new MartialArtsMaster(){Id = 2, Age = 50, Kongfu = "葵花宝典", Name = "张加", Lever = 50, Mengpai = "厚大法考"},
	new MartialArtsMaster(){Id = 3, Age = 3, Kongfu = "葵花宝典", Name = "默认", Lever = 25, Mengpai = "厚大法考"},
	new MartialArtsMaster(){Id = 4, Age = 10, Kongfu = "葵花宝典", Name = "阿萨", Lever = 10, Mengpai = "厚大法考"}
};

var kongfu = new List<Kongfu>()
{
    new Kongfu(){Id = 1, Name = "葵花宝典", Power = 999999}
};
//使用LINQ做查询(表达式写法)
var res = from m in masterList
		//from后设置查询的集合
		where m.Lever >= 50
		//where后是查询的条件
		select m.Name;
		//select后是m的结果结合返回
foreach (var temp in res)
{
    Console.WriteLine(temp);
}
```

## 扩展方法

```c#
//过滤方法
static bool Test1(MartialArtsMaster master)
{
	if (master.Lever >= 50) return true;
	return false;
}
        
//扩展的方法的写法
var res = masterList.Where(Test1);
            
foreach (var temp in res)
{
    Console.WriteLine(temp);//需要重写ToString方法
}
```

## 利用lambda表达式

```c#
var res = masterList.Where(m=>m.Lever>=50);
            
foreach (var temp in res)
{
    Console.WriteLine(temp);//需要重写ToString方法
}
```

## LINQ联合查询

```c#
var res = from m in masterList
    	form k in Kongfu
		where m.Kongfu == k.Name && k.Power >99
		select m;
```

