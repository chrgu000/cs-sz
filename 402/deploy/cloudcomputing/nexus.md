参考文档
https://blog.csdn.net/qq_36357820/article/details/76184400
http://www.cnblogs.com/qq27271609/tag/Nexus/
https://help.sonatype.com/repomanager3/rest-and-integration-api

安装机器和目录
bigdata009
cd /cloudstar/software


下载和解压
wget https://sonatype-download.global.ssl.fastly.net/repository/repositoryManager/3/nexus-3.12.0-01-unix.tar.gz
tar -zxvf nexus-3.12.0-01-unix.tar.gz


配置文件
cd nexus-3.12.0-01/bin/
vim nexus
 
 配置jdk
INSTALL4J_JAVA_HOME_OVERRIDE=/cloudstar/software/jdk1.8.0_151


3、  修改使用的用户

vi nexus.rc
run_as_user="root"

4.启动nexus服务
cd /cloudstar/software
./nexus start

sudo /cloudstar/software/nexus-3.12.0-01/bin/nexus start


修改默认端口
vim ../etc/nexus-default.properties
默认
application-port=8081
改为
application-port=9081


登录验证
http://bigdata009:9081
用户名 admin
密码   admin123
 
 