http://www.cnblogs.com/dijia478/p/8075638.html




一、安装Tomcat
cd /cloudstar/software
wget https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-9/v9.0.11/bin/apache-tomcat-9.0.11.tar.gz

tar -zxvf apache-tomcat-9.0.11.tar.gz


二、配置环境变量
        vim　~/.bashrc
        添加如下内容
        export TOMCAT_HOME=/cloudstar/software/apache-tomcat-9.0.11
        export PATH=$PATH:$TOMCAT_HOME/bin
        分发环境变量
        scp ~/.bashrc bigdata002:~/.bashrc
        scp ~/.bashrc bigdata003:~/.bashrc
        scp ~/.bashrc bigdata004:~/.bashrc
        scp ~/.bashrc bigdata009:~/.bashrc
        scp ~/.bashrc bigdata010:~/.bashrc
        
        
二、修改Tomcat默认端口
vim $TOMCAT_HOME/conf/server.xml

```
原来的配置
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />


修改后的配置
<Connector port="8783" protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
```

四、验证Tomcat安装成功
    启动命令：
        $TOMCAT_HOME/bin/startup.sh
    浏览器：
        http://bigdata001:8783/