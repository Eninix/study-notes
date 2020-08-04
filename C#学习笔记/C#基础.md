目录Demo

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
```

