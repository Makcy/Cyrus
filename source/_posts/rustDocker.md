title: 基于Rust项目的最小Docker镜像的构建
date: 2018-08-04 02:49:09
tags: Docker
---
通常我们打包一个Rust完整环境的镜像如图所示，大的一比
![image](http://cdn.image.huoqiuapp.com/archive/image/7e702ef8c83d0bd773f6_1576_382.png)
<!--more-->

但是如果我们采用Alpine作为Rust的运行环境，同时用Apk进行Rust的安装，并且当项目依赖`cargo build --release`完成之后，再把多余的依赖包给剔除，那么就能得到一个超级精简包含项目可执行文件的Docker Image，在此提供之前基于kong http-log而开发的一款log-server的Dockfile作为参考：
```
FROM alpine:latest

COPY ./ /app
WORKDIR /app

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
	&& mkdir $HOME/.cargo \
	&& mv config $HOME/.cargo/ \
	&& apk add --no-cache libgcc \
	&& apk add --no-cache --virtual .build-rust rust cargo \
	&& cargo build --release \
	&& cp target/release/log-server .\
	&& rm -rf target/ ~/.cargo/ \
	&& apk del --purge .build-rust \
	&& mkdir -p /data/logServer/

EXPOSE 3000

ENTRYPOINT ["./log-server"]
```