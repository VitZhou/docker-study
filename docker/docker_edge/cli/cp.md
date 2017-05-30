# Docker cp
> 这是Docker CE Edge版本的CLI参考文档。 其中一些选项可能不适用于Docker CE stable或Docker EE。

## 描述
在容器与本地文件系统之间复制文件/目录

## 使用方法
```shell
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH
```

## options
| 名称,缩写 | 默认值 |		功能描述	|
|--------|--------|--------|
|    --follow-link,-L    |    false    |     始终遵循SRC_PATH中的符号链接   |

## 拓展描述
docker cp命令会将SRC_PATH的内容复制到DEST_PATH.你可以从容器的文件系统复制到本地机器,也可以从本地文件系统复制到容器.如果被指定为SRC_PATH或DEST_PATH,你还可以将指定tar压缩包从STDIN或者STDOUT流失传输.CONTAINER可以是运行或者停止的容器.SRC_PATH或者DEST_PATH可以是文件或目录.

docker cp命令假定路径是相对于容器的/（root）目录.这意味着提供/是可选的;该命令会将compassionate_darwin：/tmp/foo/myfile.txt和compassionate_darwin：tmp / foo / myfile.txt当做相同的路径.本地机器路径可以是绝对或者相对路径.该命令将本地机器的相对路径解释为相对于运行docker cp的当前工作目录.

cp命令的行为类似于Unix cp -a命令，如果可能，目录将以递归方式复制并保留权限。 所有权设置为目的地的用户和组。 例如，使用root用户的UID：GID创建用于复制到容器的文件。 使用调用docker cp命令的用户的UID：GID创建复制到本地计算机的文件。 如果指定-L选项，则docker cp遵循SRC_PATH中的任何符号链接。 docker cp不会为DEST_PATH创建父目录（如果它们不存在）。

假设路径分隔符为/,第一个参数SRC_PATH和第二个参数DEST_PATH，行为如下：
- SRC_PATH指定一个文件
	- DEST_PATH不存在
		- 该文件将保存到DEST_PATH创建的文件中
    - DEST_PATH不存在并以/结尾
    	- 错误条件：目标目录必须存在。
    - DEST_PATH存在,并且是一个文件
		- 目的地文件被源文件的内容覆盖
	- DEST_PATH存在,并且是一个目录
		- 该文件将使用SRC_PATH中的basename复制到此目录中
- SRC_PATH指定一个目录
	- DEST_PATH不存在
		- DEST_PATH被创建为目录，源目录的内容被复制到该目录中
    - DEST_PATH不存在并以/结尾
    	- 错误条件：无法将目录复制到文件
    - DEST_PATH存在,并且是一个文件
		- 目的地文件被源文件的内容覆盖
	- DEST_PATH存在,并且是一个目录
		- SRC_PATH不以/结尾。
			- 源目录被复制到该目录中
        - SRC_PATH以/结尾。
			- 源目录的内容被复制到该目录中

该命令需要根据上述SRC_PATH和DEST_PATH的存在规则。如果SRC_PATH是本地的，并且是符号链接，则默认情况下复制符号链接而不是目标。 要复制链接目标而不是链接，请指定-L选项。

冒号(:)用作容器与其路径之间的分隔符。 您还可以使用：在本地计算机上指定SRC_PATH或DEST_PATH的路径，例如file：name.txt。 如果在本地机器路径中使用：必须使用相对路径或绝对路径显式，例如：
```shell
`/path/to/file:name.txt` or `./file:name.txt`
```

无法将某些系统文件（例如/ proc，/ sys，/ dev，tmpfs）下的资源和用户在容器中创建的挂载复制。 但是，您仍然可以通过在docker exec中手动运行tar来复制此类文件。 以下两个示例都以不同的方式执行相同的操作（假设SRC_PATH和DEST_PATH是目录）：
```shell
$ docker exec foo tar Ccf $(dirname SRC_PATH) - $(basename SRC_PATH) | tar Cxf DEST_PATH -
```
```shell
$ tar Ccf $(dirname SRC_PATH) - $(basename SRC_PATH) | docker exec -i foo tar Cxf DEST_PATH -
```
使用-作为SRC_PATH将STDIN的内容作为tar存档流。 该命令将tar的内容提取到容器文件系统中的DEST_PATH。 在这种情况下，DEST_PATH必须指定一个目录。 使用-作为DEST_PATH将资源的内容作为tar存档流传输到STDOUT。