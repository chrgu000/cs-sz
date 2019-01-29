

三、安装kafka
九、安装kafka
0.角色分配如下：
           bigdata001,bigdata002，bigdata003
1.安装kafka(在chinac27上执行)
a.解压： tar -zxvf kafka_2.9.1-0.8.2.0.tgz
b.变量： vi ~/.bashrc 定义如下变量，并添加到PATH中
          c.刷新变量：  source ~/.bashrc
          d.配置kafka(在chinac27上执行)
vi ${KAFKA_HOME}/config/server.properties
broker.id=0
host.name=hadoop200
num.partitions=3
log.dir=/usr/local/kafka/data
zookeeper.connect=chinac27:2181,chinac82:2181,chinac88:2181
delete.topic.enable=true（运行删除主题再使用此配置）
                    e.创建日志文件夹
mkdir ${KAFKA_HOME}/data
改变权限保证权限不能小于775
chmod u+w ${KAFKA_HOME}/data
chmod g+w ${KAFKA_HOME}/data
chmod o+w ${KAFKA_HOME}/data
f.分发kafka
             scp -r /usr/local/kafka hadoop20X:/usr/local/
g.分发环境变量(对方要刷新环境变量)
              scp /etc/profile hadoop204:/etc/
          2.适配kafka(在hadoop20X上执行,hadoop204为例)
         vi ${KAFKA_HOME}/config/server.properties
         broker.id=4
         host.name=hadoop204

         3.启动Kafka(每一台)
a.启动zookeeper集群（每一台上执行）
启动：  ${ZOOKEEPER_HOME}/bin/zkServer.sh start  
停止：  ${ZOOKEEPER_HOME}/bin/zkServer.sh stop  
验证：  ${ZOOKEEPER_HOME}/bin/zkServer.sh status
客户端： ${ZOOKEEPER_HOME}/bin/zkCli.sh
b.启动kafka集群（每一台上执行）
启动：nohup ${KAFKA_HOME}/bin/kafka-server-start.sh ${KAFKA_HOME}/config/server.properties &
停止：${KAFKA_HOME}/bin/kafka-server-stop.sh &
验证：jps能看到Kafka进程
c.如果重装KAFKA需要删除以下znode
在zkCli.sh命令下删除以下ZNODE
rmr  /consumers
rmr  /admin
rmr  /config
rmr  /controller
rmr  /brokers
rmr  /controller_epoch
rm -rf ${KAFKA_HOME}/data/*
rm -rf ${KAFKA_HOME}/logs/*

d.如果修改了server.properties需要分配到其他機器上
scp /root/software/kafka_2.11-0.8.2.2/config/server.properties chinac82:/root/software/kafka_2.11-0.8.2.2/config/
scp /root/software/kafka_2.11-0.8.2.2/config/server.properties chinac82:/root/software/kafka_2.11-0.8.2.2/config/





b.启动kafka集群（每一台上执行）
启动：
nohup ${KAFKA_HOME}/bin/kafka-server-start.sh ${KAFKA_HOME}/config/server.properties &

停止：
${KAFKA_HOME}/bin/kafka-server-stop.sh &
验证：jps能看到Kafka进程

4.使用kafka客户端
a.创建主题：
     ${KAFKA_HOME}/bin/kafka-topics.sh --create --zookeeper   bigdata002:2181, bigdata003:2181 --replication-factor 3 --partitions 3 --topic app3k
b.删除主题：
     ${KAFKA_HOME}/bin/kafka-topics.sh --delete --zookeeper   bigdata002:2181, bigdata003:2181 --topic app3k
c.列出主题：
     ${KAFKA_HOME}/bin/kafka-topics.sh --list   --zookeeper  bigdata002:2181, bigdata003:2181
e.描述主题：
     ${KAFKA_HOME}/bin/kafka-topics.sh --describe --zookeeper  bigdata002:2181, bigdata003:2181 --topic app3k
f.发送消息:(producer,接下来输入你想要发送的消息)
     ${KAFKA_HOME}/bin/kafka-console-producer.sh --broker-list  bigdata001:9092, bigdata002:9092, bigdata003:9092 --topic app3k
g.接受消息:(consumer,可以接受到来自提供方的消息)
     ${KAFKA_HOME}/bin/kafka-console-consumer.sh --zookeeper bigdata002:2181, bigdata003:2181 --topic app3k  --from-beginning
     
     
####################################################################   
   
启动kafka的jmx监控   

启动：
 export JMX_PORT=17988; ${KAFKA_HOME}/bin/kafka-server-start.sh ${KAFKA_HOME}/config/server.properties &


本机 jconsole
bigdata001:17988



####################################################################   

 wget https://codeload.github.com/smartloli/kafka-eagle-bin/tar.gz/v1.2.4
 
 
 
 
 
 
 

${KAFKA_HOME}/bin/kafka-topics.sh --create --zookeeper   bigdata002:2181, bigdata003:2181 --replication-factor 2 --partitions 2 --topic chun 
${KAFKA_HOME}/bin/kafka-topics.sh --create --zookeeper   bigdata002:2181, bigdata003:2181 --replication-factor 2 --partitions 2 --topic xia

 
