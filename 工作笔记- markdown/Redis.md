## Redis

[toc]

### 一、常用命令

##### 1.分布式锁

- SETNX key value

  ```markdown
  - setnx的含义就是SET if Not Exists，其主要有两个参数 setnx(key, value)。
  
  `将 key 的值设为 value ，当且仅当 key 不存在。
  `若给定的 key 已经存在，则 SETNX 不做任何动作。
  
  - 该方法是原子的，如果key不存在，则设置当前key成功，返回1；如果当前key已经存在，则设置当前key失败，返回0。
  ```

- SET key value [EX seconds] [PX milliseconds] [NX|XX]

  ```markdown
  set key value [EX seconds] [PX milliseconds] [NX|XX]
  EX seconds：设置失效时长，单位秒
  PX milliseconds：设置失效时长，单位毫秒
  NX：key不存在时设置value，成功返回OK，失败返回(nil)
  XX：key存在时设置value，成功返回OK，失败返回(nil)
  
  SET product:10001 true  ex  10  nx
  ```

- 注意⚠️：

  > Redis的setnx命令是当key不存在时设置key，但setnx不能同时完成expire设置失效时长，不能保证setnx和expire的原子性。我们可以使用set命令完成setnx和expire的操作，并且这种操作是原子操作。

##### 设置过期时间

`Redis` 使用 `expire` 命令设置一个键的过期时间，到时间后 `Redis` 会自动删除它。`expire` 命令的使用方法为

```markdown
` expire key seconds

- 返回 1 表示设置成功，
- 返回 0 表示键不存在或者设置失败。

127.0.0.1:6379> expire b 10
(integer) 1
127.0.0.1:6379> expire z 10
(integer) 0
127.0.0.1:6379> 


如果想要更精确的过期时间可以使用 pexpire，时间单位是毫秒，pttl 可以查看剩余过期时间。
```

Redis Setex 命令为指定的 key 设置值及其过期时间。如果 key 已经存在， SETEX 命令将会替换旧的值。

```sh
redis 127.0.0.1:6379> SETEX KEY_NAME TIMEOUT VALUE


redis 127.0.0.1:6379> SETEX mykey 60 redis
OK
redis 127.0.0.1:6379> TTL mykey
60
redis 127.0.0.1:6379> GET mykey
"redis
```

##### 不设置过期时间

```markdown
使用 `set` 命令为键赋值也会清除键的过期时间。

127.0.0.1:6379> set a "world"
OK
127.0.0.1:6379> ttl a
(integer) -1
127.0.0.1:6379> 
```

##### 2.查询过期时间

```sh
`ttl 'mala:cron_task:core_conf:test_2'

当键不存在时返回 -2 ；
当键没有过期时间即永久存在，那么返回值为 -1；


127.0.0.1:6379> set num 60
OK
127.0.0.1:6379> set a "hello"
OK
127.0.0.1:6379> expire a 60
(integer) 1
127.0.0.1:6379> ttl a
(integer) 53
127.0.0.1:6379> expire z 100
(integer) 0
127.0.0.1:6379> ttl z	# 键不存在
(integer) -2
127.0.0.1:6379> ttl a	# 键过期
(integer) -2
127.0.0.1:6379>
127.0.0.1:6379> set b "world"
OK
127.0.0.1:6379> ttl b	# 键没有过期时间
(integer) -1
127.0.0.1:6379>  
```

##### 3.取消过期时间

persist 命令用于设置取消键的过期时间，如果过期时间被成功清除则返回 1；否则返回 0。

```sh
` persist
` persist 命令用于设置取消键的过期时间，如果过期时间被成功清除则返回 1；否则返回 0。

127.0.0.1:6379> expire key 20
(integer) 1
127.0.0.1:6379> ttl key
(integer) 17
127.0.0.1:6379> persist key
(integer) 1
127.0.0.1:6379> ttl key
(integer) -1
127.0.0.1:6379> 
```

除过使用 `persist` 命令外，使用 `set` 命令为键赋值也会清除键的过期时间。

```sh
127.0.0.1:6379> setex a 60 "hello"
OK
127.0.0.1:6379> ttl a
(integer) 57
127.0.0.1:6379> ttl a
(integer) 56
127.0.0.1:6379> set a "world"
OK
127.0.0.1:6379> ttl a
(integer) -1
127.0.0.1:6379> 

```



