# Web 应用版本上线流程

1. 检查 *master* 是否和 *origin/master* 平齐；

2. 检查本地 Git 目录下是否干净，没有未 commit 或 stash 的改动；

3. 跳版本：
	如果这个版本引入了新功能，`npm version minor`；
	如果这个版本只是对已有功能做了修正和完善， `npm version patch`；

4. 推送版本改动到 Github：

		git push origin && git push --tags origin

5. 发布。

## Booking、Studio 发布

确认自己在 *master* 分支上，然后
	git push -f prod

## Gateway 发布

	docker build . -t gateway:<version>
	docker tag gateway:<version> docker.theweflex.com/gateway:<version>
	docker push docker.theweflex.com/gateway:<version>
	ssh root@api.getweflex.com 'spawn gateway <version>'

如果有数据库迁移脚本需要运行，

	scp -r migration root@api.getweflex.com:/opt/
	ssh root@api.getweflex.com
	(on remote): mongo $MONGO_URI /opt/migration/<script file>
