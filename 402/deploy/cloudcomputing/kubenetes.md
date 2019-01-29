
http://www.cnblogs.com/zhenyuyaodidiao/p/6500897.html

一、开发环境：bigdata001,bigdata002,bigdata003
1.设置yum源
    cat >> /etc/yum.repos.d/kubernetes.repo <<EOF
    [kubernetes]
    name=Kubernetes
    baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
    enabled=1
    gpgcheck=0
    EOF
    
    升级系统
    yum -y update
2.安装kubernetes
    yum install -y  kubernetes
    

3.在kubernetes master上需要运行以下组件：
　　　　Kubernets API Server
　　　　Kubernets Controller Manager
　　　　Kubernets Scheduler
    3.1配置Kubernets API Server
        编辑命令
            vim /etc/kubernetes/apiserver
        主要关注的配置项
            KUBE_API_ADDRESS="--insecure-bind-address=0.0.0.0"
            KUBE_API_PORT="--port=8080"
            KUBE_ETCD_SERVERS="--etcd-servers=http://bigdata001:2379"
            KUBE_ADMISSION_CONTROL="--admission-control=NamespaceLifecycle,NamespaceExists,LimitRanger,SecurityContextDeny,ResourceQuota"
            KUBE_API_ARGS="--service-node-port-range=1000-65535"
            KUBELET_ARGS="--max-pods=20000"
        分发配置
            scp /etc/kubernetes/apiserver bigdata002:/etc/kubernetes/apiserver
            scp /etc/kubernetes/apiserver bigdata003:/etc/kubernetes/apiserver
    
    3.2配置kubernetes
        编辑命令
            vim /etc/kubernetes/config
        主要配置项
            KUBE_MASTER="--master=http://bigdata001:8080"
        分发配置
            scp /etc/kubernetes/config bigdata002:/etc/kubernetes/config
            scp /etc/kubernetes/config bigdata003:/etc/kubernetes/config

    3.3启动master
        systemctl enable kube-apiserver.service
        systemctl start kube-apiserver.service
        systemctl enable kube-controller-manager.service
        systemctl start kube-controller-manager.service
        systemctl enable kube-scheduler.service
        systemctl start kube-scheduler.service

4.在kubernetes node上需要运行以下组件：
        在kubernetes node上需要运行以下组件：
        　　　　Kubelet
        　　　　Kubernets Proxy
    4.1配置kubelet
         编辑命令
             vim /etc/kubernetes/kubelet
         关注项     
            KUBELET_ADDRESS="--address=0.0.0.0"
            KUBELET_HOSTNAME="--hostname-override=bigdata001"
            KUBELET_API_SERVER="--api-servers=http://bigdata001:8080"
         分发配置
            scp /etc/kubernetes/kubelet  bigdata002:/etc/kubernetes/kubelet
            scp /etc/kubernetes/kubelet  bigdata003:/etc/kubernetes/kubelet
         修改节点名称，根据实际情况来
            KUBELET_HOSTNAME="--hostname-override=bigdata002"
            KUBELET_HOSTNAME="--hostname-override=bigdata003"
    4.2启动kubernetes node
            systemctl enable kubelet.service
            systemctl start kubelet.service
            systemctl enable kube-proxy.service
            systemctl start kube-proxy.service

5.查看节点状态
    kubectl -s http://bigdata001:8080 get node
    返回如下配置说明配置成功
    NAME         STATUS    AGE
    bigdata001   Ready     1m
    bigdata002   Ready     1m
    bigdata003   Ready     1m





主要的集群为何命令
systemctl  stop kube-apiserver kube-controller-manager kube-scheduler kubelet kube-proxy

systemctl enable kube-apiserver.service
systemctl start kube-apiserver.service
systemctl enable kube-controller-manager.service
systemctl start kube-controller-manager.service
systemctl enable kube-scheduler.service
systemctl start kube-scheduler.service



systemctl enable kubelet.service
systemctl start kubelet.service
systemctl enable kube-proxy.service
systemctl start kube-proxy.service





重启flannel
systemctl stop flanneld.service 
systemctl start flanneld.service 

重启master
systemctl restart kube-apiserver.service
systemctl restart kube-controller-manager.service
systemctl restart kube-scheduler.service
重启slave
systemctl restart  kubelet.service
systemctl restart  kube-proxy.service
查看节点信息
kubectl -s http://bigdata03:8080 get node

停止集群
systemctl stop kube-apiserver.service kube-scheduler.service kube-controller-manager.service
systemctl stop  kubelet.service kube-proxy.service

systemctl start kube-apiserver.service kube-scheduler.service kube-controller-manager.service
systemctl start  kubelet.service kube-proxy.service



设置端口分配范围
vim /etc/kubernetes/apiserver 
KUBE_API_ARGS="--service-node-port-range=1000-65535"
KUBELET_ARGS="--max-pods=200"

