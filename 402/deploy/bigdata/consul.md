https://www.jianshu.com/p/d16a1bea5cbb




docker run -h node1  --name consul -d -v /data:/data --restart=always\
    -p   8300:8300 \
    -p   8301:8301 \
    -p   8301:8301/udp \
    -p   8302:8302 \
    -p   8302:8302/udp \
    -p   8400:8400 \
    -p   8500:8500 \
progrium/consul -server \
-bootstrap-expect 3 \
-advertise 10.68.9.4



docker run -h node2 --name consul -d -v /data:/data   --restart=always\
    -p   8300:8300 \
    -p   8301:8301/udp \
    -p   8302:8302 \
    -p   8302:8302/udp \
    -p   8400:8400 \
    -p   8500:8500 \
progrium/consul -server \
-advertise 10.68.9.5 \
-join  10.68.9.4

docker run -h node3 --name consul -d -v /data:/data   --restart=always\
    -p   8300:8300 \
    -p   8301:8301/udp \
    -p   8302:8302 \
    -p   8302:8302/udp \
    -p   8400:8400 \
    -p   8500:8500 \
progrium/consul -server \
-advertise 10.68.9.6 \
-join  10.68.9.4



http://10.68.9.4:8500/