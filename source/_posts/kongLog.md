title: 基于Kong的http log插件
date: 2018-07-26 21:11:19
tags: Kong
---

##### 在上一篇中，Kong已经搭建完成，作为Api-Gateway，各个http请求的日志记录是必不可少的，目前在Kong的 free plugins中，比较常用的有这么三个：`Syslog`、`File-Log`以及`Http-Log`，下面对这三种插件逐一分析一下。
<!--more-->

#### [Syslog](https://docs.konghq.com/plugins/syslog)
顾名思义，这个插件是把Kong中记录的日志给打印到系统日志中，开启插件之后只需要指定需要使用的API，无需做多余的配置，即可在`/var/log/message`中发现对应的日志信息，d 但是系统日志鱼龙混杂，如果需要用到ELK等日志分析工具时，需要做一次数据清洗工作。

#### [File-Log](https://docs.konghq.com/plugins/file-log)
与Syslog一样，File-log的配置也很方便，只需要配置日志路劲就行，开启插件之后，会在对应的对应产生一个logFile。Syslog中提到需要做一些日志清洗工作，但是换成了File-log乍一看好像解决了之前的痛点，实则不然，官方建议这个插件不适合在生产环境中使用，会带来一些性能上的开销，影响正常业务。

#### [Http-Log](https://docs.konghq.com/plugins/http-log)
http-log是我比较推荐的，它的原理是设置一个log-server地址，然后Kong会把日志通过post请求发送到设置的log-server，然后通过log-server把日志给沉淀下来，相比之前两种插件，这一种只要启一个log-server就好了，出于性能考虑，我用Rust实现了一个[log-server](https://github.com/Makcy/log-server)，有兴趣可以参考看一下。


