docker run --name redis -p 6379:6379 -v /data/redis/data:/data  -d redis redis-server  --appendonly yes
