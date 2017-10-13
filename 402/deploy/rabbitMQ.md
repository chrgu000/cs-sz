http://www.jianshu.com/p/c40166cb4e86


一、开发环境：bigdata004

1.启动
docker run -d --name rabbitmq --publish 5671:5671 \
 --publish 5672:5672 --publish 4369:4369 --publish 25672:25672 --publish 15671:15671 --publish 15672:15672 \
rabbitmq:management

2.访问url
http://bigdata004:15672

userName: guest
passWord: guest


二、开发环境：bigdata008