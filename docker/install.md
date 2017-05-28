# 安装Docker
Docker有两个版本:社区版（CE）和企业版（EE）。
Docker社区版（CE）是开发人员和小团队的理想选择，适合开始使用Docker并尝试使用基于容器的应用程序。 Docker CE有两个更新通道，Stable和Edge：
- Stable: 每季度提供可靠更新
- Edge: 每个月为你提供新特性

Docker企业版（EE）专为企业开发和IT团队而设计，这些团队在生产中大规模生产，运送和运行关键业务应用。

| 功能 | CE |	EE 基础版 | EE标准版 | EE 高级版|
|--------|--------|--------|--------|--------|
|    容器引擎和内置的编排,网络化,安全    |    yes    |    yes    |    yes    |    yes    |
|    认证基础设施，插件和ISV容器    |    no    |    yes    |    yes    |    yes    |
|    镜像管理    |    no    |    no    |    yes    |    yes    |
|    app容器管理    |    no    |    no    |    yes    |    yes    |
|    镜像安全扫描    |    no    |    no    |    no    |    yes    |

## 支持平台
| 平台 | Docker CE x86_64 |	Docker CE ARM | Docker EE |
|--------|--------|--------|--------|--------|
|    Ubuntu    |    yes    |    yes    |    yes    |
|    Debian    |    yes    |    yes    |    no    |
|    Red Hat Enterprise Linux    |    no    |    no    |    yes    |
|    CentOS    |    yes    |    no    |    yes    |
|    Fedora    |    yes    |    no    |    no    |
|    Oracle Linux    |    no    |    no    |    yes    |
|    SUSE Linux Enterprise Server    |    no    |    no    |    yes    |
|    Microsoft Windows Server 2016    |    no    |    no    |    yes    |
|    Microsoft Windows 10    |    yes    |    no    |    no    |
|    macOS    |    yes    |    no    |    no    |
|    Microsoft Azure    |    yes    |    no    |    yes    |
|    Amazon Web Services    |    yes    |    no    |    yes    |

## Docker Cloud
您可以使用Docker Cloud自动配置和管理云实例。
- [Amazon Web Services设置指南](https://docs.docker.com/docker-cloud/infrastructure/link-aws/)
- [DigitalOcean设置指南](https://docs.docker.com/docker-cloud/infrastructure/link-do/)
- [Microsoft Azure设置指南](https://docs.docker.com/docker-cloud/infrastructure/link-azure/)
- [数据包安装指南](https://docs.docker.com/docker-cloud/infrastructure/link-packet/)
- [SoftLayer设置指南](https://docs.docker.com/docker-cloud/infrastructure/link-softlayer/)
- [使用Docker Cloud Agent自带主机](https://docs.docker.com/docker-cloud/infrastructure/byoh/)