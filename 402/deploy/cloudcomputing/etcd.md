http://www.cnblogs.com/zhenyuyaodidiao/p/6500830.html
http://www.cnblogs.com/zhenyuyaodidiao/p/6500830.html
一、开发环境部署：bigdata001

1.规划
    先部署单节点etcd后期扩展为cluster模式
    开发环境  bigdata001
2.安装
    yum install etcd -y
3.配置
    vim /etc/etcd/etcd.conf
    
    主要关注的配置项如下
    # [member]
    ETCD_NAME=bigdata001
    ETCD_DATA_DIR="/var/lib/etcd/default.etcd"
    ETCD_LISTEN_CLIENT_URLS="http://0.0.0.0:2379,http://0.0.0.0:4001"
    #[cluster]  
    ETCD_ADVERTISE_CLIENT_URLS="http://bigdata001:2379,http://bigdata001:4001"

4.启动
  systemctl start etcd  &&   systemctl enable etcd 
5.验证
   命令：
       etcdctl -C http://bigdata001:4001 cluster-health
   返回：
       member 8e9e05c52164694d is healthy: got healthy result from http://bigdata001:2379
       cluster is healthy
6.对etcd进行操作
       etcdctl set testdir/testkey0 0
       etcdctl get testdir/testkey0 
二、生产环境部署（参考开发环境）： bigdata005   