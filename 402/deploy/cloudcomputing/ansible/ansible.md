 bigdata001
 
 
 一、安装 Ansible
 
 shell > yum -y install ansible
 二、配置 Ansible

 shell > ls /etc/ansible   # ansible.cfg 是 Ansible 工具的配置文件；hosts 用来配置被管理的机器；roles 是一个目录，playbook 将使用它
 ansible.cfg hosts roles