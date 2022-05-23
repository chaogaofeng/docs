# 快速入门

## 快速安装

### 1.1 基础环境
我们为您提供了能够快速部署GNC测试网络的脚本，在使用前需要您准备如下环境:
docker，推荐版本18.03+ [点击下载安装 docker](https://docs.docker.com/get-docker/)
docker-compose，推荐版本1.26.0+ [点击下载安装 docker-compose](https://github.com/docker/compose/releases)

### 1.2 网络启动
环境准备好之后，可以通过执行脚本快速部署测试网络:

```shell
git clone https://gitlab.hang-xin.cn/blockchain/chain.git
cd chain
make localnet-start
```

使用脚本也可以快速销毁网络：

```shell
make localnet-start stop
```

## 源码安装

### 1.1 环境准备
使用 golang 进行开发，当您使用源码进行编译和安装时，首先需要准备源码以及编译的环境：

- 系统环境，推荐使用Linux或MacOS操作系统
- golang编译器，推荐版本1.13+ [点击下载安装golang](https://studygolang.com/dl)
- git源码下载工具 [点击下载安装git](https://git-scm.com/download)
- make编译工具

### 1.2 源码编译

```shell
git clone https://gitlab.hang-xin.cn/blockchain/chain.git
cd chain
make build
```

编译产出为 output 文件夹，内容为：


## 命令行工具