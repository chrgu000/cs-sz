##gitlab项目规划
![](images/gitlab.png)
```
gitlab中把项目分开
1.前端project
2.后端project
3.配置project
4.文档管理project
```
##gitlab前端项目规划
![](images/gitlabFront.png)
```
前端project中区分相关module，每个module是完整的Vue工程，
通过package.json来管理前端依赖项。
1.合作module
2.营销module
3.宣传module
4.管理module
5.deployment中放置前端项目Dockerfile模板
9.sonar-project.properties如果使用sonar进行代码检查需要配置
10.build_push_deploy_jenkins_k8s_402.sh是Jenkins调用的入口脚本
```
##webstorm规划
![](images/webstorm02.png)
![](images/webstorm01.png)






##gitlab后端项目规划
![](images/gitlabBackend.png)
```
后端project中区分相关module，每个module是完整的maven工程，可以允许的springboot工程。
通过父子maven工程来管理通用的依赖项。
1.验证相关module
2.路由相关module
3.eureka注册中心module
4.config-server配置中心module
5.zipkin追踪中心module
6.monitor监控中心module
7.x-pay支付微服务
8.x-file文件微服务
9.deployment中放置后端项目Dockerfile模板
10.sonar-project.properties如果使用sonar进行代码检查需要配置
11.build_push_deploy_jenkins_k8s_402.sh是Jenkins调用的入口脚本
12.common中放置多个业务module中使用的工具方法和工具类库，不需要时springboot的工程
```