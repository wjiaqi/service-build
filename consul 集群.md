启动分布式consul

主

```text
docker run --network=need-cash --name=formal-consul -d -p 8500:8500  -p 8600:8600 -p 8600:8600/udp -p 8300:8300 -p 8301:8301 -p 8301:8301/udp -p 8302:8302 -p 8302:8302/udp consul agent -server -bootstrap -ui -client=0.0.0.0 -advertise=172.31.30.253
```

docker run --network=need-cash-test  --name=trial-consul -d -p 18500:8500  -p 18600:8600 -p 18600:8600/udp -p 18300:8300 -p 18301:8301 -p 18301:8301/udp -p 18302:8302 -p 18302:8302/udp consul agent -server -bootstrap -ui -client=0.0.0.0 -advertise=172.31.30.253

从

```text
docker run --network=lyy_net -d -p 8500:8500 -p 8600:8600 -p 8600:8600/udp -p 8300:8300 -p 8301:8301 -p 8301:8301/udp -p 8302:8302 -p 8302:8302/udp consul agent -server -join=172.31.19.86 -client=0.0.0.0 -advertise=172.31.30.253
```

从

```text
docker run --network=lyy_net  -d -p 8500:8500 -p 8600:8600 -p 8600:8600/udp -p 8300:8300 -p 8301:8301 -p 8301:8301/udp -p 8302:8302 -p 8302:8302/udp consul agent -server -join=172.31.19.86 -client=0.0.0.0 -advertise=172.31.26.179
```

测试服consul:

docker run --network=test_lyy_net  --name=trial-consul -d -p 18500:8500  -p 18600:8600 -p 18600:8600/udp -p 18300:8300 -p 18301:8301 -p 18301:8301/udp -p 18302:8302 -p 18302:8302/udp consul agent -server -bootstrap -ui -client=0.0.0.0 -advertise=172.31.30.253

### Docker Swarm启动Registrator

1. 拉取gliderlabs/registrator镜像

   docker pull gliderlabs/registrator

2.  在Swarm Manager上为所有Swarm主机上启动Registrator

```text
docker service create --name=registrator --mount type=bind,src=/var/run/docker.sock,dst=/tmp/docker.sock --mode global --network=lyy_net gliderlabs/registrator -deregister="always" -internal consul://172.31.19.86:8500
```

 Note:以下是命令中相关参数的说明
`--name`设定该服务的名称是registrator
`--mount`设定每个容器挂载主机的目录，这是Registrator启动所要求的
`--mode`设定global表示每个Swarm主机都要启动一个Registrator实例
`--network`设定该服务内所有实例都属于`micro`这个Overlay网络
`-deregister`设定"always"表示服务下线时自动执行服务注销
`-internal`设定Registrator可以自动注册网络内部的实例，方便网络内实例互相调用
`consul://172.31.19.86:8500`指明了Consul的服务地址，写consul集群中其他实例的地址也可以  