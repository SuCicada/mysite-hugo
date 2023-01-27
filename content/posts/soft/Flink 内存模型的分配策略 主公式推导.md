结论: 启动flink设定的 ytm数值 与实际监控展示的JVM_Heap数值关系是 (ytm大于1920的简化公式)

`JVM_Heap = ytm * 0.45 - 256`

------------------
啓動參數: -ytm  设定的实际是 进程总内存,相当于yarn容器大小
```
Total_Process_Memory: ytm
JVM_Metaspace: 默認 256m
JVM_Overhead: 默認 jtm * 0.1 (必須在 192m ~ 1g (默認))

Total_Flink_Memory: Total_Process_Memory - JVM_Overhead - JVM_Metaspace
Framework_Heap: 默認 128m
Managed_Memory: 默認 Total_Flink_Memory * 0.4
Framework_Off-Heap: 默認 128m
Task_Off-Heap: 默認 0
Network: 默認 Total_Flink_Memory * 0.1 (必須在 64m ~ 1g (默認))

所以 Task_Heap = Total_Flink_Memory - Framework_Heap - Managed_Memory - Framework_Off-Heap - Task_Off-Heap - Network
```

如果都用默认配置,那么代入化简就是 
```
Total_Flink_Memory = ytm - 256 - min(max(ytm * 0.1, 192), 1024)
JVM_Heap = Total_Flink_Memory - min(max(Total_Flink_Memory * 0.1,64), 1024) - Total_Flink_Memory * 0.4 - 128 - 0 - 128
         = Total_Flink_Memory * 0.6 - min(max(Total_Flink_Memory * 0.1,64), 1024) - 256
         = (ytm - 256 - min(max(ytm * 0.1, 192), 1024))*0.6 - min(max((ytm - 256 - min(max(ytm * 0.1, 192), 1024))*0.1,64), 1024) - 128
Task_Heap = JVM_Heap - 128 
          = (ytm - 256 - min(max(ytm * 0.1, 192), 1024))*0.6 - min(max((ytm - 256 - min(max(ytm * 0.1, 192), 1024)) * 0.1,64), 1024) - 256
```

附带公式计算的小程序
```python
def full(ytm, pri=True):
    Total_Process_Memory = ytm
    JVM_Metaspace = 256
    JVM_Overhead = min(max(Total_Process_Memory * 0.1, 192), 1024)
    Total_Flink_Memory = Total_Process_Memory - JVM_Metaspace - JVM_Overhead
    Network = min(max(Total_Flink_Memory * 0.1, 64), 1024)
    Task_Off_Heap = 0
    Framework_Off_Heap = 128
    Managed_Memory = Total_Flink_Memory * 0.4
    Framework_Heap = 128
    JVM_Heap = Total_Flink_Memory - Network - Task_Off_Heap - Framework_Off_Heap - Managed_Memory
    Task_Heap = JVM_Heap - Framework_Heap
    if pri:
        print("-------------------------------")
        print("Total_Process_Memory: ", Total_Process_Memory)
        print("JVM_Metaspace: ", JVM_Metaspace)
        print("JVM_Overhead: ", JVM_Overhead)
        print("Total_Flink_Memory: ", Total_Flink_Memory)
        print("Network: ", Network)
        print("Task_Off_Heap: ", Task_Off_Heap)
        print("Framework_Off_Heap: ", Framework_Off_Heap)
        print("Managed_Memory: ", Managed_Memory)
        print("Framework_Heap: ", Framework_Heap)
        print("JVM_Heap: ", JVM_Heap)
        print("Task_Heap: ", Task_Heap)
    return JVM_Heap

def simple(ytm):
    return ytm * 0.45 - 256

```

参考 
[https://blog.csdn.net/ytp552200ytp/article/details/107508034](https://blog.csdn.net/ytp552200ytp/article/details/107508034)
[https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/ops/deployment/yarn_setup.html](https://ci.apache.org/projects/flink/flink-docs-release-1.11/zh/ops/deployment/yarn_setup.html)
