#Docker ckeckpoint
> 这是Docker CE Edge版本的CLI参考。 其中一些选项可能不适用于Docker CE stable或Docker EE。

## 使用
```shell
docker checkpoint COMMAND
```

## 子命令
| 命令 | 描述 |
|--------|--------|
|    docker checkpoint create    |    从正在运行的容器创建一个checkpoint    |
|	docker checkpoint ls	| 容器的checkpoint列表	|
|	docker checkpooint rm	| 删除checkpoint	|

### docker checkpoint create
#### 使用
```shell
docker checkpoint create [OPTIONS] CONTAINER CHECKPOINT
```
#### Options
| 名称 | 默认值 |	描述	|
|--------|--------|--------|
|   --checkpoint-dir     |        |		使用自定义checkpoint存储目录	|
| 	--leave-running		|	false	|	在checkpoint后运行容器	|

### docker ckeckpoint ls
#### 用法
```shell
docker checkpoint ls [OPTIONS] CONTAINER
```
#### options
| 名称 | 默认值 |	描述	|
|--------|--------|--------|
|   --checkpoint-dir     |        |		使用自定义checkpoint存储目录	|

### docker checkpoint rm
#### 使用
```shell
docker checkpoint rm [OPTIONS] CONTAINER CHECKPOINT
```
#### option
| 名称 | 默认值 |	描述	|
|--------|--------|--------|
|   --checkpoint-dir     |        |		使用自定义checkpoint存储目录	|