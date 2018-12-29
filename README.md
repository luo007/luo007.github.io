## 欢迎来到luo007的GitHub

这是我的个人笔记记录的地方
### Docker
#### Docker自启动
这是启动脚本
```Docker
# docker.service
#!/bin/sh
sudo systemctl enable docker
sudo systemctl start docker
```
将脚本放置在/etc/init.d/目录下，修改成root执行权限，然后输入
```
sysv-rc-conf
```
#### Docker容器自启动
我们设置了docker自启动后，docker可以管理各种容器了，对于容器我们也可以设置重启的策略。
在容器退出或断电开机后，docker可以通过在容器创建时的--restart参数来指定重启策略；
```
# 多个参数值选择
no  不自动重启容器. (默认值)
on-failure  容器发生error而退出(容器退出状态不为0)重启容器,可以指定重启的最大次数，如：on-failure:10
unless-stopped  在容器已经stop掉或Docker stoped/restarted的时候才重启容器
always  在容器已经stop掉或Docker stoped/restarted的时候才重启容器，手动stop的不算
```
```
# 设置启动策略
docker run --restart always --name mynginx -d nginx
```
如果容器已经被创建，我们想要修改容器的重启策略
```docker update --restart no mynginx```
##### 注意：
容器只有在成功启动后restart policy才能生效。这里的"成功启动"是指容器处于up至少10秒且已经处于docker监管。这是避免没有成功启动的容器陷入restart的死循环。
如果手动stop一个容器，容器设置的restart policy将会被忽略，除非Docker守护进程重启或者容器手动重启；这是避免了如果重启策略设置了always，如果不忽略policy那么容器无法手动停止。
