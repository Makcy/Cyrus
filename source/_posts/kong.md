title: Kong GateWay 快速搭建
date: 2018-07-20 11:33:42
tags: Kong
---

##### 采用Docker快速搭建Kong Api-GateWay & Kong-Dashboard可视化界面
1、安装Docker运行环境
2、创建docker network 
```
docker network create kong-net
```
3、Kong支持cassandra和postgreql两种数据库，这里使用postgresql
```
docker run -d --name kong-database \
                --network=kong-net \
                -p 5432:5432 \
                -e "POSTGRES_USER=kong" \
                -e "POSTGRES_DB=kong" \
                postgres:9.5
```
<!--more-->
4、加载Kong并进行数据库migration, `截止此时Kong-Dashboard只支持0.13版本Kong`
```
docker run --rm \
    --network=kong-net \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
    kong:0.13.1 kong migrations up
```
5、启动Kong Container
```
docker run -d --name kong \
    --network=kong-net \
    -e "KONG_DATABASE=postgres" \
    -e "KONG_PG_HOST=kong-database" \
    -e "KONG_LOG_LEVEL=info" \
    -e "KONG_ADMIN_LISTEN=0.0.0.0:8001" \
    -e "KONG_ADMIN_LISTEN_SSL=0.0.0.0:8444" \
    -p 8000:8000 \
    -p 8443:8443 \
    -p 8001:8001 \
    -p 8444:8444 \
    kong:0.13.1
```
6、Kong Dashboard 启动！ 
```
docker run -d --rm \
    --network=kong-net \
    -p 8080:8080 \
    pgbi/kong-dashboard \
    start --kong-url http://kong:8001
```