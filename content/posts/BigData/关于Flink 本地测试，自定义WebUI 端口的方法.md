以1.11.1版本举例，相差不大的版本之间大同小异。
先给成品：以Scala代码举例，Java大同小异。
通过反射将配置加入env的配置对象中。之后使用修改过的env来创建flink的任务流即可。
```scala
val env = StreamExecutionEnvironment.getExecutionEnvironment

val javaEnv: environment.StreamExecutionEnvironment = env.getJavaEnv
val field = classOf[org.apache.flink.streaming.api.environment.StreamExecutionEnvironment].getDeclaredField("configuration")
field.setAccessible(true)
import org.apache.flink.configuration.Configuration
val configuration: Configuration = field.get(javaEnv).asInstanceOf[Configuration]
configuration.setString("rest.bind-port", "8081")
```
<br><br>
下面是探索过程，没兴趣的可以过了。

--------------



当我们加入了pom依赖后.发现能够看到本地IDE中的flink的webUI了.
```xml
   	<dependency>
		<groupId>org.apache.flink</groupId>
       	<artifactId>flink-runtime-web_2.11</artifactId>
       	<version>${flink.version}</version>
       	<scope>compile</scope>
	</dependency>
```

根据日志中显示可知我们的本地web端口为16434. 这不是一个我们想要看到的. 而且每一次运行都会产生一个随机的端口.这实在很痛苦.
```log
17:15:28,577 INFO  org.apache.flink.runtime.dispatcher.DispatcherRestEndpoint    - Rest endpoint listening at localhost:16434
17:15:28,578 INFO  org.apache.flink.runtime.highavailability.nonha.embedded.EmbeddedLeaderService  - Proposing leadership to contender http://localhost:16434
17:15:28,581 INFO  org.apache.flink.runtime.dispatcher.DispatcherRestEndpoint    - Web frontend listening at http://localhost:16434
17:15:28,581 INFO  org.apache.flink.runtime.dispatcher.DispatcherRestEndpoint    - http://localhost:16434 was granted leadership with leaderSessionID=eb84fead-f735-4350-aff4-a7f883013432
```
所以我们要想办法来固定端口.最好可以自定义.
来看源码
根据日志定位到`org.apache.flink.runtime.dispatcher.DispatcherRestEndpoint`这个类，但是进去之后你会发现里面并没有期望中的日志内容。很坑，这是一个异步调用的中转站。
那么我们就需要知道这样一条日志出现在哪里了
>Rest endpoint listening at

首先我们目前所在的地方是`org.apache.flink:flink-runtime-web`。根据(1)关联模块的命名相近原则以及 (2)`DispatcherRestEndpoint`中大量的 `flink-runtime`模块类调用。我们可以认为我们想要找的东西在`org.apache.flink:flink-runtime`中。

那么前往`flink-runtime_2.11-1.11.1.jar`源码下进行全目录检索。发现了这条日志的藏身之所：`org.apache.flink.runtime.rest.RestServerEndpoint`

然后就是一步一步往回探了。线索图如下，我们发现源自`restBindPortRange`全局变量。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200909185140165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1X2NpY2FkYQ==,size_16,color_FFFFFF,t_70#pic_center)
是这里
`this.restBindPortRange = configuration.getRestBindPortRange();`
是从configuration对象中拿键为`org.apache.flink.configuration.RestOptions.BIND_PORT`的值的值。

而这个变量来自于本类的构造函数通过参数传入的。这个参数的类型是`RestServerEndpointConfiguration`

然后就是一步一步的对这个config对象寻根溯源。
得利于IDE的美妙。我们终于找到了他的发源地，因为接口继承的关系，有两种来源。一个是`ClusterEntrypoint`一个是`MiniCluster`，由于我们是本地调试，所以只要看后者即可。即`org.apache.flink.runtime.minicluster.MiniCluster`中的
	`
		final Configuration configuration = miniClusterConfiguration.getConfiguration();
	`


所以我们只要把配置想办法加进去即可。
是的，我们找到了，这个配置对象最初始最初始的状态就是在我们的老朋友`StreamExecutionEnvironment`中装载的。甚至new都是在这里。但是这个对象很遗憾。
```java
	private final Configuration configuration;
```
私有，且没有public方法能get到。没办法了，我们只能祭出大杀器，反射。

按照开头给出的代码，实现configuration动态修改，再使用修改之后的env来创建Flink的任务流。
然后我们就能发现我们能在本地固定的8081端口打开Flink的WebUI了。
