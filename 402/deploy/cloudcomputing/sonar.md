
0.参考链接
http://blog.csdn.net/jamenew/article/category/6700114
          http://blog.csdn.net/abcdocker/article/details/53840582
1.下载并解压sonar
wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-5.6.zip
unzip sonarqube-5.6.zip

wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-2.8.zip
unzip sonar-scanner-2.8.zip
2.配置sonar的数据库
CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL ON sonar.* TO 'sonar'@'*' IDENTIFIED BY 'sonar@pw';
GRANT ALL ON sonar.* TO 'sonar'@'%' IDENTIFIED BY 'sonar@pw';
FLUSH PRIVILEGES;
3.启动Sonar 
　　  你可以在Sonar的配置文件来配置Sonar Web监听的IP地址和端口，默认是9000端口。
          命令
          vim sonar.properties
          内容
sonar.web.host=0.0.0.0
sonar.web.port=9000

        启动：
            cd /cloudstar/tool/sonarqube-5.6/bin/linux-x86-64
            ./sonar.sh  start

        验证：
 


浏览器验证
    http://10.100.134.2:9000/

    用户：admin
    密码：admin

