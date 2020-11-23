nginx 启动（正式官网站点）

docker run -d --name nginx-need-cash  --network=need-cash -p 80:80 -v /var/opt/formal/project:/usr/share/nginx/html  -v /var/opt/formal/nginx/nginx.conf:/etc/nginx/nginx.conf  nginx:latest

// 测试
docker run -d --name nginx-need-cash-test  --network=need-cash-test -p 8077:80 -p 8076:81 -p 8075:82 -v /var/opt/trial/project:/usr/share/nginx/html  -v /var/opt/trial/nginx/nginx.conf:/etc/nginx/nginx.conf  nginx:latest

后台pm

docker run -d --name lyy-pm-site  --network=lyy_net   -p 80:80 -v /var/opt/web-pm:/usr/share/nginx/html  -v /var/opt/nginx/nginx.conf:/etc/nginx/nginx.conf  nginx:latest

根据hyperf 基础镜像启动(挂载盘方式)

docker run -d -i -t --name lyy-client -p 9501:9501 -v /Users/admin/Documents/LEYOUYOU/client-server:/opt/www 597c28fb076b  /bin/sh

创建镜像

docker build -t lyy-hyperf/client-server:v1.0  .

运行镜像

docker run -i -t -d --name lyy-client -p 9501:9501 [镜像id] /bin/sh

运行镜像(swarm网络)

docker run -i -t -d --name lyy-center-server  --network=lyy_net -p 9501:9501 [镜像id] /bin/bash

第三方镜像登录，打标签，提交

```
docker login --username=深圳乐悠悠互娱 registry.cn-hongkong.aliyuncs.com
docker tag [ImageId] registry.cn-hongkong.aliyuncs.com/need_cash/client-server:[镜像版本号]
docker push registry.cn-hongkong.aliyuncs.com/need_cash/client-server:[镜像版本号]
```

```python
docker network create \
--driver overlay \
--subnet 10.0.2.0/24 \
--opt encrypted \
--attachable \
test_lyy_net
```