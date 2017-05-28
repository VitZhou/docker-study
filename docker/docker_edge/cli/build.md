# DOcker build
将Dockerfile构建出镜像
## 使用
```shell
docker build [OPTIONS] PATH | URL | -
```

## Options
| Name, shorthand| 	Default |	Description  |
|--------|--------|--------|
|   --add-host     |        |    添加自定义的host-to-ip映射(host：ip)    |
|   --build-arg     |        |    设置构建时变量    |
|   --cache-from     |        |    要考虑的镜像作为缓存源    |
|   --cgroup-parent     |        |    容器的可选父cgroup    |
|   --compress     |    false    |     使用gzip压缩构建环境   |
|   --cpu-period     |    0    |     限制CPU CFS（完全公平的调度程序）周期   |
|   --cpu-quota     |    0   |     限制CPU CFS（完全公平的调度程序）配额   |
|   --cpu-shares, -c     |    0    |    CPU份额（相对重量）    |
|   --cpuset-cpus     |        |    允许执行的CPU（0-3,0,1）    |
|   --disable-content-trust     |    true    |    跳过镜像验证    |
|   --file, -f     |    PATH/Dockerfile    |    Dockerfile的名称    |
|   --force-rm     |    false    |    总是删除intermediate容器    |
|   --isolation     |        |    容器隔离技术    |
|   --label     |        |    设置镜像的元数据    |
|   --memory, -m    |    0    |    内存限制    |
|   --memory-swap     |    0    |    交换限制等于内存加交换：'-1'可以启用无限制的交换    |
|   --network     |    default    |    在构建期间设置RUN指令的联网模式    |
|   --no-cache     |    false    |    构建镜像时不要使用缓存    |
|   --pull     |    false    |    始终尝试拉一个较新版本的图像    |
|   --quiet, -q     |    false    |    抑制构建输出并在构建成功后打印镜像id    |
|   --rm     |    true    |    成功构建后移除intermediate(中间)容器    |
|   --security-opt     |        |    安全选项    |
|   --shm-size     |    0    |    /dev/shm的大小    |
|   --squash     |    false    |    将新建的layer压缩成一个新的layer(图层)    |
|   --tag, -t     |        |    名称和可选的“name：tag”格式的标签    |
|   --target     |        |    设置目标构建阶段来构建。    |
|   --ulimit     |        |    Ulimit选项    |

## 拓展描述
从Dockerfile和“context”构建Docker镜像.构建的context是位于指定PATH或URL中的文件.构建过程可以引用context中的任何文件,例如，您的构建可以使用ADD指令来引用上下文中的文件。
URL参数可以指三种资源：Git存储库，预打包的tarball上下文和纯文本文件。
### git仓库
当URL参数指向git仓库的位置时,仓库充当构建上下文.系统使用git clone --depth 1 --recursive命令递归地克隆资源仓库及其子模块.此命令在本地主机上的临时目录中运行.命令成功后,该目录将作为上下文发送到Docker daemon.本地克隆使你能够使用本地用户凭据,VPN等访问私有存储库.

Git URL在其片段中接受上下文配置，用冒号分隔。第一部分表示Git将检出的引用，这可以是分支，标签或提交SHA。 第二部分表示存储库中将用作构建上下文的子目录。

例如，运行此命令在分支容器中使用名为docker的目录：
```shell
$ docker build https://github.com/docker/rootfs.git#container:docker
```
下表表示其构建上下文的所有有效后缀：
| Build语法后缀 | 提交使用 | build上下文使用 |
|--------|--------|--------|
|    myrepo.git    |    refs/heads/master    |     /   |
|    myrepo.git#mytag    |    refs/tags/mytag    |    /    |
|    myrepo.git#mybranch    |    refs/heads/mybranch    |    /    |
|    myrepo.git#abcdef    |    sha1 = abcdef    |    /    |
|    myrepo.git#:myfolder    |    refs/heads/master    |    /myfolder   |
|    myrepo.git#master:myfolder    |    refs/heads/master    |    /myfolder    |
|    myrepo.git#mytag:myfolder    |    refs/tags/mytag    |    /myfolder    |
|    myrepo.git#mybranch:myfolder    |    refs/heads/mybranch    |    /myfolder    |
|    myrepo.git#abcdef:myfolder    |    sha1 = abcdef    |    /myfolder    |

