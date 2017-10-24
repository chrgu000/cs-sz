一：开发环境
    前端：http://bigdata001:31112
    注册中心-Eureka：http://bigdata001:8761
    路由中心-gateway: http://bigdata003:8082
    追踪中心-Zipkin：http://bigdata002:10001
    监控中心-Monitor：http://bigdata003:10001
    kubernetes UI：http://bigdata001:8080/ui
    rabbitMQ：http://bigdata004:15672
    后端：bigdata002 || bigdata003
    存储-mysql: bigdata004/3306   用户名：root 密码：123456
    存储-redis: bigdata004/6379  
    nfs: bigdata004:/data/nfs
二：生产环境
    前端：http://bigdata005:31112
    注册中心-Eureka：http://bigdata005:8761
    路由中心-gateway: http://bigdata007:8082
    追踪中心-Zipkin：http://bigdata006:10001
    监控中心-Monitor：http://bigdata007:10001
    kubernetes UI：http://bigdata005:8080/ui
    rabbitMQ：http://bigdata008:15672
    后端：bigdata006/bigdata007
    存储-mysql: bigdata008/3306   用户名：root 密码：123456
    存储-redis: bigdata008/6379   
    nfs: bigdata008:/data/nfs
三：Common
    GitLab: http://bigdata010:10080
    Jenkins: http://bigdata010:8080/jenkins   用户名：admin  密码：Pioneer@123
    rabbitMQ: http://bigdata010:15672   用户名：guest 密码：guest
    代码ReView-Upsource: http://bigdata010:10008  
    自动化测试-SonarQube: http://bigdata010:9000   用户名：admin 密码：admin
    