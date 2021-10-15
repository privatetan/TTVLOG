## 1、键（Key）

```
-----------操作相关--------------
DEL key：key存在时删除，不存在的key会被忽略；
DUMP key：序列化key，并返回被序列化的值；
EXISTS key: 检查key是否存在，若key存在返回1，否则返回0；
KEYS pattern：查找所有符合给定模式（pattern）的 key；示例：KEYS a*
MOVE key db：将key移动到给定的数据库db中;
RANDOMKEY 从当前数据库中随机返回一个 key;
RENAME key newkey：修改key的名称；
RENAMENX key newkey：仅当newkey不存在时，将key改名为newkey；
TYPE key：返回 key 所储存的值的类型；

--------设置过期时间相关-----------
EXPIRE key seconds：设置key过期时间（秒）；
EXPIREAT key timestamp: key设置过期时间（时间戳-秒）；
PEXPIRE key milliseconds：key设置过期时间（毫秒）；
PEXPIREAT key milliseconds-timestamp：key设置过期时间（时间戳-毫秒）；
TTL key: 返回key 的剩余的过期时间（秒）；
PTTL key：返回key 的剩余的过期时间（毫秒）；
PERSIST key：移除key的过期时间；
```

## 2、字符串（String）

```
SET key value：设置key及其值；
GET key：获取key的值；
GETSET key value：将key的值设为value，并返回key的旧值(old value)；
SETNX key value：只有在key不存在时设置key的值；
STRLEN key：返回key所储存的字符串值的长度；

MSET key value [key value ...]：设置多个key与值；
MGET key1 [key2..]：获取多个key的值；
MSETNX key value [key value ...]：同时设置一个或多个key-value对，当且仅当所有给定key都不存在；

SETRANGE key offset value：同value覆盖从偏移量（offset）开始的值；
GETRANGE key start end：返回key中字符串值的子字符，字符串的截取范围包括start和end在内；

SETBIT key offset value：对key所储存的字符串值，设置或清除指定偏移量上的位(bit)；
GETBIT key offset：对key所储存的字符串值，获取指定偏移量上的位(bit)，当偏移量比字符串值的长度大，或key不存在时，返回0；

SETEX key seconds value：将值value关联到key ，并设置key的过期时间（秒）；
PSETEX key milliseconds value：将值value关联到key ，并设置key的过期时间（毫秒）；

INCR key：将key中储存的数字值增一；
DECR key：将key中储存的数字值减一；
INCRBY key increment：将key所储存的值加上给定的增量值（increment）；
DECRBY key decrement：将key所储存的值减去给定的减量值（decrement）；
INCRBYFLOAT key increment：将key所储存的值加上给定的浮点增量值（increment）；

APPEND key value：如果key已经存在并且是一个字符串， 将value追加到该key值（value）的末尾；
```

## 3、哈希表（Hash）

```
HMSET key field1 value1 [field2 value2 ] ：同时将多个 field-value (域-值)对设置到哈希表 key 中；
HMGET key field1 [field2] ：获取所有给定字段的值；
HSET key field value ：将哈希表 key 中的字段 field 的值设为 value ；
HGET key field ：获取存储在哈希表中指定字段的值；
HKEYS key ： 获取所有哈希表中的字段；
HVALS key ： 获取哈希表中所有值。

HINCRBY key field increment ：为哈希表 key 中的指定字段的整数值加上增量 increment ；
HINCRBYFLOAT key field increment ：为哈希表 key 中的指定字段的浮点数值加上增量 increment 。

HSETNX key field value ：只有在字段 field 不存在时，设置哈希表字段的值；
HEXISTS key field ：查看哈希表 key 中，指定的字段是否存在；
HGETALL key ：获取在哈希表中指定 key 的所有字段和值；
HLEN key ：获取哈希表中字段的数量；
HSCAN key cursor [MATCH pattern] [COUNT count] ：迭代哈希表中的键值对；
HDEL key field1 [field2] ：删除一个或多个哈希表字段；
```

## 4、列表（List）

```
```

