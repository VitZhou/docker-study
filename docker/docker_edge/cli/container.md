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
|	--cpu-rt-period	|	0	| 限制CPU的real-time周期(单位是毫秒)	|
|	--cpu-shares, -c	|	0	|	CPU份额(权重)	|
|	--cpus	|	|	cpu数量	|
|	--cpuset-cpus	|	|	允许执行的CPU（0-3,0,1）	|
|	--cpuset-mems	|	|	允许执行的MEM（0-3,0,1）	|
|	--device	|	|	添加主机设备到容器中	|
|	--device-cgroup-rule	|	|	将规则添加到cgroup允许的设备列表	|
|	--device-read-bps	|		|	限制从设备读取数据的速率（每秒字节数）	|
|	--device-read-iops	|		|	限制从设备读取数据的速率（每秒IO）	|
|	--device-write-bps	|		|	限制写入设备的速率（每秒字节数）	|
|	--device-write-iops	|		|	限制写入设备的速率(每秒io数)	|
|	--disable-content-trust	|	true	|	跳过镜像验证	|
|	--dns	|		|	设置自定义DNS服务器	|
|	--dns-opt	|		|	设置DNS选项	|
|	--dns-option	|		|	设置DNS选项	|
|	--dns-search	|		|	设置自定义DNS搜索域	|
|	--entrypoint	|		|	覆盖镜像的默认ENTRYPOINT	|
|	--env, -e	|		|	设置环境变量	|
|	--env-file	|		|	读入环境变量文件	|
|	--expose	|		|	暴露端口或端口范围	|
|	--group-add	|		|	加入其他组	|
|	--health-cmd	|		|	健康检查命令	|
|	--health-interval	|	0s	|	运行检查间隔的时间（ns/us/ ms/s/m/h）（默认值0s）	|
|	--health-retries	|	0	|	连续失败需要报告非健康状态	|
|	--health-timeout	|	0s	|	允许一次健康检查运行的最大时间（ns/us/ ms/s/m/h）（默认值0s）	|
|	--help	|	false	|	打印使用方法	|
|	--hostname,-h	|		|	容器的hostname	|
|	--init	|	false	|	在容器内运行一个转发信号并收集进程的init	|
|	--init-path	|		|	docker-init二进制文件的路径	|
|	--interactive, -i	|	false	|	保持STDIN打开,即使不附加	|
|	--io-maxbandwidth	|	0	|	系统驱动的最大IO带宽限制（仅限Windows）	|
|	--io-maxiops	|	0	|	系统驱动的最大IOPS(每秒字节数)限制（仅限Windows）	|
|	--ip	|		|	ipv4地址(例如: 172.30.100.104)	|
|	--ip6	|		|	ipv6地址(例如: 2001:db8::33)	|
|	--ipc	|		|	使用的IPC 命名空间	|
|	--isolation	|		|	容器隔离技术	|
|	--kernel-memory	|	0	|	内核内存大小限制	|
|	--label, -l	|		|	设置容器的元数据	|
|	--label-file	|		|	读取标签的行分隔文件	|
|	--link	|		|	添加链接到另一个容器	|
|	--link-local-ip	|		|	容器IPv4/IPv6链接本地地址	|
|	--log-driver	|		|	容器的日志驱动	|
|	--log-opt	|		|	日志驱动选项	|
|	--mac-address	|		|	容器的mac地址(例如:92:d0:c6:0a:29:33)	|
|	--memory, -m	|	0	|	内存限制	|
|	--memory-reservation	|	0	|	内存软限制	|
|	--memory-swap	|	0	|	交换限制等于内存附加交换：'-1'以启用无限制的交换	|
|	--memory-swappiness	| -1	|	调整容器内存swappiness（0到100）	|
|	--mount	|		|	将文件系统安装附加到容器	|
|	--name	|		|	为容器指定名称	|
|	--net	|	default	|	将容器连接到网络	|
|	--net-alias	|		|	为容器添加网络别名	|
|	--network	|	default	|	将容器连接到网络	|
|	--network-alias	|		|	为容器添加网络别名	|
|	--no-helthcheck	|	false	|	禁用任何容器指定的HEALTHCHECK	|
|	--oom-kill-disable	|	false	|	禁用OOM Killer	|
|	--oom-score-adj	|	0	| 调整主机的OOM设置(-1000到1000)`|
| 	--pid	|	|	使用PID命名空间	|
|	--pids-limit	|	0	|	调整容器pids限制（设置为-1为无限制）	|
|	--privileged	|	false	|	为此容器提供扩展权限	|
|	--publish, -p	|		|	将容器的端口发布到主机	|
|	--publish-all, -P	|	false	|	将所有暴露的端口发布到随机端口	|
|	--read-only	|	false	|	将容器的根文件系统设置为只读	|
|	--restart	|	no	|	重新启动策略以在容器退出时应用	|
|	--rm	|	false	|	退出时自动删除容器	|
|	--runtime	|	| 运行时用于此容器	|
|	--security-opt	|		|	安全选项	|
|	--shm-size	|	0	|	/dev/shm的大小	|
|	--stop-signal	|	SIGTERM	|	指示停止容器	|
|	--stop-timeout	|	0	|	停止容器的超时时间(单位是秒)	|
|	--storage-opt	|		|	容器的存储驱动选项	|
|	--sysctl	|	map[]	|	Sysctl选项	|
|	--tmpfs	|		|	装载一个tmpfs目录	|
|	--tty, -t	|	false	|	分配伪TTY	|
|	--ulimit	|		|	Ulimit选项	|
|	--user, -u	|		|	用户名或UID(格式：< name/uid >[：< group/gid >])	|
|	--userns	|		|	要使用的用户名空间	|
|	--uts	|		|	使用UTS命名空间	|
|	--volume, -v	|	|	绑定一个挂载卷	|
|	--volume-driver	|		|	容器的挂载卷驱动	|
|	--volumes-from	|		|	从指定的容器装载挂载卷	|
|	--workdir, -w	|		|	容器内的工作目录	|
