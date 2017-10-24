1.使用docker安装mysql
    docker run --name mysql -p 3306:3306  -v /data/mysql/data:/var/lib/mysql -e MYSQL\_ROOT\_PASSWORD=123456 -d mysql  --lower_case_table_names=1
2.登录mysql内部操作
    docker run -it mysql /bin/bash
3.