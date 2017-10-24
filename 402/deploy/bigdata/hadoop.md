
1.下载解压
    cd /cloudstar/software
    wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-3.0.0-beta1/hadoop-3.0.0-beta1.tar.gz
    tar -zxvf hadoop-3.0.0-beta1.tar.gz 
2.环境变量
    
一、hadoop配置部分
1.修改Hadoop配置文件
    命令如下：
    vim ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
    添加如下内容如下：
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
2.分发配置文件
    scp ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml bigdata7:/${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
    scp ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml bigdata8:/${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
    scp ${HADOOP_HOME}/etc/hadoop/hdfs-site.xml bigdata9:/${HADOOP_HOME}/etc/hadoop/hdfs-site.xml
3.重启Hadoop
    3.1重启HDFS
        ${HADOOP_HOME}/sbin/stop-dfs.sh
        ${HADOOP_HOME}/sbin/start-dfs.sh
    3.2重启YARN
        ${HADOOP_HOME}/sbin/stop-yarn.sh
        ${HADOOP_HOME}/sbin/start-yarn.sh

4.下载，解压knox,配置KNOX
 根据提示配置相应的hostname
 wget  http://apache.fayea.com/knox/0.8.0/knox-0.8.0.tar.gz
 chmod 777 knox-0.8.0.tar.gz
 tar -zxvf knox-0.8.0.tar.gz
 cd /bigdata/software/knox-0.8.0/conf/
 vim /bigdata/software/knox-0.8.0/conf/topologies/sandbox.xml
二、KNOX配置部分
5.KNOX服务启动
    cd /bigdata/software/knox-0.8.0/bin
    5.1启动嵌入式ldap
        启动命令：
            ./ldap.sh start
        回显如下：
            Starting LDAP succeeded with PID 50563.
    5.2创建knox的gate-way实例
        创建命令：
             ./knoxcli.sh  create-master
         回显如下：
             回显中提示输入密码，本利中输入的密码是bigdata
             ***************************************************************************************************
            You have indicated that you would like to persist the master secret for this service instance.
            Be aware that this is less secure than manually entering the secret on startup.
            The persisted file will be encrypted and primarily protected through OS permissions.
            ***************************************************************************************************
            Enter master secret:
            Enter master secret again:
            Master secret has been persisted to disk.
    5.3启动knox-gateway
        改变所属和权限
            chmod -R ugo+rwx knox-0.8.0/
            chown -R bigdata knox-0.8.0
        以bigdata用户只需程序（此程序不能以root用户执行，因此需要在root用户的CLI下只需如下指令）
            su bigdata -l -c "/bigdata/software/knox-0.8.0/bin/gateway.sh start"
    5.4常用命令
            su bigdata -l -c "/bigdata/software/knox-0.8.0/bin/gateway.sh start"
            su bigdata -l -c "/bigdata/software/knox-0.8.0/bin/gateway.sh stop"
            su bigdata -l -c "/bigdata/software/knox-0.8.0/bin/gateway.sh status"
            su bigdata -l -c "/bigdata/software/knox-0.8.0/bin/gateway.sh clean"
            注意：
            NOTE: This command will also clear any .out and .err file from the {GATEWAY_HOME}/logs directory so use this with caution.
6.验证启动
    6.1查看监听端口
         netstat -lunt
         能有如下信息
        tcp6       0      0 :::8443                 :::*                    LISTEN
    6.2CURL获取HDFS状态
          curl -i -k -u guest:guest-password -X GET  'https://bigdata6:8443/gateway/sandbox/webhdfs/v1/?op=LISTSTATUS'
        回显如下：我们可以抗到HDFS文件系统的信息
            HTTP/1.1 200 OK
            Set-Cookie: JSESSIONID=r7ctpupb8cvv1ekkuv3pu8khe;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Cache-Control: no-cache
            Expires: Fri, 01 Apr 2016 07:23:13 GMT
            Date: Fri, 01 Apr 2016 07:23:13 GMT
            Pragma: no-cache
            Expires: Fri, 01 Apr 2016 07:23:13 GMT
            Date: Fri, 01 Apr 2016 07:23:13 GMT
            Pragma: no-cache
            Server: Jetty(6.1.26)
            Content-Type: application/json
            Transfer-Encoding: chunked

            {"FileStatuses":{"FileStatus":[{"accessTime":0,"blockSize":0,"childrenNum":1,"fileId":16449,"group":"supergroup","length":0,"modificationTime":1459333745586,"owner":"root","pathSuffix":"data","permission":"755","replication":0,"storagePolicy":0,"type":"DIRECTORY"},{"accessTime":0,"blockSize":0,"childrenNum":7,"fileId":16386,"group":"supergroup","length":0,"modificationTime":1459414365085,"owner":"root","pathSuffix":"hbase","permission":"755","replication":0,"storagePolicy":0,"type":"DIRECTORY"},{"accessTime":0,"blockSize":0,"childrenNum":2,"fileId":16446,"group":"supergroup","length":0,"modificationTime":1459222589726,"owner":"root","pathSuffix":"history","permission":"770","replication":0,"storagePolicy":0,"type":"DIRECTORY"},{"accessTime":0,"blockSize":0,"childrenNum":3,"fileId":16389,"group":"supergroup","length":0,"modificationTime":1459332722472,"owner":"root","pathSuffix":"tmp","permission":"733","replication":0,"storagePolicy":0,"type":"DIRECTORY"},{"accessTime":0,"blockSize":0,"childrenNum":1,"fileId":16404,"group":"supergroup","length":0,"modificationTime":1459154498158,"owner":"root","pathSuffix":"user","permission":"755","replication":0,"storagePolicy":0,"type":"DIRECTORY"}]}}
    6.3浏览器获取    HDFS状态
        浏览器请求如下URL，需要输入账号和密码，账号为：guest 密码为：guest-password
        https://bigdata6:8443/gateway/sandbox/webhdfs/v1/?op=LISTSTATUS
        可以看到如下界面：

三、Hadoop与KNOX集成实验部分
7.hdfs操作
    7.1：删除HDFS中的文件夹/user/guest/example
    命令如下：
        curl -i -k -u guest:guest-password -X DELETE 'https://bigdata6:8443/gateway/sandbox/webhdfs/v1/user/guest/example?op=DELETE&recursive=true'
    回显如下：
        HTTP/1.1 200 OK
        Set-Cookie: JSESSIONID=19sljea4zx9vlgz5xq0vu66dr;Path=/gateway/sandbox;Secure;HttpOnly
        Expires: Thu, 01 Jan 1970 00:00:00 GMT
        Cache-Control: no-cache
        Expires: Fri, 01 Apr 2016 07:51:54 GMT
        Date: Fri, 01 Apr 2016 07:51:54 GMT
        Pragma: no-cache
        Expires: Fri, 01 Apr 2016 07:51:54 GMT
        Date: Fri, 01 Apr 2016 07:51:54 GMT
        Pragma: no-cache
        Server: Jetty(6.1.26)
        Content-Type: application/json
        Transfer-Encoding: chunked

     7.2：Register the name for a sample file README in /user/guest/example
        命令如下：
            curl -i -k -u guest:guest-password -X PUT 'https://bigdata6:8443/gateway/sandbox/webhdfs/v1/user/guest/example/README?op=CREATE'
        回显如下：
            HTTP/1.1 307 Temporary Redirect
            Set-Cookie: JSESSIONID=zsd3hdcj4y61u1yw3vdtsdmw;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Cache-Control: no-cache
            Expires: Fri, 01 Apr 2016 07:54:14 GMT
            Date: Fri, 01 Apr 2016 07:54:14 GMT
            Pragma: no-cache
            Expires: Fri, 01 Apr 2016 07:54:14 GMT
            Date: Fri, 01 Apr 2016 07:54:14 GMT
            Pragma: no-cache
            Location: https://bigdata6:8443/gateway/sandbox/webhdfs/data/v1/webhdfs/v1/user/guest/example/README?_=AAAACAAAABAAAABwcxIpXspp-mgu_KW2yUgnh4LyVE-Lja3PXqvmf55-bqFM5u42b1WgvqLym6JMd6OZQOS6pkgMzzyQeCCvAkXynJKyN2VWn4oLkR7yKDwxatQTkwOCTPfbNs3odqjyGP47Fav7h3D7EU2jLDeXiG3eeCad4ZmDHM5TlGQgcu8xtR5m9fJmCv4xtA
            Server: Jetty(6.1.26)
            Content-Type: application/octet-stream
            Transfer-Encoding: chunked

     7.3：Upload README to /user/guest/example. Use the README in {GATEWAY_HOME}
        命令如下：
            curl -i -k -u  guest:guest-password -T README -X PUT '{https://bigdata6:8443/gateway/sandbox/webhdfs/data/v1/webhdfs/v1/user/guest/example/README?_=AAAACAAAABAAAABwcxIpXspp-mgu_KW2yUgnh4LyVE-Lja3PXqvmf55-bqFM5u42b1WgvqLym6JMd6OZQOS6pkgMzzyQeCCvAkXynJKyN2VWn4oLkR7yKDwxatQTkwOCTPfbNs3odqjyGP47Fav7h3D7EU2jLDeXiG3eeCad4ZmDHM5TlGQgcu8xtR5m9fJmCv4xtA}'
        回显如下：
            HTTP/1.1 100 Continue
            HTTP/1.1 201 Created
            Set-Cookie: JSESSIONID=13611xoonu0xd1oyr1ik0m11a5;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Location: hdfs://cluster1/user/guest/example/README
            Connection: close
            Server: Jetty(8.1.14.v20131031)
        如果出现Hadoop中的HDFS权限异常，可以执行如下操作：
            hadoop fs -chmod -R 777 /user/
        执行效果：
           可以在HDFS中创建/user/guest/example/README文件
     7.4：Request the content of the README file in /user/guest/example
        命令如下：
            curl -i -k -u guest:guest-password -X GET 'https://localhost:8443/gateway/sandbox/webhdfs/v1/user/guest/example/README?op=OPEN'
        回显如下：
            HTTP/1.1 307 Temporary Redirect
            Set-Cookie: JSESSIONID=14w7b47cyjuz91fqm4bf33ikio;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Cache-Control: no-cache
            Expires: Fri, 01 Apr 2016 08:15:34 GMT
            Date: Fri, 01 Apr 2016 08:15:34 GMT
            Pragma: no-cache
            Expires: Fri, 01 Apr 2016 08:15:34 GMT
            Date: Fri, 01 Apr 2016 08:15:34 GMT
            Pragma: no-cache
            Location: https://localhost:8443/gateway/sandbox/webhdfs/data/v1/webhdfs/v1/user/guest/example/README?_=AAAACAAAABAAAABwcxIpXspp-mgu_KW2yUgnh4LyVE-Lja3PXqvmf55-bqFM5u42b1WgvqLym6JMd6OZQOS6pkgMzzwM0BX7EwJPGlwjcHAp5YT925k9PlHTXaLBu4H9lcHwbEn9See3he5qmPJaUvU3C3VV9AXsgzaPsWxk1gsW2b7X1A7RyZ3rgj1m88to5pAj-g
            Server: Jetty(6.1.26)
            Content-Type: application/octet-stream
            Transfer-Encoding: chunked

     7.5：Request the content of the README file in /user/guest/example
        命令如下：
            curl -i -k -u guest:guest-password -X GET '{https://localhost:8443/gateway/sandbox/webhdfs/data/v1/webhdfs/v1/user/guest/example/README?_=AAAACAAAABAAAABwcxIpXspp-mgu_KW2yUgnh4LyVE-Lja3PXqvmf55-bqFM5u42b1WgvqLym6JMd6OZQOS6pkgMzzwM0BX7EwJPGlwjcHAp5YT925k9PlHTXaLBu4H9lcHwbEn9See3he5qmPJaUvU3C3VV9AXsgzaPsWxk1gsW2b7X1A7RyZ3rgj1m88to5pAj-g}'
        回显如下：
            HTTP/1.1 200 OK
            Set-Cookie: JSESSIONID=of4tpetyeaqy1sfwck450wvwb;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Access-Control-Allow-Methods: GET
            Access-Control-Allow-Origin: *
            Content-Type: application/octet-stream
            Connection: close
            Server: Jetty(8.1.14.v20131031)
            /*以下是README文件中的内容*/
     7.6：Optionally cleanup the example directory
        命令如下：
            curl -i -k -u guest:guest-password -X DELETE 'https://localhost:8443/gateway/sandbox/webhdfs/v1/user/guest/example?op=DELETE&recursive=true'
        回显如下：
            HTTP/1.1 200 OK
            Set-Cookie: JSESSIONID=dvvs9r1mk5ic1lhf6sabnj4w;Path=/gateway/sandbox;Secure;HttpOnly
            Expires: Thu, 01 Jan 1970 00:00:00 GMT
            Cache-Control: no-cache
            Expires: Fri, 01 Apr 2016 08:19:31 GMT
            Date: Fri, 01 Apr 2016 08:19:31 GMT
            Pragma: no-cache
            Expires: Fri, 01 Apr 2016 08:19:31 GMT
            Date: Fri, 01 Apr 2016 08:19:31 GMT
            Pragma: no-cache
            Server: Jetty(6.1.26)
            Content-Type: application/json
            Transfer-Encoding: chunked

三、HadoopHA与KNOX集成部分

cd /bigdata/software/knox-0.8.0/conf/topologies
vim sandbox.xml
配置内容如下
     1.在gateway中添加如下配置
        <provider>
           <role>ha</role>
           <name>HaProvider</name>
           <enabled>true</enabled>
           <param>
               <name>WEBHDFS</name>
               <value>maxFailoverAttempts=3;failoverSleep=1000;maxRetryAttempts=300;retrySleep=1000;enabled=true</value>
           </param>
        </provider>
    2.修改server为如下配置，其中当前active的namenode配到第一个
        <service>
            <role>WEBHDFS</role>
            <url>http://bigdata6:50070/webhdfs</url>
            <url>http://bigdata7:50070/webhdfs</url>
        </service>
