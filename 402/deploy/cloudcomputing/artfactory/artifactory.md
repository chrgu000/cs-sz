

参考地址
https://blog.csdn.net/yanggd1987/article/details/78302003

一、下载并解压
wget https://akamai.bintray.com/ee/ee242486476a94869fdeaf0cfc3233bed9a4566d603ce8078bcb02386e9463cb?__gda__=exp=1537946337~hmac=212ae4dee1a101809bcf90158ee034627b7fb2a87fdb3063cf118ec41b0b6f18&response-content-disposition=attachment%3Bfilename%3D%22jfrog-artifactory-oss-6.3.3.zip%22&response-content-type=application%2Foctet-stream&requestInfo=U2FsdGVkX1-WY-Od0GxwKnqHzff2ilwpRw7ajs3ss-hVkB3B1uM8qGFr3oJrI2nw7r8edEP26q0FpYMjU9sR-p7UDzyug-0SAn8Ryb5taHVAbh_PT9JXYCOw57KfKydRoWANecwMYamxlYc6QbGvbkewrdrPxF9_RMA_W3SKO7U&response-X-Checksum-Sha1=002cb31a45e5bcd78aee47a724fc31320fce17c9&response-X-Checksum-Sha2=ee242486476a94869fdeaf0cfc3233bed9a4566d603ce8078bcb02386e9463cb
unzip  artifactory-oss-6.3.3.zip

二、更换服务端口
cd /cloudstar/software/artifactory-oss-6.3.3/tomcat/conf
vim server.xml
```xml
<Server port="8015" shutdown="SHUTDOWN">

    <Service name="Catalina">
        <Connector port="8081" sendReasonPhrase="true" relaxedPathChars='[]' relaxedQueryChars='[]'/>

        <!-- Must be at least the value of artifactory.access.client.max.connections -->
        <Connector port="8040" sendReasonPhrase="true" maxThreads="50"/>

        <!-- This is the optional AJP connector -->
        <Connector port="8019" protocol="AJP/1.3" sendReasonPhrase="true"/>

        <Engine name="Catalina" defaultHost="localhost">
            <Host name="localhost" appBase="webapps" startStopThreads="2"/>
        </Engine>
    </Service>

</Server>
```
更改为：

```xml

<Server port="8600" shutdown="SHUTDOWN">

    <Service name="Catalina">
        <Connector port="8601" sendReasonPhrase="true" relaxedPathChars='[]' relaxedQueryChars='[]'/>

        <!-- Must be at least the value of artifactory.access.client.max.connections -->
        <Connector port="8602" sendReasonPhrase="true" maxThreads="50"/>

        <!-- This is the optional AJP connector -->
        <Connector port="8603" protocol="AJP/1.3" sendReasonPhrase="true"/>

        <Engine name="Catalina" defaultHost="localhost">
            <Host name="localhost" appBase="webapps" startStopThreads="2"/>
        </Engine>
    </Service>

</Server>

```


三、配置artifactory


cd /cloudstar/software/artifactory-oss-6.3.3/bin
# vim artifactory.default

```

export ARTIFACTORY_HOME=/cloudstar/software/artifactory-oss-6.3.3
export ARTIFACTORY_USER=artifactory
export JAVA_HOME=/cloudstar/software/jdk1.8.0_151
export START_LOCAL_REPLICATOR=true

```



四、启动
cd /cloudstar/software/artifactory-oss-6.3.3/bin
/artifactory.sh start



五、验证
http://bigdata009:8601
默认用户名/密码：admin/password


六、rest api
https://www.jfrog.com/confluence/display/RTF/Artifactory+REST+API

https://www.jfrog.com/confluence/display/RTF