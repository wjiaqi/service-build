安装docker 集群

yum -y install docker 

配置tcp连接端口(2375)

ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375

重新加载配置

systemctl daemon-reload

开机启动

systemctl enable docker



```
初始化集群 指定ip

docker swarm init  --advertise-addr=172.25.0.30
```



```python
创建集群网络

docker network create \
--driver overlay \
--subnet 10.0.4.0/24 \
--attachable \
need-cash-test

创建单机集群
docker network create -d bridge --subnet 172.25.0.0/16 test_lyy_net

容器连接加入网络
docker run -d --network=lyy_net redis
```
创建单机网络
docker network create --driver bridge leyouyou


```python
查看加入集群命令

docker swarm join-token worker
```



```python
加入集群

docker swarm join --token SWMTKN-1-5u33b53uo759k9e5ktq9sx4szzl07p56m7b5b02gp5icpgi2sr-bej3o747ln4sjx2ffvmvh29zs 172.25.0.30:2377
```



```bash
离开集群

docker swarm leave --force
```

安装portainer web 管理界面(主节点执行)

```python
docker service create \
    --name portainer \
    --publish 9000:9000 \
    --replicas=1 \
    --constraint 'node.role == manager' \
    --mount type=volume,src=portainer_data,dst=/data \
    --mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock \
    portainer/portainer
```