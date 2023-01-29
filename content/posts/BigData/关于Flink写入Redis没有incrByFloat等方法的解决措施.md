首当其冲：改源码。
使用的是`org.apache.bahir:flink-connector-redis_2.11`
目前2020年8月中maven官方库中最新的版本只有1.0。此版本未提供`incrByFloat`的方法。

首先猜测可能maven库不是最新的。去到[此项目的github](https://github.com/apache/bahir-flink)上一看。居然是1.1-SNAPSHOT的版本。但是此版本中仍然没有找到`incrByFloat`。

所以我们可以使用改源码重新编译的方式来解决这个问题。`org.apache.flink:flink-connector-redis_2.11`库与其类似。好在这个库比较简单，不需要做结构性的改动。

本博客意在记录需要改动源码的地方。
- `org.apache.flink.streaming.connectors.redis.common.container.RedisCommandsContainer` 
redis的操作接口，在这里添加`void incrByFloat(String key, Double value);`
的接口定义。
- 操作接口的实现有2处。`org.apache.flink.streaming.connectors.redis.common.container.RedisClusterContainer`和`org.apache.flink.streaming.connectors.redis.common.container.RedisContainer`
加入incrByFloat实现：（写法完全照抄其他方法实现）
```java
    @Override
    public void incrByFloat(String key, Double value) {
        Jedis jedis = null;
        try {
            jedis = getInstance();
            jedis.incrByFloat(key, value);
        } catch (Exception e) {
            if (LOG.isErrorEnabled()) {
                LOG.error("Cannot send Redis message with command INCRBYFLOAT to key {} and value {} error message {}",
                        key, value, e.getMessage());
            }
            throw e;
        }finally {
            releaseInstance(jedis);
        }
    }
```
- `org.apache.flink.streaming.connectors.redis.common.mapper.RedisCommand`
redis操作指令枚举
加入`INCRBYFLOAT(RedisDataType.STRING),`


- `org.apache.flink.streaming.connectors.redis.RedisSink`:
最后在sink中的switch中加入对incrByFloat的支持
```java
case INCRBYFLOAT:
	this.redisCommandsContainer.incrByFloat(key, Double.valueOf(value));
	break;
```

---
至此大功告成。maven安装即可。注意要在父工程中安装才行。
`mvn install`

然后就可以在其他项目中使用了，可以选择maven本地引用（注意版本是**1.1-SNAPSHOT**），也可以直接引用jar包。
```xml
<dependency>
      <groupId>org.apache.bahir</groupId>
      <artifactId>flink-connector-redis_2.11</artifactId>
      <version>1.1-SNAPSHOT</version>
  </dependency>
```

---
其他方法缺失也可以使用同样方法在同样位置加代码。这就是有源码的好处，还可以改工程的version。
