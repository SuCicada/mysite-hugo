#### [Event 构成 ](#1)
#### [Source](#source)

---

Flume的三个部分：Source，Channel，Sink
数据是存储在Event对象中在这三部分之间传递

<div id="1"></div>

### Event 构成
Event 接口
```java
public interface Event {

  public Map<String, String> getHeaders();
  public void setHeaders(Map<String, String> headers);
  public byte[] getBody();
  public void setBody(byte[] body);

}
```
以 最简单的实现类 `SimpleEvent` 为例子
```java
public class SimpleEvent implements Event {

  private Map<String, String> headers;
  private byte[] body;
  ...
}
```
可见
+  header 
	> 以 `Map<String, String>` 的形式存储键值对信息
+  body
 	> 二进制形式存储，从Source接收到的数据会存储在这里
 
 ---
 	
<div id="source"></div>
 
### Source
###### 带着问题
+ header 里有什么
###### 一些实现类
+ `ExecSource`
	 + ExecRunnable 主要处理 
	 + 通过 EventBuilder.withBody 基础创建 Event ， 一行一个 Event
	 + header 为空
	 + flushEventBatch 中 
          + ChannelProcessor :: processEventBatch
                1. 依次通过所有拦截器 Intercept
                2. 通过 ChannelSelector 分发
                3. Event 放入 Channel 中
          - SourceCounter :: addToEventAcceptedCount
             - 监控器作用
    +  SpoolDirectorySource
        + SpoolDirectoryRunnable 处理
        
- Channel
    - MemoryChannel
        - doPut 由 Source 放入
            1. 将传入的 Event 放入 putList
            2. Event 不做操作
        - doTake 由 Sink 拿走 
            1. queue.poll()  --  LinkedBlockingDeque<Event> 类型 
            2. 放入 tackList 中， 缓冲充当，有大小 transCapacity 限制 
    - KafkaChannel
        - doPut 由 Source 放入
            1. 寻找 Event header 中的 
                - _key_  名称：key
                - _partitionHeader_ `Integer` 类型。（名称 在配置文件中 由 partitionIdHeader 指定）
                    > 用于 分区id 
                - _parseAsFlumeEvent_  ( xml ) 
                    > 指定是否序列化 为 FlumeEvent <br> 默认 true
            2. 创建 ProducerRecord 
                - 键为 Event header 中的 key
                - 值为 对 event 进行 serializeValue ， 根据 parseAsFlumeEvent 来 序列化
            - Event 被 序列化 传给 kafka broker
        - doTake 由 Sink 拿走 
            - Event 由 kafka 反序列生成，再返回给 Sink 的
            1. 拿 ConsumerRecord 由 kafka 消费者
            2. deserializeValue 根据 parseAsFlumeEvent 反序列化，转化为 Event
            3. 如果 record 有 key， 对 Event header 加入 “key” -> record.key() 
- Sink
    - ```channel.take();```调用的是 Channel 中的 toTake
    - LoggerSink
        - 直接拿
    - KafkaSink
        - 找 Event header 中的 
            - _topicHeader_
                - _allowTopicOverride_ ( xml ) `boolean` 为 true 
                - 否则
                    - _kafka.topic_ ( xml )
            - _key_ 
            - _partitionIdHeader_ `Integer`
                > 用于 分区id 
            - _useFlumeEventFormat_ `boolean`( xml )
                > 默认 false

        1. 从 Channel 拿到 Event（ take ）
        2. 生成 ProducerRecord
            > 对 event 进行 serializeValue ， 根据 _useFlumeEventFormat_ 来 序列化
        3. 生产者生产 ProducerRecord
        4. producer 加入 future 异步进程
        5. producer flush 
        6. 提交channel事务
b
