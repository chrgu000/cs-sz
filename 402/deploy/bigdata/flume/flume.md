参考链接

https://blog.csdn.net/liangrui1988/article/details/71774663






wget http://mirrors.hust.edu.cn/apache/flume/1.8.0/apache-flume-1.8.0-bin.tar.gz
tar -zxvf apache-flume-1.8.0-bin.tar.gz







cd conf/
touch example.conf
vim  example.conf

```
# example.conf: A single-node Flume configuration

# Name the components on this agent
a1.sources = r1
a1.sinks = k1
a1.channels = c1


# Describe/configure the source
a1.sources.r1.type = netcat
a1.sources.r1.bind = localhost
a1.sources.r1.port = 44444


# Describe the sink
a1.sinks.k1.type = logger


# Use a channel which buffers events in memory
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100


# Bind the source and sink to the channel
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```


cd /cloudstar/software/apache-flume-1.8.0-bin
nohup bin/flume-ng agent --conf-file conf/example.conf --name a1 -Dflume.monitoring.type=http -Dflume.monitoring.port=34545 &
 
 
 
 yum install telnet
 
 
 telnet localhost 44444
 
 
 helloworld 测试(a->b传送数据)
 
 
 
 
 
 
 
 
 
 
 
 
 
 vim conf/directorytest.conf
 
 ```
 
 # Name the components on this agent
 a1.sources = r1
 a1.sinks = k1
 a1.channels = c1
 
 
 # Describe/configure the source
 a1.sources.r1.type = spooldir
 a1.sources.r1.spoolDir = /cloudstar/software/flumeTest
 a1.sources.r1.fileHeader = true
 a1.sources.r1.channels = c1
 
 
 # Describe the sink
 a1.sinks.k1.type = logger
 a1.sinks.k1.channel = c1
 
 
 # Use a channel which buffers events in memory
 a1.channels.c1.type = memory
 a1.channels.c1.capacity = 1000
 a1.channels.c1.transactionCapacity = 100
 
```
 
 cd /cloudstar/software/apache-flume-1.8.0-bin
 nohup bin/flume-ng agent --conf-file conf/directorytest.conf --name a1 -Dflume.monitoring.type=http -Dflume.monitoring.port=34545 &