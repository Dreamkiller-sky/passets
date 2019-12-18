### elasticsearch7.4.1安装

elasticsearch7.4.1安装，详细说明，可以参见[官方支持文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/docker.html)。



### 安装参考 - CentOS 7.3



**Pulling the image**
Obtaining Elasticsearch for Docker is as simple as issuing a docker pull command against the Elastic Docker registry.

```
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.4.1
```

**Starting a single node cluster with Docker**：
To start a single-node Elasticsearch cluster for development or testing, specify single-node discovery to bypass the bootstrap checks:

```
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.4.1
```

Starting a multi-node cluster with Docker Compose
To get a three-node Elasticsearch cluster up and running in Docker, you can use Docker Compose:

### Create a docker-compose.yml file:

### docker-compose

```
version: "3"

services:
  elasticsearch:
    image: elasticsearch:7.4.1
    container_name: passets-elasticsearch
    environment:
      - TZ=Asia/Shanghai
#      - ES_JAVA_OPTS=-Xms256m -Xmx512m
      - discovery.type=single-node
    ports:
      - "127.0.0.1:9210:9200"
    volumes:
      - ./data/logs:/usr/share/elasticsearch/logs
      - ./data/elasticsearch:/usr/share/elasticsearch/data
    networks:
      - passets
    restart: unless-stopped
```
	
Run docker-compose to bring up the cluster:

### docker-compose运行
```
docker-compose up
```

