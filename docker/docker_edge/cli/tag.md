# Docker tag
> 这是Docker CE Edge版本的CLI参考文档。 其中一些选项可能不适用于Docker CE stable或Docker EE。

## 描述
镜像标签
## 使用方法
```shell
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

## 拓展描述
镜像名称一般由斜杠分割的名称组成,可以以注册表中的hostname作为前缀(可选).hostname必须符合标准DNS规则,不能包含下划线.如果存在hostname,则可以在其后面跟一个端口号(例如8080).如果不存在,则该命令默认使用位于registry-1.docker.io的Docker公共注册表.名称可以包含小写字母,数组,分隔符.分隔符定义为句号,一个或两个下划线或一个多个破折号.但是不能以分隔符开头或结尾.

标签名称必须是有效的ASCII码，并且可能包含小写和大写字母，数字，下划线，句点和破折号。 标签名称不能以句点或破折号开头，最多可包含128个字符。

您可以使用名称和标签将图像分组，然后将其上传到通过存储库共享镜像。

## 例子
### 标记由ID引用的镜像
要使用“version1.0”将ID为“0e5574283393”的本地映像标记到“fedora”存储库中：
```shell
$ docker tag 0e5574283393 fedora/httpd:version1.0
```
### 标记由Name引用的镜像
要使用“version1.0”将名为“httpd”的本地映像标记到“fedora”存储库中：
```shell
$ docker tag httpd fedora/httpd:version1.0
```
请注意，如果未指定标签名，因此将为现有本地版本httpd：latest创建别名。
### 引用标签或名称标记镜像
要使用“version1.0.test”将名为“httpd”的本地映像标记到“fedora”存储库中，并使用“test”标签：
```shell
$ docker tag httpd:test fedora/httpd:version1.0.test
```
### 标记一个私人存储库的镜像
要将图像推送到私人注册表，而不是中央Docker注册表，您必须使用注册表hostname和端口（如果需要）来标记它。
```shell
$ docker tag 0e5574283393 myregistryhost:5000/fedora/httpd:version1.0
```