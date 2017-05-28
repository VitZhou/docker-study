#docker attach
## 描述
将本地标准输入，输出和错误流链接到正在运行的容器
## 使用
```shell
docker attach [OPTIONS] CONTAINER
```
## Options
| name | 默认值 | 描述 |
|--------|--------|--------|
|   --detach-keys    |        |     覆盖分离容器的key sequence(顺序)   |
|	--no-stdin	|	false	|	不链接标准输入	|
|	--sig-proxy	|	true	|	将所有接收的信号代理到该过程	|

## 父命令
| name | 描述 |
|--------|--------|--------|
|   docker    |   Docker CLI的基本命令  |

## 拓展描述
使用docker attach 容器ID或者名称连接你终端的标准输入,输出和错误(或三者的任何组合)到正在运行的容器.这可以让你查看到其正在运行的输出或以交互方式进行控制,就像命令直接在终端中运行一样.

你可以同时多次连接到相同的包含进程中,即使不同用户具有适当权限的话也可以.

要停止容器，请使用CTRL-c。 该key sequence将SIGKILL信号发送到容器。 如果--sig-proxy为true（默认值），CTRL-c会向容器发送SIGINT信号。 您可以从容器中分离，并使用CTRL-p CTRL-q key sequence将其运行。
> 在容器内作为PID 1运行的进程由Linux特别处理：它忽略任何具有默认操作的信号。 所以，这个过程不会在SIGINT或SIGTERM信号终止，除非它被编码为这样做。

禁止在连接到启用tty的容器（即：使用-t启动）时重定向docker连接命令的标准输入.

当客户端使用docker acctach连接到容器的stdio时,docker使用差不多1MB内存缓冲区来最大限度地提供应用程序的吞吐量.如果这个缓冲区被填满,API连接的速度将开始影响到进程输出的写入速度.这类似于其他应用,例如SSH.因此,不推荐在慢客户端连接上运行在前端产生大量输出的性能关键应用.相反,用户应该使用docker logs命令来访问日志.

## 覆盖分离序列
如果需要,你可以覆盖docker key sequnce以进行分离.如果docker默认序列与用于其它应用的key sequence冲突,则此功能非常有用.有两种方法来定义你自己的分离key sequence,作为整个容器覆盖或整个配置的配置属性.

覆盖单个容器的sequence,请使用docker attach命令使用--detach-keys="<sequence>"标志.<sequence>的格式是字母[a-Z]或ctrl-与以下任何一个组合：
- a-z: 单个小写字母字符
- @: at sign
- [: 左括号
- \\: 两个反斜杠
- _: 下划线
- ^: 插入符号

这些a，ctrl-a，X或ctrl - \\值都是有效key sequence的示例。 要为所有容器配置不同的配置默认密钥序列，请参阅[配置文件部分](https://docs.docker.com/edge/engine/reference/commandline/cli/#configuration-files)。

## 例子
连接到正在运行的容器上并分离
```shell
$ docker run -d --name topdemo ubuntu /usr/bin/top -b

$ docker attach topdemo

top - 02:05:52 up  3:05,  0 users,  load average: 0.01, 0.02, 0.05
Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
Cpu(s):  0.1%us,  0.2%sy,  0.0%ni, 99.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:    373572k total,   355560k used,    18012k free,    27872k buffers
Swap:   786428k total,        0k used,   786428k free,   221740k cached

PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
 1 root      20   0 17200 1116  912 R    0  0.3   0:00.03 top

 top - 02:05:55 up  3:05,  0 users,  load average: 0.01, 0.02, 0.05
 Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
 Cpu(s):  0.0%us,  0.2%sy,  0.0%ni, 99.8%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
 Mem:    373572k total,   355244k used,    18328k free,    27872k buffers
 Swap:   786428k total,        0k used,   786428k free,   221776k cached

   PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
       1 root      20   0 17208 1144  932 R    0  0.3   0:00.03 top


 top - 02:05:58 up  3:06,  0 users,  load average: 0.01, 0.02, 0.05
 Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
 Cpu(s):  0.2%us,  0.3%sy,  0.0%ni, 99.5%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
 Mem:    373572k total,   355780k used,    17792k free,    27880k buffers
 Swap:   786428k total,        0k used,   786428k free,   221776k cached

 PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
      1 root      20   0 17208 1144  932 R    0  0.3   0:00.03 top
^C$

$ echo $?
0
$ docker ps -a | grep topdemo

7998ac8581f9        ubuntu:14.04        "/usr/bin/top -b"   38 seconds ago      Exited (0) 21 seconds ago   
```
### 获取容器的退出代码
在第二个例子中，您可以看到bash进程返回的退出代码由docker attach命令返回给其调用者：
```shell
$ docker run --name test -d -it debian

275c44472aebd77c926d4527885bb09f2f6db21d878c75f0a1c212c03d3bcfab

$ docker attach test

root@f38c87f2a42d:/# exit 13

exit

$ echo $?

13

$ docker ps -a | grep test

275c44472aeb        debian:7            "/bin/bash"         26 seconds ago      Exited (13) 17 seconds ago                         test
```
