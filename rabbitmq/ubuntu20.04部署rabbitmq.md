#### 1.创建脚本并执行

```bash
#!/bin/sh

## If sudo is not available on the system,
## uncomment the line below to install it
# apt-get install -y sudo

sudo apt-get update -y

## Install prerequisites
sudo apt-get install curl gnupg -y

## Install RabbitMQ signing key
curl -fsSL https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo apt-key add -

## Install apt HTTPS transport
sudo apt-get install apt-transport-https

## Add Bintray repositories that provision latest RabbitMQ and Erlang 23.x releases
sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list <<EOF
deb https://dl.bintray.com/rabbitmq-erlang/debian bionic erlang
deb https://dl.bintray.com/rabbitmq/debian bionic main
EOF

## Update package indices
sudo apt-get update -y

## Install rabbitmq-server and its dependencies
sudo apt-get install rabbitmq-server -y --fix-missing
```

#### 2.安装web访问插件

```bash
rabbitmq-plugins enable rabbitmq_management
```

#### 3.创建远程访问用户

```bash
#1.创建一个test用户：
rabbitmqctl add_user user_name user_passwd
 
2.设置该用户为administrator角色：
rabbitmqctl set_user_tags user_name  administrator
 
#3.设置权限：
rabbitmqctl  set_permissions  -p  '/'  user_name '.' '.' '.'

#4.重启rabbitmq服务：
systemctl restart  rabbitmq-server.service
```

