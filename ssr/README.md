# ssr

基于 [秋水逸冰](https://github.com/teddysun/shadowsocks_install) 发布的 [一键安装脚本](https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh) 写的 `Dockerfile`。

你要做的，Clone 下来之后在 `Dockerfile` 的同一目录创建你自己的 `shadowsocks.json` 文件，然后构建生成镜像，然后上传到你自己的 [Docker Hub](https://hub.docker.com/)。之后在任何主机上部署，pull 下你自己的镜像，run 即可，省去每次要修改你的配置的步骤。

> 注意：你创建的这个镜像中是包含了自己的配置文件信息的，所以最好是 `private`。

## 构建流程

- 在 [Docker Hub](https://hub.docker.com/) （没有就先注册）创建你的 respository （最好 private），如 `ssr`，假设你的 docker hub 用户名为 `hanjun806`。
- Clone 本项目
- 在项目根目录创建 `shadowsocks.json`，编写类似如下的 ssr 配置：
 
 ```json
 {
    "server":"0.0.0.0",
    "server_ipv6":"[::]",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "9000":"xxxxxx",
        "9001":{"password":"xxxxxx", "protocol":"auth_chain_a", "obfs":"tls1.2_ticket_auth", "obfs_param":""},
        "9002":{"password":"xxxxxx", "protocol":"auth_chain_a", "obfs":"tls1.2_ticket_auth", "obfs_param":""}
        // ...
    },
    "timeout":120,
    "method":"chacha20",
    "protocol":"origin",
    "protocol_param":"",
    "obfs":"plain",
    "obfs_param":"",
    "redirect":"",
    "dns_ipv6":false,
    "fast_open":false,
    "workers":1
 }
 ```
 
- 创建 docker 镜像：cd 到本项目的根目录，运行命令 `docker build --no-cache -t hanjun806/ssr:1.0 .`。创建成功之后运行 `docker images` 确认下。
- 把你本地生成的镜像 push 到你的仓库：
 - 运行 `docker login`，确认你是登录状态，未登录则登录 `Docker Hub`。
 - 执行 `docker push hanjun806/ssr:1.0`。
 - 成功之后到你的 `Docker Hub` 查看该 respository 的 tag 是否有 `1.0` 的版本存在了。

## 使用流程

- 登录你的 vps，安装 docker，执行 `docker login` 登录。
- 运行 `docker pull hanjun806/ssr:1.0` 命令把刚刚你创建的镜像 pull 下来。
- 再运行镜像：`docker run -itd --name ssr -p 9001:9001 -p 9002:9002 hanjun806/ssr:1.0`
- 容器（容器的名字为 `ssr`）启动之后，`ssr server` 会自动跑起来。
- 根据你 vps 的系统，把 `docker run ssr` 作为开机自启。

## 客户端配置

- ip，端口，密码等用配置的
- 加密：chacha20
- 协议：auth_chain_a
- 混淆：tls1.2_ticket_auth
