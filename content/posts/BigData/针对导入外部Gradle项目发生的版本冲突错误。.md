比如一个使用场景：Intellij 导入Kafka 2.2.1版本源码。

在使用gradle初始化项目时各种grafana配置文件报错。

原因主要是本机构建用的Gradle版本与项目编写配置文件产生冲突，简而言之就是Gradle版本不对。

比如Kafka 2.2.1 在发布的时候，Gradle 版本最高直到5.4.1。而本机使用了Gradle 6 。导致了冲突，下载 Gradle 5.4.1 并用其构建即可解决。
