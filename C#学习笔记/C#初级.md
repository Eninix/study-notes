

**目录:**

[TOC]



---

# 三元运算符

```ｃ#
N?A:B 
//如果N为ture返回A,否则返回B
```

# 枚举类型

```c#
enum GameState: byte//设置默认存储的类型,默认为int
{
	Pause = 111,  //默认代表的值为 0
    Faild = 222,  //默认代表的值为 1
    Success = 223,//默认代表的值为 2
    Start = 244   //默认代表的值为 3
}

/**
GameState state = GameState.Start;
这里的state的值为Start
**/
```

# 结构体

```c#
/**
定义语法
Strut <typeName/结构体名字>
{
	<memberDeclarations/成员>
}
**/

Strut Position
{
	public int x;
    public int y;
    public int z;
}
Position A;
A.x = 1;
```

# Foreach

```c#
// int[] scores = new int[5];
// int[] scores = new int[5]{33, 26, 256, 15, 159};
int[] scores = {33, 26, 256, 15, 159};
foreach (int temp in scores)
{
	Console.WriteLine(temp);
}
```

# 数组的排序

```c#
int[] arr = {64, 12, 1213, 855,151}

Array.Sort(arr);     //从小到大排序
Array.Reverse(arr);  //逆转数组
```

# @符号格式化字符串

```c#
Console.WriteLine(@"www.abcd.com \n 123456789");
//输出结果:www.abcd.com \n 123456789
//不会对转义字符进行转义
```

