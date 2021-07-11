``` sh
docker build . -t <your username>/node-web-app
docker run --name <容器名> -p 49160:8080 -d <your username>/node-web-app //运行镜像
-d 守护容器，就是后台运行，退出命令窗口容器也不会停止 -it 交互式容器 退出命令窗口容器就停止运行了
-p 宿主机端口和容器端口映射
docker exec -it <container id> bash // 进入容器
docker start <container id>// 运行容器
docker kill <container id> // 杀死容器
docker stop <container id> // 停止容器
docker rm <container id>// 删除容器
docker restart <container id>// 重启容器
docker system prune 
docker ps 查看正在运行的容器
docker ps -a 查看所有容器
docker logs <container id> // 查看容器日志
docker images //镜像列表
```

