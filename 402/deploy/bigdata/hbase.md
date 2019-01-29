


一、hbase安装
      1.Hbase概述
               因为我们要在bigdata001,bigdata002,bigdata003上安装Hbase
               所以这些节点上都应该有Hbase软件。  
       2.安装Hbase(因为我们要在bigdata001)
               a.清除:  rm -rf hbase
               
               wget http://mirrors.hust.edu.cn/apache/hbase/2.1.0/hbase-2.1.0-bin.tar.gz
   b.解压:  tar -zxvf hbase-1.1.2-bin.tar.gz   
               c.定义变量:
       3.配置Hbase
        a. 修改${HBASE_HOME}/conf/hbase-env.sh，修改内容如下：
  vim ${HBASE_HOME}/conf/hbase-env.sh
(A)配置JAVA_HOME环境变量,文件中间
    export JAVA_HOME=/cloudstar/software/jdk1.8.0_151
(B)放开HBASE_MANAGES_ZK环境变量,文件最后
    不让让hbase管理zookeeper集群,hbase的启动和关闭不牵涉zookeeper
    如果希望hbase启动和关闭能自动启动和关闭zookeeper集群则设置为true
    export HBASE_MANAGES_ZK=false
        b.修改${HBASE_HOME}/conf/hbase-site.xml，修改内容如下：
执行命令:  vim ${HBASE_HOME}/conf/hbase-site.xml
如果需要配置成集群的ha使用下面配置，
bigdata001和bigdata002哪个nameNode处于active都能运行

```xml
<configuration>
   <!--hbase存储数据的根路径，必须是hdfs路径,会自动创建
                 因为是多个NameNode共存的集群，因此配置要和
        ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
        中的dfs.nameservices的值一致.
        -->
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://cluster1/hbase</value>
    </property>
    <!--hbase是否按装在分布式的集群中-->
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <!--hbase存储数据的副本数,不大于hregionServers的个数即可 -->
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <!-- 当多个hmaster的时候需要提供端口，当一个hmaster的时候需要hbase.master的属性-->
    <property>
        <name>hbase.master.port</name>
        <value>60000</value>
    </property>
    <!--hbase使用的zookeeper节点在哪台机器上,-->
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>bigdata002,bigdata003</value>
    </property>
    <!--hbase使用的zookeeper的客户端端口，必须和${ZOOKEEPER_HOME}/conf/zoo.cfg
             中配置的clientPort的值一致-->
    <property>
        <name>hbase.zookeeper.property.clientport</name>
        <value>2181</value>
    </property>
    <!--hbase使用的zookeeper的数据存放路径，必须和${ZOOKEEPER_HOME}/conf/zoo.cfg
             中配置的dataDir的值一致-->
    <property>
        <name>hbase.zookeeper.property.datadir</name>
        <value>/cloudstar/software/zookeeper-3.3.6/data</value>
    </property>
    <property>
        <name>zookeeper.session.timeout</name>
        <value>60000</value>
    </property>
    <property>
        <name>hbase.unsafe.stream.capability.enforce</name>
        <value>false</value>
    </property>
</configuration>


```
  c. (可选)文件${HBASE_HOME}/conf/regionservers的内容为
vim ${HBASE_HOME}/conf/regionservers
bigdata001
bigdata002
bigdata003
d. 分发hbase
      在bigdata001上执行命令: scp -r ${HBASE_HOME} bigdata001X:/cloudstar/software/
                        例如：scp -r  /cloudstar/software/hbase-2.1.0 bigdata002:/cloudstar/software/
e. 启动HMaster(在bigdata001和bigdata002上执行)
      启动HMaster      :    nohup ${HBASE_HOME}/bin/hbase-daemon.sh start master
      关闭HMaster      :    ${HBASE_HOME}/bin/hbase-daemon.sh stop master
f. 启动Hregionserver(在bigdata001,bigdata002和bigdata003上执行)
      启动Hregionserver:    nohup ${HBASE_HOME}/bin/hbase-daemon.sh start regionserver 
      关闭Hregionserver:    ${HBASE_HOME}/bin/hbase-daemon.sh stop regionserver
g.全部启动全部关闭
      在bigdata001上执行命令: ${HBASE_HOME}/bin/start-hbase.sh
      在cbigdata001上执行命令: ${HBASE_HOME}/bin/stop-hbase.sh
      启动hbase之前，确保hadoop是运行正常的，并且可以写入文件。
      启动hbase之前，确保zookeeper集群是正常的。
