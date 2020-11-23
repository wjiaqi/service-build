# 安装redis

*# cd /software* 

wget http://download.redis.io/releases/redis-5.0.4.tar.gz

tar zxf redis-5.0.4.tar.gz && mv redis-5.0.4/ /usr/local/redis

cd /usr/local/redis && make && make install



# 创建集群的文件夹

 mkdir /usr/local/redis/cluster

cp /usr/local/redis/redis.conf /usr/local/redis/cluster/redis_7001.conf

 cp /usr/local/redis/redis.conf /usr/local/redis/cluster/redis_7002.conf

chown -R redis:redis /usr/local/redis

mkdir -p /data/redis/cluster/{redis_7001,redis_7002} && chown -R redis:redis /data/redis

# 修改集群中的redis配置

bind 192.168.30.128 

port 7001 

daemonize yes 

pidfile "/var/run/redis_7001.pid" 

logfile "/usr/local/redis/cluster/redis_7001.log" 

dir "/data/redis/cluster/redis_7001" 

masterauth 123456 

requirepass 123456 

appendonly yes 

cluster-enabled yes 

cluster-config-file nodes_7001.conf 

cluster-node-timeout 15000

# 创建集群

放开安全组 端口+ 总线端口（+10000）

启动服务

*/usr/local/bin/redis-server /usr/local/redis/cluster/redis_7001.conf*

连接服务

redis-cli -c -h 172.31.7.138 -p 7002 -a '****'

创建集群

*redis-cli -a 'lyy89**!ll' --cluster create 172.31.7.138:7001 172.31.7.138:7002 172.31.3.119:7003 172.31.3.119:7004 172.31.11.40:7005 172.31.11.40:7006 --cluster-replicas 1



redis-cli -a 'lyy89**!ll' --cluster create 18.162.167.89:7001 18.162.167.89:7002 18.162.169.94:7003 18.162.169.94:7004 18.163.184.200:7005 18.163.184.200:7006 --cluster-replicas 1

https://blog.csdn.net/miss1181248983/article/details/90056960