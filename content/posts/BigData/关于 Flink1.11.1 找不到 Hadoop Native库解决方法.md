可以试试在 `flink` 的 `conf/flink-conf.yaml` 配置文件中加入配置如下
其中的native库的具体路径换成你自己的。

```yaml
yarn.application-master.env.LD_LIBRARY_PATH: /opt/cloudera/parcels/CDH/lib/hadoop/lib/native:$LD_LIBRARY_PATH
yarn.taskmanager.env.LD_LIBRARY_PATH: /opt/cloudera/parcels/CDH/lib/hadoop/lib/native:$LD_LIBRARY_PATH
```
