title: Kong Admin GUI之Kong-Dashboard到Konga的过渡
date: 2018-08-15 01:44:00
tags: Kong
---

由于Kong只在企业版中提供了Admin GUI的功能，所以对于大多数白手起家的开发者来说，寻找一个免费、开源、骚包的Admin GUI是一件非常重要的事， 在之前Kong的快速搭建中用的是`Kong-Dashborad`，这个GUI给人的第一感觉就是太Low逼了，并且只支持到了Kong 0.13.0，要不是找不到合适的我说什么也不会用它。直到某一天，发现了一个神器 `Konga`，不仅支持了Kong的最新版本（service和route的拆分新特性）同时支持管理员的权限控制和多个Kong连接池的管理。
<!--more-->
放两张对比图，你们感受一下
![image](http://cdn.image.huoqiuapp.com/archive/image/2b924791129b3fc87fcb_2870_1556.png)
![image](http://cdn.image.huoqiuapp.com/archive/image/95b711884d6fa0a50411_2864_1558.png)

相信你们心里已经有*数了
Konga安装还是很方便的，这里是[仓库地址](https://github.com/pantsel/konga)，但是安装的时候有一些坑，Konga由于自带了用户权限控制和Kong连接池管理，所以需要一些数据持久化处理；默认支持的数据库有mongodb、postgres、mysql；本人尝试过postgres的使用，因为文档的不完善，绕了很多弯路。这里推荐大家使用mongodb，可以省去很多不必要的步骤，如果启动最为方便。当然，最佳方案还是通过用Docker启动，具体方式可以参考README里的叙述
