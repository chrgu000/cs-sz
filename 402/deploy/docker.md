
####################################################################################################################################
四、docker部署
####################################################################################################################################
5.安装docker
    1.安装：
        yum install -y docker
    2. 启动&开机自启动
        systemctl enable docker && systemctl start docker
    3.重置加速镜像
    vim  /etc/docker/daemon.json
    {"registry-mirrors": ["http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"]}
    
    echo {"registry-mirrors": ["http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"]} >> /etc/docker/daemon.json
    
    4.重启docker服务
         systemctl restart docker
    5.测试最小镜像
         docker run hello-world
    如果要开启docker远程访问，可以参考如下链接
         http://blog.csdn.net/faryang/article/details/75949611
####################################################################################################################################
五、docker私有仓库
####################################################################################################################################

一、搭建docker私有仓库
客户端
    1.服务端使用http协议
        echo "other_args='--selinux-enabled --insecure-registry 10.99.21.26:5000'" >> /etc/sysconfig/docker
    2.docker常用命令
        service docker restart && service docker start
服务端
    2.下载registry镜像
        docker pull registry

    5.启动registry镜像
        docker run -d -p 5000:5000  --restart=always  -v /opt/registry:/tmp/registry  registry

        参数说明：
        -v /opt/registry:/tmp/registry :默认情况下，会将仓库存放于容器内的/tmp/registry目录下，指定本地目录挂载到容器
        此容器要用 docker stop 进行停止
    6.测试仓库运行地址
        浏览地址： http://10.99.21.26:5000/v2/_catalog
        效果示例： {"repositories":[]}

二、测试私有仓库
    1.本地使用http
        在mac中通过docker-tools来设置如下参数
        {
            "insecure-registries": [
                "10.99.21.26:5000"
            ]
        }
    2.本地下载
        docker pull busybox

    3.本地tag
        docker tag busybox 10.99.21.26:5000/busybox

    4.本地查看image
        docker images

    5.上传到私有仓库
        docker push 10.99.21.26:5000/busybox

    6.验证上传成功
        浏览地址： http://10.99.21.26:5000/v2/_catalog
        效果样例： {"repositories":["busybox"]}

三、可能出现的错误
    1.错误提示
        http: server gave HTTP response to HTTPS client
    2.错误原因
        docker repository默认使用https协议进行通信，一般情况下我们用http协议可以满足要求。
        所以只要在server和client任何一方导致的协议不一致都会出现类似的问题。
    3.解决方法
        server端使用http协议
        client端使用http协议

    如果出现如下错误
        http: server gave HTTP response to HTTPS client
    解决方法如下：
        1.修改配置文件
            vim /etc/docker/daemon.json
            填写如下内容
    {
        "registry-mirrors": ["http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn","10.99.21.26:5000"],
        "insecure-registries":["10.99.21.26:5000"]
    }
    
        2.分发配置文件
            scp /etc/docker/daemon.json  bigdata002:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata003:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata004:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata005:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata006:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata007:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata008:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata009:/etc/docker/daemon.json
            scp /etc/docker/daemon.json  bigdata010:/etc/docker/daemon.json
    
        3.重启docker服务
            systemctl restart docker

        4.上传到私有仓库
            docker push 10.99.21.26:5000/busybox
            
        5.验证上传成功
            浏览地址： http://10.99.21.26:5000/v2/_catalog
            效果样例： {"repositories":["busybox"]}

