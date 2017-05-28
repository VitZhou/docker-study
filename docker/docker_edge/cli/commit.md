# Docker commit
> 这是Docker CE Edge版本的CLI参考文档。 其中一些选项可能不适用于Docker CE stable或Docker EE。

## 描述
更改容器创建新镜像

## 使用
```shell
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

## Options
| 名称,缩写 | 默认值 |	描述	|
|--------|--------|--------|
|    --author, -a    |        |		作者   |
| 	--change, -c	|	|	将Dockerfile指令应用于创建的镜像 |
|	--message, -m	|	| 提交信息	|
|	--pause,-p	| true	|	提交期间暂停容器	|
## 拓展描述
在提交容器的文件或者设置新镜像很有用.这允许您通过运行交互式shell来调试容器，或将工作数据集导出到另一个服务器.一般来说,最好使用Dockerfiles以文件和可维护的方式管理镜像.详情情况请查看[镜像的名称和标签](tag.md)

提交操作不会包含容纳在容器内的卷中的任何数据。

默认情况下，正在提交的容器及其进程将在镜像提交时暂停。 这减少了在创建提交过程中遇到数据损坏的可能性。 如果不需要此行为，请将--pause选项设置为false。

--change选项将对创建的映像应用Dockerfile指令。 支持Dockerfile指令：
CMD|ENTRYPOINT|ENV|EXPOSE|LABEL|ONBUILD|USER|VOLUME|WORKDIR

## 例子
### 提交容器
```shell
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker images

REPOSITORY                        TAG                 ID                  CREATED             SIZE
svendowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB
```
### 提交具有新配置的容器
```shell
$ docker ps

CONTAINER ID       IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker inspect -f "{{ .Config.Env }}" c3f279d17e0a

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin]

$ docker commit --change "ENV DEBUG true" c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker inspect -f "{{ .Config.Env }}" f5283438590d

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin DEBUG=true]
```
### 提交具有新CMD和EXPOSE指令的容器
```shell
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit --change='CMD ["apachectl", "-DFOREGROUND"]' -c "EXPOSE 80" c3f279d17e0a  svendowideit/testimage:version4

f5283438590d

$ docker run -d svendowideit/testimage:version4

89373736e2e7f00bc149bd783073ac43d0507da250e999f3f1036e0db60817c0

$ docker ps

CONTAINER ID        IMAGE               COMMAND                 CREATED             STATUS              PORTS              NAMES
89373736e2e7        testimage:version4  "apachectl -DFOREGROU"  3 seconds ago       Up 2 seconds        80/tcp             distracted_fermat
c3f279d17e0a        ubuntu:12.04        /bin/bash               7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash               7 days ago          Up 25 hours                            focused_hamilton
```