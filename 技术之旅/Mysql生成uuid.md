# MySQL生成32位UUID

* MySQL提供UUID()函数的用法如下：

```sql
mysql> SELECT UUID();
mysql> c2cb8f66-351f-11e7-b3ed-00163e0429b6
mysql> SELECT REPLACE(UUID(), '-', '');  #将'-'符号替换掉
mysql> 45c87fa0352211e78d40d4977a9ea871
```


在 MySQL 的 UUID() 函数中，前三组数字从时间戳中生成，第四组数字暂时保持时间戳的唯一性，
第五组数字是一个 IEEE 802 节点标点值，保证空间唯一。使用 UUID() 函数，可以生成时间、空间上都独一无二的值。

**使用UUID，基本上不可能出现相同的UUID值**