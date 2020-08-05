**目录**

[TOC]

---

# 委托

### 委托的定义与使用

```c#
private delegate string GetString(); //定义委托GetString
static void Main(string[] args)
{
    int x = 40;
    GetString GetXString = new GetString(x.ToString); //将x.ToString委托给GetXString
    string str = GetXString(); //GetXString()与直接调用x.ToString相同
    Console.WriteLine(str);
}
/**输出结果:
40
**/
```