### Tarball(压缩)上下文
如果您将URL传递远程压缩包，则URL本身将发送到daemon：
```shell
$ docker build http://server/context.tar.gz
```
下载操作将在运行Docker daemon的宿主机上执行,这不一定是正在发出构建命令的主机.Docker deamon将获取context.tar.gz并将其用作build context.压缩包上下文必须是符合标准tar unix格式的tar存档,并且可以使用'xz','bzip2','gzip'或'identity'(无压缩)格式中的任何一个进行压缩.

### Text文件
可以在URL或通过STDIN管道中传递的单个Dockerfile,而不是指定上下文.从STDIN管道传递Dockfile:
```shell
$ docker build - < Dockerfile
```
在Windows上使用Powershell，您可以运行：
```shell
Get-Content Dockerfile | docker build -
```
如果您使用STDIN或指定一个指向纯文本文件的URL，系统会将内容放入一个名为Dockerfile的文件中，并忽略任何-f，--file选项。 在这种情况下，没有上下文。

默认情况下,docker build命令将在构建上下文的根目录中查找Dockerfile.-f,--file,选项可以指定替代文件的路径.在同一组文件用于多个构建的情况下,这是有用的.该路径必须是构建上下文中的文件.如果指定了相对路径,则将其解释为相对于上下文的根.

在大多数情况下,最好将每个Dockerfile放在一个空目录中.然后,仅添加构建Dockerfile所需要的文件.要增加构建的性能,你可以通过将.dockerignore文件添加到该目录来排除文件和目录。

如果Docker客户端丢失与守护程序的连接，则构建将被取消。 如果您使用CTRL-c中断Docker客户端或者由于任何原因Docker客户端被杀死，则会发生这种情况。 如果构建启动了在构建被取消时仍然运行的pull，则pull也被取消。

## 例子
### 在path上build
```shell
$ docker build .

Uploading context 10240 bytes
Step 1/3 : FROM busybox
Pulling repository busybox
 ---> e9aa60c60128MB/2.284 MB (100%) endpoint: https://cdn-registry-1.docker.io/v1/
Step 2/3 : RUN ls -lh /
 ---> Running in 9c9e81692ae9
total 24
drwxr-xr-x    2 root     root        4.0K Mar 12  2013 bin
drwxr-xr-x    5 root     root        4.0K Oct 19 00:19 dev
drwxr-xr-x    2 root     root        4.0K Oct 19 00:19 etc
drwxr-xr-x    2 root     root        4.0K Nov 15 23:34 lib
lrwxrwxrwx    1 root     root           3 Mar 12  2013 lib64 -> lib
dr-xr-xr-x  116 root     root           0 Nov 15 23:34 proc
lrwxrwxrwx    1 root     root           3 Mar 12  2013 sbin -> bin
dr-xr-xr-x   13 root     root           0 Nov 15 23:34 sys
drwxr-xr-x    2 root     root        4.0K Mar 12  2013 tmp
drwxr-xr-x    2 root     root        4.0K Nov 15 23:34 usr
 ---> b35f4035db3f
Step 3/3 : CMD echo Hello world
 ---> Running in 02071fceb21b
 ---> f52f38b7823e
Successfully built f52f38b7823e
Removing intermediate container 9c9e81692ae9
Removing intermediate container 02071fceb21b
```
该例子指定path为.,因此本地目录中的所有文件都已获取并发送到Docker daemon.PATH指定在Docker daemon上查找构建“context”的文件的位置.请记住，守护进程可能在远程计算机上运行，并且在客户端（您正在运行docker构建）时不会解析Dockerfile.这意味着PATH中的所有文件都将被发送，而不仅仅是在Dockerfile中列出的文件。

将上下文从本地机器传输到Docker deamon时，当您看到“Sending build context”消息时，docker客户端意味着什么。

如果希望在构建完成后保留intermediate容器，则必须使用--rm = false。 这不影响构建缓存。

### Build with URL
```shell
$ docker build github.com/creack/docker-firefox
```
这将克隆GitHub存储库并使用克隆的存储库作为上下文。 存储库根目录下的Dockerfile用作Dockerfile。 您可以使用git：//或git @ scheme指定任意的Git仓库。
```shell
$ docker build -f ctx/Dockerfile http://server/ctx.tar.gz

Downloading context: http://server/ctx.tar.gz [===================>]    240 B/240 B
Step 1/3 : FROM busybox
 ---> 8c2e06607696
Step 2/3 : ADD ctx/container.cfg /
 ---> e7829950cee3
Removing intermediate container b35224abf821
Step 3/3 : CMD /bin/ls
 ---> Running in fbc63d321d73
 ---> 3286931702ad
Removing intermediate container fbc63d321d73
Successfully built 377c409b35e4
```
它将URL http：//server/ctx.tar.gz发送到Docker daemon，Docker daemon下载并提取引用的tarball。 -f ctx/Dockerfile参数指定用于构建镜像的Dockerfile的ctx.tar.gz内的路径。 引用本地路径的Dockerfile中的任何ADD命令必须相对于ctx.tar.gz内的内容的根目录。 在上面的示例中，tarball包含一个目录ctx/，因此ADD ctx/container.cfg/操作正常工作。

