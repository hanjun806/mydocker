

 
- 创建 docker 镜像：cd 到本项目的根目录，运行命令 `docker build -t hanjun806/nginx-stream:1.0 .`。创建成功之后运行 `docker images` 确认下。
- 把你本地生成的镜像 push 到你的仓库：
 - 运行 `docker login`，确认你是登录状态，未登录则登录 `Docker Hub`。
 - 执行 `docker push hanjun806/nginx-stream:1.0`。
 - 成功之后到你的 `Docker Hub` 查看该 respository 的 tag 是否有 `1.0` 的版本存在了。

 - 运行

 ```
 
docker run --name my-nginx \
-v "/mnt/data/nginx/tcp.d:/opt/nginx/stream.conf.d" \
-p 3389:3389 -p 3309:3309 -d  hanjun806/nginx-stream:1.0

 ```