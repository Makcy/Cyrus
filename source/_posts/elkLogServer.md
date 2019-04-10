title: 用ELK进行Kong的日志分析
date: 2018-08-22 01:10:15
tags: [ELK, Kong]
---

之前我们用了LogServer来讲Kong的日志进行一个落地，既然数据源有了，那么接下来的工作必然是对日志进行一个分析，使日志物尽其用达到其应有的效果。
目前比较主流的日志分析方案是ELK(Elasticsearch + Logstash + Kibana)进行一个日志的分析，当然之后Logstash这一日志收集和过滤层演变为了`Beat + Logstash` 又或者是 `Beat + Logstash + Queue + Logstash`，这里贴一张ELK的架构图，具体不过多描述。
![image](https://cdn.image.huoqiuapp.com/archive/image/13ae8758a489e6e7fbc4_1122_577.png)
我采用的是Filebeat进行日志的收集工作，我们的日志文件格式以`JSON`为一个最小单元，使用Filebeat进行手机的时候需要注意，每个JSON对象之间需要有一个换行，不然Filebeat将无法识别，我这有一个[Demo](https://github.com/Makcy/elk-analyze-demo)，里面已经附上了docker-compose文件，只需启动docker-compose便可以看到效果，访问Kibana然后建立Index，便可以看到对应的日志。
