# 安装

## 1.1

![image-20210320185731731](assets.MySQL/image-20210320185731731.png)

```
update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';

mysql8 用：ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

![image-20210320190720812](assets.MySQL/image-20210320190720812.png)

## 1.2
