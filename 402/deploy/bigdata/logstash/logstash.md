参考链接
https://www.cnblogs.com/swordfall/p/8860941.html
https://blog.csdn.net/lwei_998/article/details/77869728



下载、解压
wget https://artifacts.elastic.co/downloads/logstash/logstash-6.4.0.tar.gz
tar -zxvf logstash-6.4.0.tar.gz

配置、分发环境变量

vim ~/.bashrc
添加如下内容
export LOGSTASH_HOME=/cloudstar/software/logstash-6.4.0
export PATH=$PATH:$LOGSTASH_HOME/bin
分发环境变量
scp ~/.bashrc bigdata002:~/.bashrc
scp ~/.bashrc bigdata003:~/.bashrc
scp ~/.bashrc bigdata004:~/.bashrc
scp ~/.bashrc bigdata009:~/.bashrc
scp ~/.bashrc bigdata010:~/.bashrc


修改监控监听端口
vim $LOGSTASH_HOME/binlogstash.yml
```
http.host: "127.0.0.1"
http.host: "0.0.0.0"
```


测试
 logstash -e 'input { stdin { } } output { stdout {} }' &
 
 
 
测试监控接口
curl -XGET 'bigdata001:9600/?pretty'
 
 

 
 
 
 
第一个配置
vim /cloudstar/software/logstash-6.4.0/fileTest1.conf
 
input {
 file {
     path => "/cloudstar/software/logstashTest/*/*.txt"
 }
}
output {
 stdout {}
}

logstash  -f /cloudstar/software/logstash-6.4.0/fileTest1.conf


























以下还没有试验成功

输入源为txt文件，输出源为kafka
vim /cloudstar/software/logstash-6.4.0/fileTest3.conf

input {
   file {
      codec => plain {
        charset => "UTF-8"
      }
      path => "/cloudstar/software/logstashTest/*/*.txt"
      discover_interval => 30
      start_position => "beginning"
   }
}
output {
   kafka {
       topic_id => "dagu"
       codec => plain {
          charset => "UTF-8"
       }
       bootstrap_servers => "bigdata001:9092,bigdata002:9092,bigdata003:9092"
   }
}







vim /cloudstar/software/logstash-6.4.0/fileTest4.conf


input {
    file{
        path => "/cloudstar/software/logstashTest/1.txt"
    }
}
output {
    kafka {
        codec => json
        topic_id => "dagu"
        bootstrap_servers => "bigdata001:9092,bigdata002:9092,bigdata003:9092"
        batch_size => 1
    }
}



mkdir /cloudstar/software/logstashTest/


logstash  -f /cloudstar/software/logstash-6.4.0/fileTest4.conf