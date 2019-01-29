ssh root@125.208.28.242 -o TCPKeepAlive=yes -o ServerAliveInterval=60
ssh root@125.208.28.242 -o TCPKeepAlive=yes -o ServerAliveInterval=60 -o ClientAliveInterval 60 -o ClientAliveCountMax 1


1.下载
wget http://emqtt.io/static/brokers/emqx-centos7-v3.0-beta.2.zip
2.安装
 unzip emqx-centos7-v3.0-beta.2.zip

3.配置
cd emqttd
vim etc/emq.conf
    配置如下内容
```
a.配置node name
## Node Args
node.name = emqx@bigdata001


b.配置TCP 服务端口占用
EMQ 2.0 消息服务器默认占用的 TCP 端口包括:
EMQ 2.0 占用的上述端口，可通过 etc/emq.conf 配置文件的 ‘listener’ 段落设置:
listener.api.mgmt = 0.0.0.0:8183

```

4、常用端口
```
1883 MQTT 协议端口
8883 MQTT/SSL 端口
8083 MQTT/WebSocket 端口
8080 HTTP API 端口
18083 Dashboard 管理控制台端口
EMQ 2.0 占用的上述端口，可通过 etc/emq.conf 配置文件的 ‘listener’ 段落设置:
## TCP Listener: 1883, 127.0.0.1:1883, ::1:1883
listener.tcp.external = 0.0.0.0:1883

## SSL Listener: 8883, 127.0.0.1:8883, ::1:8883
listener.ssl.external = 8883

## External MQTT/WebSocket Listener
listener.ws.external = 8783

## Examples: 8084, 127.0.0.1:8084, ::1:8084
listener.wss.external = 8784

## HTTP Management API Listener
listener.api.mgmt = 0.0.0.0:8785
```


4.常用命令
  控制台方式启动服务：
    ./bin/emqttd console
  后台启动服务
    ./bin/emqttd start
  关闭服务
    ./bin/emqttd stop
  检查服务状态
    ./bin/emqttd_ctl status
5.验证服务启动
    http://bigdata009:18083
    默认用户: admin，密码：public

【
6.配置集群
在chinac242上重新安装
7.加入集群
 ./bin/emqttd_ctl cluster join bigdata009@127.0.0.1
8.查看集群状态
[root@chinac242 emqttd]# ./bin/emqttd_ctl cluster status

】








http://bigdata009:18083/#/http_api
http://emqtt.io/docs/v2/rest.html#