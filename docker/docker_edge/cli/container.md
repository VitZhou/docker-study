# Docker container
> 这是Docker CE Edge版本的CLI参考文档。 其中一些选项可能不适用于Docker CE stable或Docker EE。

## 描述
容器管理

## 使用方法
```shell
docker container COMMAND
```

## 子命令
### docker container attach
将本地标准输入，输出和错误流附加到正在运行的容器
#### 使用方法
```shell
docker container attach [OPTIONS] CONTAINER
```
#### Options
| 名称 | 默认值 |描述	|
|--------|--------|--------|
|    --detach-keys    |        |    覆盖分离容器的key sequence    |
|	--no-stdin	|	false	|	不要附加STDIN	|
|	--sig-proxy	|	true	|	将所有接收的信号代理到该进程	|

### docker container commit
#### 描述
更改容器创建新镜像
#### 使用方法
```shell
docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
#### Options
| 名称,缩写 | 默认值 |	描述	|
|--------|--------|--------|
|    --author, -a    |        |		作者   |
| 	--change, -c	|	|	将Dockerfile指令应用于创建的镜像 |
|	--message, -m	|	| 提交信息	|
|	--pause,-p	| true	|	提交期间暂停容器	|

### docker container cp
#### 描述
在容器和本地文件系统之间复制文件/目录
#### 使用方法
```shell
docker container cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```
#### options
| 名称,缩写 | 默认值 |	描述	|
|--------|--------|--------|
|    --follow-link, -L    |    false    |	始终遵循SRC_PATH中的符号链接   |

### docker container create
#### 描述
创建一个新的容器
#### 使用方式
```shell
docker container create [OPTIONS] IMAGE [COMMAND] [ARG...]
```
#### Options
| 名称,缩写 | 默认值 |	描述	|
|--------|--------|--------|
|    --add-host    |        |	添加自定义的host-to-ip映射(host:ip)   |
|	--attach, -a	|	|	附加到STDIN，STDOUT或STDERR	|
|	--blkio-weight	|	0	|	阻塞IO（相对权重），介于10和1000之间，0为禁用（默认为0）|
|	--blkio-weight-device	|	|	阻塞IO权重（相对设备权重）	|
|	--cap-add	|	|	添加linux功能	|
|	--cap-drop	|	|	删除linux功能	|
|	--cgroup-parent	|	|	容器的可选父cgroup	|
|	--cidfile	|	|	将容器ID写入文件	|
|	--cpu-count	|	0	|	设置CPU总数（仅限Windows）	|
|	--cpu-percent	|	0	|	设置cpu百分比(仅限windows)	|
|	--cpu-period	|	0	|	限制CPU CFS（完全公平的调度程序）周期	|
|	--cpu-quota	|	0	|	限制CPU CFS（完全公平的调度程序）配额	|
|