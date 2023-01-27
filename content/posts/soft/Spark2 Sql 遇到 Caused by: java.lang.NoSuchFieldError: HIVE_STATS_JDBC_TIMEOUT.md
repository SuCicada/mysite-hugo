问题版本：
Spark：2.14.0
Hive：2.1.0

原因参见[spark hive java.lang.NoSuchFieldError: HIVE_STATS_JDBC_TIMEOUT](https://www.cnblogs.com/fbiswt/p/11798514.html)

解决方案：
使用 cdh版本的包，比如
```xml
<spark.version>2.4.0-cdh6.3.2</spark.version>
<hive.version>2.1.1-cdh6.3.2</hive.version>
```

如果遇到有关 apache的log4j包缺失，加入
```xml
 		<dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.8.2</version>
        </dependency>
 ```
注意版本

相关问题参见[I'm getting “NoClassDefFoundError: org/apache/logging/log4j/util/ReflectionUtil”](https://stackoverflow.com/questions/52700803/im-getting-noclassdeffounderror-org-apache-logging-log4j-util-reflectionutil)

掰掰
