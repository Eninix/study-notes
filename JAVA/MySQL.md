# 1.安装

## 1.1

![image-20210320185731731](assets.MySQL/image-20210320185731731.png)

```
update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';

mysql8 用：ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

![image-20210320190720812](assets.MySQL/image-20210320190720812.png)

## 1.2

![image-20210320194215387](assets.MySQL/image-20210320194215387.png)

# 2.操作数据库

## 1.创建

```sql
CREATE DATABASE [IF NOT EXISTS] westos;
```

## 2.删除

```sql
DROP DATABASE [IF EXISTS] westos;
```

## 3.使用

```sql
-- 如果你的表名或者字段名是特殊字符,则需要带上 `` 来包裹
USE `school`
```

## 4.查看

```sql
SHOW DATABASES; --查看所有数据库
```

# 3.数据库列类型

![image-20210321103305098](assets.MySQL/image-20210321103305098.png)

![image-20210321103338968](assets.MySQL/image-20210321103338968.png)

![image-20210321103358580](assets.MySQL/image-20210321103358580.png)

![image-20210321105153426](assets.MySQL/image-20210321105153426.png)

