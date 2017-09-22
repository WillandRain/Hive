# 实用笔记

## [Hive](http://139.5.108.146/notebook/editor?type=hive)相关页面功能

## 一、基础语法

### 1.SELECT 语句
```
select column1,column2,column3,...
from table_name
where col = xxx;
```

注：column是列名，这个可以通过[Hive平台](http://139.5.108.146/notebook/editor?type=hive)查看

### 2.DISTINCT：去重

```
SELECT DISTINCT column1,column2,... FROM table_name
```

注：

distinct column1,column2 表示选取column1,和column2 不同的记录

|id|name|name2|
|---|---|---|
|1|a|A|
|1|b|B|
|1|a|a|

使用 `select distinct id,name from tb;` 

结果：

|id|name|
|---|---|
|1|a|
|1|b|

### 3.比较操作：LIKE,IN,BETWEEN

比较操作：LIKE,IN,BETWEEN 都是放在`where`语句里面的

举例：

[like](http://www.w3school.com.cn/sql/sql_like.asp)&in&between

max&min&count&avg&sum的聚合操作
 
`SELECT 聚合操作函数（column_name） FROM table_name`

group by 操作
 
`SELECT column_name FROM table_name WHERE condition GROUP BY column_name(s)`

实例：

```
select day,sum(totaltime) from bigolive.bigolive_owner_stats_orc
where day between "2017-09-18" and "2017-09-19"
and uid in (150xxxxx,151xxxxx)
group by day;
```

### 4.[联表查询](http://justcode.ikeepstudying.com/2016/08/mysql-%E5%9B%BE%E8%A7%A3-inner-join%E3%80%81left-join%E3%80%81right-join%E3%80%81full-outer-join%E3%80%81union%E3%80%81union-all%E7%9A%84%E5%8C%BA%E5%88%AB/)


## 二、json数据查询


### [需要知识点](http://www.w3school.com.cn/json/index.asp)


json格式 {'key1':'value1','key2':{'key3':'value3'}}

hive已经解析好了。 

现在只需要从中间提取就好，提取的语法如下：

新埋点数据-bigolive.bigo_show_user_event_orc

比如,事件id为010806003

```
select event from bigo_show_user_event_orc
where day = '2017-09-20'
and event_id = 010806003
limit 10;
```

结果：

`{"time":1505824795917,"lng":0,"lat":0,"net":"other","log_extra":{"bigo_show_popular":"LR_ranker_With_CF"},"event_id":"010806003","event_info":{"in_room":"0","uid":"1567912530","stay_time":"3509","live":"1","rank":"1"}}`

如果把event 改成event.time;结果就变成了`1505824795917`

那现在如果需要event改成event.event_info["uid"],结果就是`1567912530`

然后结合上面讲的语法，就能把需要的内容统计出来了

## 三、工作流和调度介绍

### 相关信息

传参方式 ：`${parameter}`

Hive XML : `/data/hive_oozie_conf/hive-site.xml`

Parameters ： `${coord:formatTime(coord:dateOffset(coord:nominalTime(), -1, 'DAY'), 'yyyy-MM-dd')}`
