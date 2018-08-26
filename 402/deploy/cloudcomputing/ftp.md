bigdata001上安装
参考链接
https://blog.csdn.net/Han_Wen2015/article/details/76794842


安装ftp
 yum install -y vsftpd
 
查看位置 
 whereis vsftpd
 
查看状态
systemctl status vsftpd.service


启动ftp
 systemctl start vsftpd.service
开机启动
systemctl enable vsftpd.service



配置vsftpd.conf文件
     vim /etc/vsftpd/vsftpd.conf
        最后添加
            userlist_deny=NO
            local_root=/var/public_root
            use_localtime=YES
            allow_writeable_chroot=YES




编辑user_list文件,主要用于配置ftp的用户
    vim /etc/vsftpd/user_list



建根目录，并设置访问权限
    mkdir /var/public_root
    chown -R root /var/public_root
    chmod -R 755 /var/public_root

 