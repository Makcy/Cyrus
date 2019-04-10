title: Elastic APM接入实践
date: 2019-02-09 15:05:09
tags: [ELK, APM]
---

前言(Newrelic)
当今的APM种类繁多，本文采用的是NodeJS集成
优势：
1、NewRelic收费，且前端界面打开满
2、ELK开源，可免费使用，节约成本
3、与ELK日志统计在同一系统内，数据都存储于ELK中，相对方便

环境 Centos7
报警功能感知相对弱
https://github.com/nswbmw/node-in-debugging/blob/master/5.1%20NewRelic.md