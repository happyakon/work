# redis常用配置参数

### 1.常用配置项

| 参数                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| #no_loose_disabled-commands | 设置禁用命令，多个命令通过逗号隔开，目前支持的命令如下：flushall, flushdb, keys, hgetall, eval, evalsha, script |
| #no_loose_sentinel-enabled  | 哨兵兼容模式开关                                             |
| hash-max-ziplist-entries    | 哈希对象同时满足以下两个条件时， 使用 ziplist 编码：哈希对象保存的所有键值对的键和值的字符串长度的字节数都小于hash-max-ziplist-value的值；哈希对象保存的键值对数量小于hash-max-ziplist-entries的值。 |
| hash-max-ziplist-value      | 哈希对象同时满足以下两个条件时， 使用ziplist编码：哈希对象保存的所有键值对的键和值的字符串长度的字节数都小于hash-max-ziplist-value的值；哈希对象保存的键值对数量小于hash-max-ziplist-entries的值。 |
| lazyfree-lazy-eviction      | lazyfree内存满后驱逐选项                                     |
| lazyfree-lazy-expire        | lazyfree过期key删除选项                                      |
| lazyfree-lazy-server-del    | lazyfree内部删除选项                                         |
| list-compress-depth         | 列表上两端不被压缩的节点个数：0为特殊值（Redis的默认值），表示都不压缩；1表示list两端各有1个节点不压缩，中间的节点压缩；2表示list两端各有2个节点不压缩，中间的节点压缩；3表示list两端各有3个节点不压缩，中间的节点压缩；其后依此类推。 |
| list-max-ziplist-size       | 取正值表示按照数据项个数来限定每个quicklist节点上的ziplist长度。例如，当该参数配置为5时，每个quicklist节点的ziplist最多包含5个数据项。取负值表示按照占用字节数来限定每个quicklist节点上的ziplist长度。此时，该值只能取-1到-5这五个值，每个值含义如下。-5：每个quicklist节点上的ziplist大小不能超过64Kb（注：1kb = 1024 bytes）；-4：每个quicklist节点上的ziplist大小不能超过32Kb；-3：每个quicklist节点上的ziplist大小不能超过16Kb；-2：每个quicklist节点上的ziplist大小不能超过8Kb（Redis默认值）；-1：每个quicklist节点上的ziplist大小不能超过4 Kb。 |
| maxmemory-policy            | 设置缓存满后Redis删除内容的策略。您可以在如下八种策略中进行选择：volatile-lru -> 只从设置失效（expire set）的key中选择最近最少使用的key进行删除 （推荐）allkeys-lru -> 优先删除掉最近最少使用的keyvolatile-lfu -> 只从设置失效（expire set）的key中选择最不常用的key进行删除allkeys-lfu -> 优先删除掉最不常用的keyvolatile-random -> 只从设置失效（expire set）的key中，随机选择一些key进行删除allkeys-random -> 随机选择一些key进行删除volatile-ttl -> 只从设置失效（expire set）的key中，选出存活时间（TTL）最短的key进行删除noeviction的密钥 -> 不删除任何key，只是在写操作时返回错误。LRU表示最近最少使用的。LFU表示最不常用的。LRU，LFU和volatile-ttl都是使用近似随机算法实现的。 |
| notify-keyspace-events      | notify-keyspace-events的参数可以是以下字符的任意组合，它指定了服务器该发送哪些类型的通知。字符：发送的通知K：键空间通知，所有通知以__keyspace@<db>__为前缀E：键事件通知，所有通知以__keyevent@<db>__为前缀g：DEL、EXPIRE、RENAME等类型无关的通用命令的通知l：列表命令的通知s：集合命令的通知h：哈希命令的通知z：有序集合命令的通知x：过期事件。每当有过期键被删除时发送e：驱逐（evict）事件。每当有键因为maxmemory政策而被删除时发送A：参数g$lshzxe的别名 |
| set-max-intset-entries      | 当Set集合内的数据符合以下条件时，会使用intset编码：当集合内所有数据都是字符对象；都是基数为10的整数，范围为64位有符号整数。 |
| slowlog-log-slower-than     | 设置是否记录慢查询日志：当取值为负时，不记录任何操作；当取值为0时记录所有操作；当取值为正时，只有当操作执行时间大于设置值，操作才被记录（单位为微秒）。 |
| slowlog-max-len             | 慢日志最多保存记录条数。                                     |
| zset-max-ziplist-entries    | 排序集合对象同时满足以下两个条件时， 使用ziplist编码：排序集合对象保存的所有键值对的键和值的字符串长度的字节数都小于zset-max-ziplist-value的值；排序集合对象保存的键值对数量小于zset-max-ziplist-entries的值。 |
| zset-max-ziplist-value      | 排序集合对象同时满足以下两个条件时， 使用ziplist编码：排序集合对象保存的所有键值对的键和值的字符串长度的字节数都小于zset-max-ziplist-value的值；排序集合对象保存的键值对数量小于zset-max-ziplist-entries的值。 |
| list-max-ziplist-entries    | 链表对象同时满足以下两个条件时， 使用ziplist编码：链表对象保存的所有元素的字符串长度的字节数都小于list-max-ziplist-value的值；链表集合对象保存的元素数量小于list-max-ziplist-entries的值。 |
| list-max-ziplist-value      | 链表对象同时满足以下两个条件时， 使用ziplist编码：链表对象保存的所有元素的字符串长度的字节数都小于list-max-ziplist-value的值；链表集合对象保存的元素数量小于list-max-ziplist-entries的值。 |
| cluster_compat_enable       | redis cluster兼容模式                                        |
| script_check_enable         | 检查lua脚本key是否在相同slot                                 |