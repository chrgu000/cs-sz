cd /root

docker -v

yum intall wget

wget https://raw.githubusercontent.com/sameersbn/docker-gitlab/master/docker-compose.yml


curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

docker-compose -v



docker-compose up -d


cd /cloudstar/software

docker-compose down

