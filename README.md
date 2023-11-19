# ![Hysteria 2](logo.svg)

# 支持对接V2board面板的Hysteria2后端

### 项目说明
本项目基于hysteria官方内核二次开发，添加了从v2b获取节点信息、用户鉴权信息与上报用户流量的功能。
性能方面已经由hysteria2内核作者亲自指导优化过了。

### TG交流群
欢迎加入交流群 [点击加入](https://t.me/+DcRt8AB2VbI2Yzc1)

### 安装docker，docker compose
```
curl -fsSL https://get.docker.com | bash -s docker
sudo systemctl start docker
sudo systemctl enable docker
docker --version
docker compose version
```
### 下载并修改配置文件docker-compose.yml,server.yaml,包括前端信息和后端域名
```
cd /etc
mkdir hysteria
git clone https://github.com/Vincent-Ksr/hysteria2-v2b.git hysteria
cd /etc/hysteria
```
### 示例配置
```
v2board:
  apiHost: https://面板域名 
  apiKey: 节点密钥
  nodeID: 节点ID
tls:
  type: tls
  cert: /etc/hysteria/tls.crt #此处是你的证书路径
  key: /etc/hysteria/tls.key  #此处是你的公钥路径
auth:
  type: v2board
trafficStats:
  listen: 127.0.0.1:7653
acl: 
  inline: 
    - reject(10.0.0.0/8)
    - reject(172.16.0.0/12)
    - reject(192.168.0.0/16)
    - reject(127.0.0.0/8)
    - reject(fc00::/7)
```
### 配置文件docker-compose.yml
暂时无需改动
> 其他配置完全与hysteria文档的一致，可以查看hysteria2官方文档 [点击查看](https://hysteria.network/zh/docs/getting-started/Installation/) 
### 一键acme证书
自行安装openssl、curl、socat
```
#Debian/ubuntu
apt-get install openssl
apt-get install curl
apt-get install socat
#centos
yum install -y openssl curl socat
#以下是证书申请和安装
curl https://get.acme.sh | sh -s email=office121233@outlook.com
~/.acme.sh/acme.sh --upgrade --auto-upgrade
可选：切换申请letsencrypt的证书，~/.acme.sh/acme.sh --set-default-ca --server letsencrypt
example.com换成你自己的后端vps绑定域名
~/.acme.sh/acme.sh --issue -d example.com --standalone
~/.acme.sh/acme.sh --install-cert -d example.com --key-file /etc/hysteria/example.com.key
~/.acme.sh/acme.sh --install-cert -d example.com --fullchain-file /etc/hysteria/example.com.crt
```

### 启动docker compose
```
docker compose up -d
查看日志：
docker logs -f hysteria-hysteria2-1
```
### docker 仓库
```
docker pull ghcr.io/cedar2025/hysteria:v1.0.5
```
