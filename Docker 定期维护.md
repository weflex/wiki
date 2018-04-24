# Docker 定期维护

由于 Docker 是基于镜像来管理服务和应用版本的，所以我们需要定期清除不需要的镜像和容器。

1. 清除已经退出的容器

		docker rm `docker ps -q --filter status=exited`

这个命令先用 `docker ps` 列出容器列表，只显示已经退出的容器 (`--filter status=exited`) 的 *id* (`-q`)，然后把列表传给 `docker rm` 命令来删除这些容器。

2. 清除过期的镜像

		docker rmi `docker images -aq`

这个命令列出所有镜像的 *id* (`docker images -aq`)，然后尝试删除这些镜像。正在被使用的镜像不会被删除。
