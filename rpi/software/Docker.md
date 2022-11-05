# Docker

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc

sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.ustc.edu.cn/docker-ce/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo sh -c 'echo "{\n  \"registry-mirrors\": [\"https://docker.mirrors.ustc.edu.cn/\"]\n}" > /etc/docker/daemon.json'
sudo systemctl restart docker

sudo usermod -aG docker $USER
```

### 安装Aria2

> [superng6/aria2 - Docker Image | Docker Hub](https://hub.docker.com/r/superng6/aria2#!)
>

```bash
docker run -d \
--name aria2 \
--restart unless-stopped \
-v <LocalDir>:/downloads \
-v <LocalDir>:/config \
-e PUID=$UID \
-e PGID=`id -g \`whoami\`` \
-e SECRET=<Token> \
-e CACHE=1024M \
-e PORT=6800 \
-e WEBUI=true \
-e WEBUI_PORT=<PortOfWebUI> \
-e BTPORT=32516 \
-e UT=true \
-e RUT=true \
-e FA=falloc \
-p 6800:6800 \
-p <PortOfWebUI>:<PortOfWebUI> \
-p 6881:6881 \
-p 6881:6881/udp \
superng6/aria2
```

```bash
docker run -d \
--name aria2 \
--restart unless-stopped \
-v /mnt/.aria2:/downloads \
-v /home/user/.config/aria2:/config \
-e PUID=1000 \
-e PGID=1000 \
-e SECRET=19981023@Xy \
-e CACHE=1024M \
-e PORT=6800 \
-e WEBUI=true \
-e WEBUI_PORT=10000 \
-e BTPORT=32516 \
-e UT=true \
-e RUT=true \
--net host \
superng6/aria2
```

### 安装Samba

> [dperson/samba - Docker Image | Docker Hub](https://hub.docker.com/r/dperson/samba/)
>

```bash
docker run -it -d \
--name samba \
--restart unless-stopped \
-p 139:139 \
-p 445:445 \
-v <PathOfHost>:<PathOfContainer> \
-v <PathOfHost2>:<PathOfContainer2> \
dperson/samba -p \
-u "<UserName>;<Password>" \
-u "<UserName2>;<Password2>" \
-s "<ShareName>;<PathOfContainer>;yes;no;yes;all" \
-s "<ShareName2>;<PathOfContainer2>;yes;no;yes;all"
```

``` bash
docker run -it -d \
--restart unless-stopped \
--name samba \
-p 139:139 \
-p 445:445 \
-v /mnt:/mnt \
dperson/samba -p \
-u "yongx;yongx-passwd" \
-s "mnt;/mnt;yes;no;yes;all;yongx;yongx"
```

### 安装Mariadb (目前存在创建的用户没有权限创建数据库的问题 还需要解决)

> [Mariadb - Official Image | Docker Hub](https://hub.docker.com/_/mariadb/)
>

```bash
docker run -d \
--name mariadb \
--env MARIADB_USER=<UserName> \
--env MARIADB_PASSWORD=<Password> \
--env MARIADB_ROOT_PASSWORD=<Password> \
-p 3306:3306 \
mariadb:latest
```

### 安装ddns-go

> [jeessy/ddns-go - Docker Image | Docker Hub](https://hub.docker.com/r/jeessy/ddns-go)
>

```bash
docker run -d \
--name ddns-go \
--restart unless-stopped \
--net host \
-v /var/local/ddns-go:/root \
jeessy/ddns-go
```

然后访问http://<rpi’s ip>:9876进入webui进行配置

### 安装Clash & yacd

- 获取Clash订阅

    > [用户中心 — 星梦数据 (stardream.xyz)](https://stardream.xyz/user)
    >
    >
    > 复制Clash订阅, 将订阅链接中的内容保存到<ClashDirPath>/config.yaml
    >
    > [辣条 (latiao.club)](https://latiao.club/#/dashboard)
    >
- 安装Clash

    > [clash as a daemon · Dreamacro/clash Wiki (github.com)](https://github.com/Dreamacro/clash/wiki/clash-as-a-daemon#docker)
    >

```bash
docker run -d \
--name clash \
--restart unless-stopped \
# Clash配置文件路径
-v <ClashDirPath>:/root/.config/clash \
# 为了方便直接映射所有 可以指定端口
--net host \
# dreamacro/clash镜像拉取不成功 暂时使用ghcr.io/dreamacro/clash代替
ghcr.io/dreamacro/clash
```

- 安装yacd用于通过web的方式管理clash

    > [p3terx/yacd - Docker Image | Docker Hub](https://hub.docker.com/r/p3terx/yacd)
    >

    ```bash
    docker run -d \
    --name yacd \
    --log-opt max-size=1m \
    --restart unless-stopped \
    -p 9091:9091 \
    p3terx/yacd
    ```

- 打开yacd管理页面(http://<ip>:9091), 添加一个API地址 http://<ip>:9090
- 通过配置文件/yacd打开AllowLan开关, 即可在需要的机器上设置代理, 默认情况下7890代理http 7891代理socks 具体使用方法可以查询clash相关使用说明

### 安装Portainer

```bash
docker volume create portainer
docker run -d \
-p 8000:8000 \
-p 9000:9000 \
--name=portainer \
--restart=unless-stopped \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer:/data \
portainer/portainer-ce
```

### 安装Gogs

> [https://hub.docker.com/r/gogs/gogs](https://hub.docker.com/r/gogs/gogs)
>

```bash
docker run -d --name=gogs --restart=unless-stopped -p 13000:3000 -v /var/local/gogs:/data gogs/gogs-rpi
```

### Jellyfin
```bash
# render group id
cat group | grep render

docker run -d  \
--name jellyfin \
--group-add=106 \
# bind gpu
--device /dev/dri/renderD128:/dev/dri/renderD128 \
--device /dev/dri/card0:/dev/dri/card0 \
--net=host \
--volume /home/user/.config/jellyfin/config:/config \
--volume /home/user/.config/jellyfin/cache:/cache \
# real media path
--volume /home/user/.config/jellyfin/media:/media \
--restart=unless-stopped \
jellyfin/jellyfin
```
