
参考地址
https://www.cnblogs.com/zhaojiankai/p/7257617.html

下载、解压、分发

wget http://mirrors.tuna.tsinghua.edu.cn/apache/storm/apache-storm-1.2.2/apache-storm-1.2.2.tar.gz
tar -xzvf apache-storm-1.2.2.tar.gz


配置环境变量

scp ~/.bashrc bigdata002:/~
scp ~/.bashrc bigdata003:/~
scp ~/.bashrc bigdata004:/~
scp ~/.bashrc bigdata009:/~
scp ~/.bashrc bigdata010:/~


[root@bigdata001 apache-storm-1.2.2]# $STORM_HOME
-bash: /cloudstar/software/apache-storm-1.2.2/: 是一个目录




vim $STORM_HOME/conf/storm.yaml

```
配置Zookeeper地址（配置Zookeeper的主机名，注意: 如果Zookeeper集群使用的不是默认端口，那么还需要配置storm.zookeeper.port）

 storm.zookeeper.servers:
     - "bigdata001"
     - "bigdata002"
     - "bigdata003"
     
     
     
storm.local.dir: The Nimbus and Supervisor daemons require a directory on the local disk to store small amounts of state (like jars, confs, and things like that). 在配置文件里添加一行
storm.local.dir: "/cloudstar/software/apache-storm-1.2.2/status"




配置nimbus.seeds：用于配置主控节点的地址，可以配置多个。
nimbus.seeds: ["bigdata001", "bigdata002", "bigdata003"]

配置supervisor.slots.ports
supervisor.slots.ports:
    - 6700
    - 6701
    - 6702
    - 6703
    

对于两台supervisor node，我们额外开启JMX支持，在配置文件中加入如下配置：
supervisor.childopts: -verbose:gc -XX:+PrintGCTimeStamps -XX:+PrintGCDetails -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.port=9998
    


```

    
    
    
分发软件   
scp -r $STORM_HOME  bigdata002:/cloudstar/software/   
scp -r $STORM_HOME  bigdata003:/cloudstar/software/   

分发配置
scp $STORM_HOME/conf/storm.yaml bigdata002:$STORM_HOME/conf/storm.yaml
scp $STORM_HOME/conf/storm.yaml bigdata003:$STORM_HOME/conf/storm.yaml



启动ui
nohup /cloudstar/software/apache-storm-1.2.2/bin/storm ui >/dev/null 2>&1 &
启动nimbus
nohup /cloudstar/software/apache-storm-1.2.2/bin/storm nimbus >/dev/null 2>&1 &
启动supervisor
nohup /cloudstar/software/apache-storm-1.2.2/bin/storm supervisor >/dev/null 2>&1 &


http://bigdata001:9980/index.html

    
    
    