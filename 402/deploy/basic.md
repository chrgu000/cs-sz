
####################################################################################################################################
一、修改机器信息
####################################################################################################################################
0.关闭selinux
    sed -i '/SELINUX/s/enforcing/disabled/' /etc/selinux/config
    setenforce 0
1.关闭防火墙
    systemctl stop firewalld && systemctl disable firewalld
    
2.修改机器名称,不用重启机器就能看到修改的办法
     hostnamectl --static set-hostname bigdata03


3.配置hosts文件
    vim /etc/hosts
    写入
	10.99.22.21       bigdata001
	10.99.22.22       bigdata002
	10.99.22.23       bigdata003
	10.99.22.24       bigdata004
	10.99.21.21       bigdata005
	10.99.21.22       bigdata006
	10.99.21.23       bigdata007
	10.99.21.24       bigdata008
	10.99.21.25       bigdata009
	10.99.21.26       bigdata010
	
    分发配置文件,首次分发需要输入密码Pioneer@123
    scp /etc/hosts bigdata002:/etc/hosts
    scp /etc/hosts bigdata003:/etc/hosts
    scp /etc/hosts bigdata004:/etc/hosts
    scp /etc/hosts bigdata005:/etc/hosts
    scp /etc/hosts bigdata006:/etc/hosts
    scp /etc/hosts bigdata007:/etc/hosts
    scp /etc/hosts bigdata008:/etc/hosts
    scp /etc/hosts bigdata009:/etc/hosts
    scp /etc/hosts bigdata010:/etc/hosts


4.配置集群免密登陆
    各个机器上生产新的秘钥
    rm -rf /root/.ssh/*
    ssh-keygen -t rsa
    cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys

    汇聚到bigdata001
    ssh-copy-id -i  bigdata001

    分发到其他机器
    scp /root/.ssh/authorized_keys bigdata002:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata003:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata004:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata005:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata006:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata007:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata008:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata009:/root/.ssh/
    scp /root/.ssh/authorized_keys bigdata010:/root/.ssh/

5.禁用ipv6
    echo 1>/proc/sys/net/ipv6/conf/all/disable_ipv6
    echo 1>/proc/sys/net/ipv6/conf/default/disable_ipv6
    
6.设置电信dns
    查看网络连接
        nmcli connection show
    设置网络连接
        nmcli con mod ens33 ipv4.dns "114.114.114.114 8.8.8.8"
        nmcli con up ens33
        
7.安装基础工具 
        yum -y install vim 