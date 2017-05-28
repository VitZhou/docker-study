# 使用多阶段构建
> 本主题仅适用于Docker CE Edge版本
> Docker 17.05中的多阶段构建是一个新功能，对于那些努力优化Dockerfiles同时保持易于阅读和维护的人而言，它们将会令人兴奋。

## 以前
构建镜像的最大挑战之一就是保持镜像尺寸的缩小.Dockerfile中的每个指令都向镜像添加一个图层,你需要记住在移动到下个图层之前清理你不需要的其他工件.要编写一个非常高效的dockerfile,以前你需要使用shell技巧和其他逻辑来保证尽可能小的图层,并确保每个图层都具有上一层所需要的工件,而没有其他的。

实际上非常常见:有一个Dockerfile用于开发（其中包含构建应用程序所需的一切）,而且仅用于生产的一个仅限于应用程序的应用程序，以及运行该应用程序所需的内容。 这被称为“builder模式”。 维护两个Dockerfiles并不理想。

以下是一个Dockerfile.build和Dockerfile的示例，它们遵循上面的builder模式：
Dockerfile.build:
```xml
FROM golang:1.7.3
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html
COPY app.go .
RUN go get -d -v golang.org/x/net/html \
  && CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .
```
请注意，此示例还使用Bash &&操作符人为地将两个RUN命令压缩在一起，以避免在镜像中创建一个附加层。 这是容易出错，难以维护的。 例如，很容易插入另一个命令，并忘记继续使用\字符。
Dockerfile:
```xml
FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY app .
CMD ["./app"]
```
build.sh:
```xml
#!/bin/sh
echo Building alexellis2/href-counter:build

docker build --build-arg https_proxy=$https_proxy --build-arg http_proxy=$http_proxy \
    -t alexellis2/href-counter:build . -f Dockerfile.build

docker create --name extract alexellis2/href-counter:build
docker cp extract:/go/src/github.com/alexellis/href-counter/app ./app
docker rm -f extract

echo Building alexellis2/href-counter:latest

docker build --no-cache -t alexellis2/href-counter:latest .
rm ./app
```
当您运行build.sh脚本时，需要构建第一个映像，从中创建一个容器，以便将工件复制出来，然后构建第二个映像。 两个图像占用您系统上的空间，您仍然有本地磁盘上的应用程序工件。

多阶段构建大大简化了这种情况！
