---
title: 用Kibana和logstash快速搭建实时日志查询、收集与分析系统
---
       Logstash是一个完全开源的工具，他可以对你的日志进行收集、分析，并将其存储供以后使用（如，搜索），您可以使用它。说到搜索，logstash带有一个web界面，搜索和展示所有日志。
        kibana 也是一个开源和免费的工具，他可以帮助您汇总、分析和搜索重要数据日志并提供友好的web界面。他可以为 Logstash 和 ElasticSearch 提供的日志分析的 Web 界面

 简单来讲他具体的工作流程就是 logstash agent 监控并过滤日志，将过滤后的日志内容发给redis(这里的redis只处理队列不做存储)，logstash index将日志收集在一起交给
全文搜索服务ElasticSearch 可以用ElasticSearch进行自定义搜索 通过Kibana 来结合 自定义搜索进行页面展示，下图是 Kibana官网上的流程图
![1](/images/LogSystem/1.png)
具体的网址：http://blog.51cto.com/storysky/1158707