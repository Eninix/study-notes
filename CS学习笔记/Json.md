[TOC]

---

## JsonMapper解析json

```
class Skill
{
    public int id;
    public string name;
    public int damage;

    public Skill() { }
    public Skill(int id, string name, int damage)
    {
        this.id = id;
        this.name = name;
        this.damage = damage;
    }

    public override string ToString()
    {
        return string.Format("ID:{0}, Name:{1}, Damage:{2}", id, name, damage);
    }
}
```



```
List<Skill> skillList = new List<Skill>();


//用JsonMapper去解析json文本
//JsonData代表一个数组或对象
//在这里jsondata是数组
JsonData jsonData = JsonMapper.ToObject(File.ReadAllText("JsonTest.json"));

foreach (JsonData temp in jsonData)//在这里jsondata是对象
{
    Skill skill = new Skill((int)temp["id"], (string)temp["name"], (int)temp["damage"]);
    skillList.Add(skill);
    //JsonData idVal = temp["id"];
    //Console.WriteLine(temp["id"] +" "+ temp["name"] +" "+ temp["damage"]);
}


foreach (var item in skillList)
{
    Console.WriteLine(item);
}
```

## JsonMapper和泛型解析json

```
//json里的key必须与Skill里的属性对应
Skill[] skillArr = JsonMapper.ToObject<Skill[]>(File.ReadAllText("JsonTest.json"));
foreach (var item in skillArr)
{
    Console.WriteLine(item);
}
```

```
List<Skill> skillList = JsonMapper.ToObject<List<Skill>>(File.ReadAllText("JsonTest.json"));
foreach (var item in skillList)
{
    Console.WriteLine(item);
}
```