### Build with -
```shell
$ docker build - < Dockerfile
```
这将构建一个从STDIN读取的压缩上下文的镜像。 支持的格式有：bzip2，gzip和xz。

### 使用a.dockerignore文件
```shell
$ docker build .

Uploading context 18.829 MB
Uploading context
Step 1/2 : FROM busybox
 ---> 769b9341d937
Step 2/2 : CMD echo Hello world
 ---> Using cache
 ---> 99cc1ad10469
Successfully built 99cc1ad10469
$ echo ".git" > .dockerignore
$ docker build .
Uploading context  6.76 MB
Uploading context
Step 1/2 : FROM busybox
 ---> 769b9341d937
Step 2/2 : CMD echo Hello world
 ---> Using cache
 ---> 99cc1ad10469
Successfully built 99cc1ad10469
```
此示例显示使用.dockerignore文件从上下文中排除.git目录。其影响可以在上传的上下文的大小变化中看到.请参考创建[a.dockerignore文件](https://docs.docker.com/edge/engine/reference/builder/#dockerignore-file)

### 标记一个镜像(-t)
```shell
$ docker build -t vieux/apache:2.0 .
```
这将像前面的例子一样构建，但它会标记生成的镜像。 存储库名称将是vieux/apache，标签将为2.0。 详细了解[有效标签](tag.md)。

你可以对镜像使用标记多个标签.例如，您可以将最新的标签应用于新建的镜像，并添加引用特定版本的另一个标签. 例如，要将镜像标记为whenry/fedora-jboss：latest和whenry/fedora-jboss：v2.1，请使用以下命令：
```shell
$ docker build -t whenry/fedora-jboss:latest -t whenry/fedora-jboss:v2.1 .
```

### 指定一个Dockerfile（-f）
```shell
$ docker build -f Dockerfile.debug .
```
这将使用一个名为Dockerfile.debug的文件作为build指令而不是Dockerfile。
```shell
$ docker build -f dockerfiles/Dockerfile.debug -t myapp_debug .
$ docker build -f dockerfiles/Dockerfile.prod  -t myapp_prod .
```
上述命令将构建当前构建上下文（由.指定）两次，一次使用Dockerfile的调试版本,一次使用生产版本。
```shell
$ cd /home/me/myapp/some/dir/really/deep
$ docker build -f /home/me/myapp/dockerfiles/debug /home/me/myapp
$ docker build -f ../../../../dockerfiles/debug /home/me/myapp
```
这两个docker构建命令执行完全相同的操作。 他们都使用debug文件的内容，而不是寻找Dockerfile，并将使用/home/me/myapp作为构建上下文的根。 请注意，debug位于构建上下文的目录结构中，无论在命令行中如何引用它。
> 如果上传的上下文中不存在文件或目录，docker构建将返回没有这样的文件或目录错误.如果没有上下文，或者如果您指定了主机系统上其他位置的文件，则可能会发生这种情况。 出于安全原因，上下文限于当前目录（及其子节点），并确保远程Docker主机上可重复构建。 这也是ADD ../file无法工作的原因。

### 使用自定义父cgroup（-cgroup-parent）
当使用--cgroup-parent选项运行docker构建时，构建中使用的容器将使用相应的docker flag运行。

### 在容器中设置ulimits（-ulimit）
对docker构建使用--ulimit选项将导致构建容器的每个步骤都使用--ulimit标志值。

### 设置构建时变量（-build-arg）
您可以在Dockerfile中使用ENV指令定义变量值。 这些值仍然存在于构建的映像中。 但是，经常不是你想要的。 用户希望根据构建镜像的主机指定变量。

一个很好的例子是http_proxy或用于拉取中间文件的源代码。 ARG指令使Dockerfile作者定义了用户可以使用--build-arg标志在构建时设置值：
```shell
$ docker build --build-arg HTTP_PROXY=http://10.20.30.2:1234 .
```
该标志允许您在Dockerfile的RUN指令中传递与常规环境变量一样的构建时变量。 此外，这些值不会像ENV值那样持续在中间或最终镜像中。

在构建过程中，当Dockerfile中的ARG行被回显时，使用此标志不会更改您看到的输出。

### 可选安全选项（-security-opt）
此标志仅在Windows上运行的守护程序支持，并且仅支持credentialspec选项。 credentialspec必须是格式file://spec.txt或registry:// keyname。

### 指定容器隔离技术（–isolation）
在Windows上运行Docker容器的情况下，此选项很有用。 --isolation = <value>选项设置容器的隔离技术。 在Linux上，唯一支持的是使用Linux namespaces的默认选项。 在Microsoft Windows上，您可以指定以下值：

| Value | 描述 |
|--------|--------|
|    default    |    使用Docker守护程序指定的值--exec-opt。 如果守护程序未指定隔离技术，Microsoft Windows将使用进程作为其默认值。    |
| process |	仅限namespace隔离。	|
| hyperv	|	Hyper-V虚拟机管理程序基于分区的隔离。	|

### 将条目添加到容器host文件（-add-host）
您可以使用一个或多个--add-host标志将其他host添加到容器的/etc/hosts文件中。 此示例为添加静态地址(名为docker的host)：
```shell
$ docker build --add-host=docker:10.180.0.1 .
```

### 压缩镜像的图层（-squash）仅实验
#### 概述
一旦构建了图像，新的图层压缩成单一图层的新镜像。 压缩不会破坏任何现有的镜像，而是创建一个包含压缩图层内容的新镜像。这有效地使它看起来像所有的Dockerfile命令都是用单个图层创建的。 使用此方法保留构建缓存。
> 使用此选项意味着新的镜像将无法替补方体育其他镜像的图层共享，并且可能会占用更多的空间。

> 使用此选项，由于存储图像的两个副本，您可能会看到显着更多的空间，一个用于构建具有所有缓存层的缓存，一个用于压缩版本。

#### 必备条件
该页面上的示例在Docker 1.13中使用实验模式。

在启动Docker守护程序时使用--experimental标志，或者在daemon.json配置文件中设置experimental：true可以启用实验模式。

默认情况下，禁用实验模式。 要查看当前配置，请使用docker version命令。
```shell
Server:
 Version:      1.13.1
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   092cba3
 Built:        Wed Feb  8 06:35:24 2017
 OS/Arch:      linux/amd64
 Experimental: false

 [...]
```
要启用实验模式，用户需要在启用实验标志的情况下重新启动docker守护程序。
#### 启用实验室实验
实际功能现已包含在1.13.0版本的标准Docker二进制文件中。 为了启用实验功能，您需要使用 -experimental标志启动Docker守护程序。 您也可以通过/etc/docker/daemon.json启用守护进程标志。 例如
```json
{
    "experimental": true
}
```
然后确保实验标志已启用：
```shell
$ docker version -f '{{.Server.Experimental}}'
true
```

### 用--SQUASH参数构建镜像
以下是使用--squash参数的docker构建示例
```shell
FROM busybox
RUN echo hello > /hello
RUN echo world >> /hello
RUN touch remove_me /remove_me
ENV HELLO world
RUN rm /remove_me
```
一个名为test的镜像是用--squash参数构建的。
```shell
$ docker build --squash -t test .

[...]
```
如果一切正确，列表将如下所示：
```shell
$ docker history test

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
4e10cb5b4cac        3 seconds ago                                                       12 B                merge sha256:88a7b0112a41826885df0e7072698006ee8f621c6ab99fca7fe9151d7b599702 to sha256:47bcc53f74dc94b1920f0b34f6036096526296767650f223433fe65c35f149eb
<missing>           5 minutes ago       /bin/sh -c rm /remove_me                        0 B
<missing>           5 minutes ago       /bin/sh -c #(nop) ENV HELLO=world               0 B
<missing>           5 minutes ago       /bin/sh -c touch remove_me /remove_me           0 B
<missing>           5 minutes ago       /bin/sh -c echo world >> /hello                 0 B
<missing>           6 minutes ago       /bin/sh -c echo hello > /hello                  0 B
<missing>           7 weeks ago         /bin/sh -c #(nop) CMD ["sh"]                    0 B
<missing>           7 weeks ago         /bin/sh -c #(nop) ADD file:47ca6e777c36a4cfff   1.113 MB
```
我们可以发现所有镜像的名称都是<missing>，并且有一个新层与COMMENT合并。

测试镜像，检查/remove_me不在，请确保hellonworld在/hello，确保HELLO envvar的值是world。

