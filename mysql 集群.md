- ### mysql安装

  先下载 mysql源安装包

  wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm

  安装mysql源

  yum -y localinstall mysql57-community-release-el7-11.noarch.rpm 

  在线安装mysql

  yum -y install mysql-community-server

  启动mysql

  systemctl start mysqld

  设置开机启动

  systemctl enable mysqld

  systemctl daemon-reload

  修改mysql登录密码

  vi /var/log/mysqld.log   #查看临时密码

  进入mysql

  mysql -u root -p

  **修改密码

  ALTER USER 'root'@'localhost' IDENTIFIED BY 'ZhipengWang2012@';

  **远程登录

  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'ZhipengWang2012@' WITH GRANT OPTION;

  **关闭防火墙

  

  **配置默认编码

  vi /etc/my.cnf

  [mysqld]

  character_set_server=utf8

  init_connect='SET NAMES utf8'

  systemctl restart mysqld

- 

- ### 查看master状态

    SHOW MASTER STATUS;

- ### 配置slave

  \#登陆在140，141据库，在slave上链接master

  - change master to master_host='172.31.7.138',master_user='root',master_password='Lyy0977#Mu**',master_log_file='mysql-bin.000001',master_log_pos=154;

  \#参数配置

  master_host：主节点mysql的IP地址

  master_user：主节点mysql登陆用户的用户名

  master_password：主节点mysql登陆用户的密码

  master_log_file：主节点mysql的日志文件指定

  master_log_pos：主节点mysql的Ppsition的ID

- ### 启动从服务

  start slave;

  

- ### 查看slave状态

  show slave status\G

