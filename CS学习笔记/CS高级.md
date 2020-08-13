***目录:***

[TOC]

---

+ [委托](#委托)
+ [LINQ](#LINQ)
+ [反射和特性](#反射和特性)
+ [多线程](#多线程)

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

# 多线程

## 1.委托方式发起线程(后台线程)

注意:若在前台程序执行完时,后台线程未结束,会被强行终止.

### 异步委托

```c#
    class Program
    {
        static long i = 0;

        static long Test()
        {
            for (int j = 0; j < 1000000; j++)
            {
                i += j;
                Thread.Sleep(0);
            }
            return i;
        }
        static void Main(string[] args)
        {
            Func<long> a = Test;
            IAsyncResult ar = a.BeginInvoke(null, null); //委托方式发起线程
            Console.WriteLine(i);
            Console.WriteLine(i);
            Console.WriteLine(i);
            Console.WriteLine(i);

            while (ar.IsCompleted == false) //如果 异步线程 没有执行完毕
            {
                Console.Write(".");
                Thread.Sleep(10);
            }
            Console.WriteLine();
            
            long res = a.EndInvoke(ar); //获取 异步线程 返回值
            Console.WriteLine(res);
            Console.WriteLine();
            
            
            Console.ReadKey();
        }
    }

```

### 检测线程结束

```c#
//检测线程结束
bool isEnd = ar.AsyncWaitHandle.WaitOne(100);//在这停下,等待线程结束 //超过100毫秒则超时返回false,否则返回true
if (isEnd)
{
    Console.WriteLine("线程结束,返回值为:" + a.EndInvoke(ar));
}
else
{
    Console.WriteLine("线程超时!");
}
```

### 回调函数 检测线程结束

```c#
/* 其中一种写法 */
static void Main(string[] args)
{
    Func<long> a = Test;

    //倒数第二个参数:委托类型的参数,表示回调函数,当线程结束时会调用这个委托指向的方法
    //倒数第一个参数:用于给回调函数传递参数
    a.BeginInvoke(OnCallBack, a);

    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);
    
    Console.ReadKey();
}

//这个ar是IAsyncResult类型的变量,是BeginInvoke()自动赋予的
static void OnCallBack(IAsyncResult ar)
{
    //ar.AcyncState表示传个回调函数的参数,在这里就是上面传来的a
    //需要做一下类型转换
    Func<long> a = ar.AsyncState as Func<long>;
    long res = a.EndInvoke(ar);
    Console.WriteLine("线程结束,线程返回值为:" + res);
}
```

### lambda表达式 匿名回调函数 检测线程结束

```c#
static void Main(string[] args)
{

    Func<long> a = Test;

    a.BeginInvoke(ar =>
    {
        long res = a.EndInvoke(ar);
        Console.WriteLine("线程结束,线程返回值为:" + res);
    }, null);

    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);
    Console.WriteLine(i);



    Console.ReadKey(true);
}
```

## 2.Thread类发起线程(前台线程)

### 发起线程

```c#
static void DownloadFile()
{
    Console.WriteLine("开始下载!");
    Thread.Sleep(2000);
    Console.WriteLine("下载完成!");
}

static void Main(string[] args)
{
    Thread t = new Thread(DownloadFile); //创建Thread对象,但是没有开始执行线程
    t.Start(); //开始执行线程

    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine(i);
        Thread.Sleep(200);
    }

    Console.ReadKey(true);
}
```

### lambda表达式发起线程

```c#
static void Main(string[] args)
{
    Thread t = new Thread(() =>
    {
        Console.WriteLine("开始下载!");
        Thread.Sleep(2000);
        Console.WriteLine("下载完成!");
    }); //创建Thread对象,但是没有开始执行线程

    t.Start(); //开始执行线程

    for (int i = 0; i < 10; i++)
    {
        Console.WriteLine(i);
        Thread.Sleep(200);
    }

    Console.ReadKey(true);
}
```

### 传参数进去01

```c#
static void DownloadFile(object fileName) //要么没参数,有的话就必须object类型
{
    Console.WriteLine("[" + Thread.CurrentThread.ManagedThreadId + "]" + "开始下载:" + fileName);
    Thread.Sleep(2000);
    Console.WriteLine("下载完成!");
}

static void Main(string[] args)
{
    Thread t = new Thread(DownloadFile); //创建Thread对象,但是没有开始执行线程

    t.Start("xxx.avi"); //开始执行线程 //传参数需要从Start()方法里传进去

    for (int i = 0; i < 15; i++)
    {
        Console.WriteLine(i);
        Thread.Sleep(200);
    }

    Console.ReadKey(true);
}
```

### 传参数进去02

　　单独传建一个下载类,让后吧下载类的下载函数传进去.

### 其他

t.Abort(); //终止线程  
t.Join(); //等待t线程执行完,在继续执行下面的代码

## 3.线程池开启线程

1. 线程池中所有线程都是后台线程
2. 线程池中的线程不能改为前台线程
3. 只适用于比较短的线程
4. 若需要一个长时间运行的线程,请使用Thread创建

```c#
static void ThreadMethod(object state)
{
    Console.WriteLine("[" + Thread.CurrentThread.ManagedThreadId + "]" + "线程开始!");
    Thread.Sleep(2000);
    Console.WriteLine("[" + Thread.CurrentThread.ManagedThreadId + "]" + "线程结束!");
}

static void Main(string[] args)
{
    ThreadPool.QueueUserWorkItem(ThreadMethod); //线程池开启线程
    ThreadPool.QueueUserWorkItem(ThreadMethod);
    ThreadPool.QueueUserWorkItem(ThreadMethod);


    Console.ReadKey(true);
}

```

## 4.任务_开始线程

后台线程

```
 static void ThreadMethod()
 {
     Console.WriteLine("[" + Thread.CurrentThread.ManagedThreadId + "]" + "线程开始!");
     Thread.Sleep(2000);
     Console.WriteLine("[" + Thread.CurrentThread.ManagedThreadId + "]" + "线程结束!");
 }

 static void Main(string[] args)
 {
     //Task t = new Task(ThreadMethod); //任务开始方法1
     //t.Start();

     TaskFactory tf = new TaskFactory(); //任务开始方法2
     Task t = tf.StartNew(ThreadMethod);




     Console.ReadKey();
 }
```

## 5.锁

```c#
//执行到这里时会申请锁住m对象,申请到了就会执行,否则一直等到申请到
lock(m)
{
	//申请到了会执行这里的代码
}//执行完解除锁定
```

