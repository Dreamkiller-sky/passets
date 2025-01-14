## ![](docs/images/logo.png)

[![Python 3.x](https://img.shields.io/badge/python-3.x-yellow.svg)](https://www.python.org/) [![Ruby 2.5](https://img.shields.io/badge/ruby-2.5-red.svg)](https://www.ruby-lang.org/) [![Nodejs 8.x](https://img.shields.io/badge/nodejs-8.x-green.svg)](https://www.ruby-lang.org/) [![Java 1.8](https://img.shields.io/badge/java-1.8-red.svg)](https://www.java.com/) [![Platform linux&docker](https://img.shields.io/badge/platform-linux&docker-9cf.svg)](https://www.java.com/) [![License](https://img.shields.io/badge/license-GPLv3-red.svg)](https://raw.githubusercontent.com/knownsec/Pocsuite/master/docs/COPYING)


## 概述

passets 是一套由 [DSO 安全实验室（DSO Security Lab）](http://www.dsolab.org)开源的被动资产识别框架，用于基于被动流量的网络资产发现。整体框架包括四大组件：

**[passets-sensor](https://github.com/DSO-Lab/passets-sensor)**
> 网络流量采集模块：提供流量采集及基本的流量过滤和处理。

**[passets-filter](https://github.com/DSO-Lab/passets-filter)**

> 数据清洗模块：根据数据清洗插件对数据进行深度清洗、标记和重组。

**[passets-api](https://github.com/DSO-Lab/passets-api)**

> 外部接口模块：负责根据业务需求提供所需的数据。


## 特性
* 从采集到数据清洗全部支持分布式架构
* 数据清洗插件可自行定制
* 支持多种硬件环境（x86、ARM）
* 容器化部署，操作简单
* 更多...


## 环境

### 软件
- Linux
- [Docker](https://www.docker.com/)
- [docker-compose](https://github.com/docker/compose)

### 硬件

用户需要自行准备一台至少包含两块网卡的计算机设备。其中一个网卡用来接收交换机镜像的流量，另一个网卡则用于通过Web API向其它应用提供数据。

用户提供的计算机设备最小配置要求如下：

| 参数项 | 最小配置 | 推荐配置 |
|------|----------|----------|
| CPU  | 四核   | 八核    |
| 内存 | 8G     | 16G      |
| 存储 | 100G  | 1T       |


## 安装方法

依赖环境安装：

```bash
$ curl -fsSL https://get.docker.com/ | sh
$ yum -y install docker-compose unzip
$ systemctl start docker
$ systemctl enable docker
```

项目快速部署方法：

**第一步**：点击 [这里](https://github.com/DSO-Lab/passets/archive/master.zip) 下载最新的部署文件包并解压缩：

```bash
$ curl -L https://github.com/DSO-Lab/passets/archive/master.zip -o master.zip
$ unzip master.zip
$ cd passets-master/
```

**第二步**：根据所的软/硬件平台修改对应 [`docker-compose`](#directory) 配置文件中下列参数：

```
services：
    ...
    sensor：
        ...
        environment：
        # 流量镜像网卡配置
        - interface=<网卡编号>
        # 是否开启详细http数据分析（包含：Http Body、Http Header、详细应用指纹识别等）
        - switch=on
```

**第三步**：下载最新的指纹库

``` bash
$ curl -L https://github.com/AliasIO/Wappalyzer/raw/master/src/apps.json -o ./rules/apps.json
```

**第四步**：数据目录赋权

``` bash
$ chmod -R 777 ./data
```

**第五步**：启动应用:

``` bash
$ docker-compose up -d
```

启动后等待一段时间，则可以通过下面的地址访问 API 接口：

http://x.x.x.x:8081/swagger-ui.html#/

或者通过集成的 Kibana 进行可视化查询或展示（[Kibana 配置方法示例](docs/KIBANA_HELP.md)）：

http://127.0.0.1:5601/


## 帮助文档

帮助文档位于 [```docs```](./docs) 目录


## 目录结构

```
├─ docker-compose.yml        # 适用于 x86_64 平台的 docker-compose 配置文件
├─ docker-compose_armv7.yml  # 适用于 ARMv7 平台的 docker-compose 配置文件
├─ rules                     # 指纹库文件夹
│  └─ apps.json              # Wappalyzer开源项目指纹文件，默认安装为空，上线前下载最新的指纹文件替换
└─data                       # 容器数据目录
   ├─ elasticsearch          # ES 数据目录
   ├─ logstash               # Logstash 数据目录
   ├─ kibana                 # Kibana 数据目录
   └─ logs                   # E.L.K 日志目录
```

## 指纹库升级

[下载](https://github.com/AliasIO/Wappalyzer/raw/master/src/apps.json) 最新指纹库，覆盖 `rules/apps.json` 文件，然后重启 `logstash` 容器即可生效。

## 链接

* [修改日志](./CHANGELOG.md)
* [缺陷跟踪](https://github.com/DSO-Lab/passets/issues)
* [软件授权](./LICENSE)