h. 验证hbase启动成功
        (1)h在bigdata001和bigdata002上执行jps有HMaster进程、bigdata001,bigdata002和bigdata003上上jps有HRegionServer
        (2)使用浏览器访问http://bigdata001:16010  能够看到hbase的管理界面(新版本是这个端口号)
        (3)使用浏览器访问http://bigdata002:16010  能够看到hbase的管理界面(新版本是这个端口号)



复制缺少的jar包
cd /cloudstar/software/hbase-2.1.0/lib/client-facing-thirdparty
cp htrace* ..



${HBASE_HOME}/bin/hbase-daemon.sh stop master
${HBASE_HOME}/bin/hbase-daemon.sh stop rest
${HBASE_HOME}/bin/hbase-daemon.sh stop regionserver
nohup ${HBASE_HOME}/bin/hbase-daemon.sh start master
nohup ${HBASE_HOME}/bin/hbase-daemon.sh start regionserver 
 
nohup ${HBASE_HOME}/bin/hbase-daemon.sh start rest  #此进程名称为RESTServer，默认端口8085

 
5.Hbase的shell命令使用
  哪个机器上运行这活跃的HMaster就使用哪个机器，
  执行以下命令.
   登录：  登录hbase命令行
            /cloudstar/software/hbase-2.1.0/bin/hbase shell
   创建表： 创建名称为user的表，行健是user_id,两个列族分别是address和info
            create 'user','user_id','address','info'
   显示表： 查看hbase中所有的表
            list
   查看表： 查看user表的描述元信息,[（create+显示的信息）=>可以用于创建定制的表结构]
            describe 'user'
   写数据： 往user表中写入两行数据,两行中的列族一样，列族中的列不一样。
            put 'user','xiaoming','info:age','24'
            put 'user','xiaoming','info:birthday','1987-06-17'
            put 'user','xiaoming','info:company','baidu'
            put 'user','xiaoming','address:coutry','china'
            put 'user','xiaoming','address:provice','shandong'
            put 'user','xiaoming','address:city','qingdao'
            put 'user','daming','info:favorite','movie'
            put 'user','daming','info:birthday','1987-06-17'
            put 'user','daming','info:company','baidu'
            put 'user','daming','address:coutry','china'
            put 'user','daming','address:provice','shandong'
            put 'user','daming','address:city','heze'
            put 'user','daming','address:town','caoxian'
   追数据： 追加user表中行健为xiaoming的地址信息,
            put 'user','daming','address:provice','shanxi'
            put 'user','daming','address:provice','henan'
   读数据： 从user表中读取行健为daming的全部信息
            get 'user','daming'
   读数据： 从user表中读取行健为daming,列族为address的信息
            get 'user','daming','address'
   读数据： 从user表中读取行健为daming,列族为address,列为 provice的信息
            get 'user','daming','address:provice'
            get 'user','daming',{COLUMN=>'address:provice'} :最新版本
            get 'user','daming',{COLUMN=>'address:provice',VERSIONS=>1}:最新1版本
            get 'user','daming',{COLUMN=>'address:provice',VERSIONS=>2}:最新2版本
            get 'user','daming',{COLUMN=>'address:provice',VERSIONS=>3}:最新3版本
            get 'user','daming',{COLUMN=>'address:provice',VERSIONS=>4}:默认存储3版本
            get 'user','daming',{COLUMN=>'address:provice',TIMESTAMP=>1384092294281}:按时间戳查询
   扫数据： 全表扫描
            scan 'user' :扫描最新版本
            scan 'user',{VERSIONS=>3}:扫描最新3版本
    scan 'user',{COLUMN=>'address'}:扫描列族
    scan 'user',{COLUMN=>'address:provice'}:扫描列
   删数据： 
            删除user表中，行健为daming，列族为address，列为city的数据
            delete 'user','daming','address:city'
   删列族： 删除user表中，info列族
            disable 'user'
    alert 'user' ,{NAME=>'info',METHOD=>'delete'}
    enable 'user'
   统计表： 统计user表中有多少行
            count 'user'
   禁用表： 禁用user表
            disable 'user'
   禁用吗： user表是否禁用
            is_disable 'user'
   启用吗： user表是否启用
            is_enable 'user'
   存在吗： user表是否存在
            exists 'user'
   删除表： 删除user表，删除以前必须先禁用，不然删除不成功
            drop 'user'
   清空表： 清空user表[此操作等同于disable+drop+create]
            truncate 'user'
 6.Hbase的javaAPI使用 (省略)