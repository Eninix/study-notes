***目录:***

[TOC]

---

+ [委托](#委托)
+ [LINQ](#LINQ)
+ [反射和特性](#反射和特性)

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
    	form k in kongfu
		where m.Kongfu == k.Name && k.Power >99
		select m;
```

## LINQ联合查询扩展方法

```c#
var res = masterList.SelectMany(m => Kongfu,(m, k) => new {master = m, kongfu = k}).Where(x => x.master.Kongfu == x.Kongfu.Name && x.kongfu.Power > 99);
```

## 对结果进行排序orderby descending

```c#
where ...
orderby m.Level, m.Age //按照多个字段排序,第一个相同就按第二个
select ...
    
//扩展写法
var res = masterList.Where(m => m.Level>8).OrderBy(m => m.Level).ThenBy(m.Age);
```

## join on 集合联合

```c#
var res = from m in masterList
    join k in kongfu on m.Kongfu equals k.Name
    select new {master = m, kongfu = k};
```

## into groups 对结果进行分组

```c#
var res = from k in kongfu
    join m in masterList on k.Name equals m.Kongfu
    into groups
    orderby groups.Count()
    select new {kongfu = k, count = groups.Count()};
```

## group by 对结果进行分组

```c#
var res = from m in masterList
	group m by m.Menpai
    into g
    select new {count = g.Count(), key = g.Key};//g.Key Key表示是按照那个类别进行的分组
```

## 量词操作符 any all

```c#
//Any只要其中一个满足就返回True
bool res1 = masterList.Any(m => m.Mengpai == "丐帮");
//All需要全部满足才返回True
bool res2 = masterList.All(m => m.Mengpai == "丐帮");
```

# 反射和特性

## type类

```c#
namespace 特性与反射
{
    class Program
    {
        static void Main(string[] args)
        {
            MyClass my = new MyClass();
            Type type = my.GetType();
            Console.WriteLine(type.Name); //获取类名
            Console.WriteLine(type.Namespace); //获取所在命名空间
            Console.WriteLine(type.Assembly); //程序集类型

            FieldInfo[] arr = type.GetFields(); //获取共有字段
            foreach (FieldInfo item in arr)
            {
                Console.Write(item.Name + " ");
            }
            Console.WriteLine();

            PropertyInfo[] arr2 = type.GetProperties(); //获取属性
            foreach (PropertyInfo item in arr2)
            {
                Console.Write(item.Name + " ");
            }
            Console.WriteLine();

            MethodInfo[] arr3 = type.GetMethods(); //获取方法
            foreach (MethodInfo item in arr3)
            {
                Console.Write(item.Name + " ");
            }
            Console.WriteLine();



            Console.ReadKey(true);
        }
    }
}
```

## Assembly类

```c#
            MyClass my = new MyClass();
            Assembly ass = my.GetType().Assembly;
            Console.WriteLine(ass.FullName); //输出获取程序集
            Type[] type = ass.GetTypes();
            foreach (var item in type)
            {
                Console.WriteLine(item); //输出的是类名
            }

```

## Obsolete特性(过时的)

```c#
[Obsolete("这个方法过时了！请使用**方法代替")]
static void OldMethod()
{
    console.WirteLine("lalala!")
}
//这样会在调用oldmethod时给出提示,但是依旧可以调用
```

## Condition特性（有条件的）

```c#
#define cdt//定义一个宏（或变量）

[Conditional("cdt")]
static void Method()
{
    console.WirteLine("lalala!")
}
//Method只会在cdt被定义时才会被调用
```

## 调用者信息特性

```c#
//在方法的参数前定义，需要给参数一个默认值
//[CallerFilePath] //调用方法的文件路径
//[CallerLineNumber] //调用方法的行数
//[CallerMemberName] //在哪个函数里调用的方法
```

## DebuggerStepThrough特性

```c#
[DebuggerStepThrough]
static void Method()
{
    console.WirteLine("lalala!")
}
//在调试时不会进入该方法里，默认该方法正确，可跳过单步调试
```

## 自定义特性

```c#
//1.特性类后缀以Attribute结尾
//2.需要继承System.Attribute
//3.一般需声明为sealed（不可被继承）
//4.一般情况下 特性类用来表示目标结构的一些状态（定义一些字段和属性，一般不定义方法）
[AttributeUsage(AttributeTarget.Class)]
sealed class MyTestAttribute: System.Attribute
{
    public string Description { get; set; }
    public string VersionNumber { get; set; }
    public int ID { get; set; }
}

//使用时需省略Attribute
[MyTest]
class person {
    
}
```

