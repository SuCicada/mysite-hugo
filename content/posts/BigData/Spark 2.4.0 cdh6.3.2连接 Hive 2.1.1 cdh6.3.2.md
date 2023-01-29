以下maven配置能正确读取hive.
不该加的不要加. spark版本用cdh的.

```xml
<properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <spark.version>2.4.0-cdh6.3.2</spark.version>
        <hive.version>2.1.1-cdh6.3.2</hive.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.11</artifactId>
            <version>${spark.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.11</artifactId>
            <version>${spark.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-hive_2.11</artifactId>
            <version>${spark.version}</version>
            <exclusions>
                <exclusion>
                    <artifactId>hive-metastore</artifactId>
                    <groupId>org.spark-project.hive</groupId>
                </exclusion>
                <exclusion>
                    <artifactId>hive-exec</artifactId>
                    <groupId>org.spark-project.hive</groupId>
                </exclusion>
            </exclusions>
        </dependency>
<!--        <dependency>-->
<!--            <groupId>org.apache.hive</groupId>-->
<!--            <artifactId>hive-metastore</artifactId>-->
<!--            <version>${hive.version}</version>-->
<!--        </dependency>-->
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>${hive.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>*</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
```

