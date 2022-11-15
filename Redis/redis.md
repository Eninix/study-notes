# Redis的使用

-----

## 基础命令

```shell
set key value # 增
get key # 查
del key # 删
select db_number # 切换数据库 # 默认为0，共计16个数据库 0-15
dbsize # 查看当前数据库大小
flushdb # 清空当前数据库
flushall # 清空全16个数据库
```

## String

```shell
# redis 中字符串 value 最大是 512M
setex key second value # 定是销毁
mset key1 value1 key2 value2 ... # set多个
mget key1 key2 ... # get多个
```

## Hash

```shell
# key { field1 : value1, field2 : value2, field3 : value3 } 
hset key field1 value1
hset key field2 value2
hget key field1
hget key field2
hgetall key
hdel key field1 field2 ... 
hmset key field1 value1 field2 value2 ... # set多个
hmget key field1 field2 ... # get多个
hlen key # 查看长度
hexists key field # 查看哈希表 key 中,给定域 field 是否存在
```

## List

lpush rpush lpop rpop lrange del

```shell
lpush list_name val1 val2 ... # 从 左 压入链表（rpush 从右压入）
lrange list_name 0 -1 # 从 左 开始 第0个 取到 倒数第一个（即取出全部）
lpop list_name # 从 左 出（rpop从右出）
del list_name
lindex list_index # 查看下标的所在元素 从0开始 -1表示倒数第一个 以此类推
```

## Set

```shell
sadd key val1 val2 ...
smembers key # [取出所有值]
sismember key val # [判断值是否是成员]
srem key val # [删除指定值]
```

