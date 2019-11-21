---
title: elasticsearch的安裝
date: 2018-05-05 21:58:46
tags:
---
-- 1.检查jdk安装

执行命令：java -version
如果有java相关信息，说明已经安装。
如果没有安装的话，执行命令：yum -y install  java-1.8.0-openjdk
-- 2.下载elasticsearch安装包

执行命令：

    wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/tar/elasticsearch/2.3.4/elasticsearch-2.3.4.tar.gz

-- 3.解压和配置elasticsearch

解压执行：tar -zxvf elasticsearch-2.3.4.tar.gz
执行：cd elasticsearch-2.3.4 && cd bin && ./elasticsearch
执行之后，会出现问题提示：

    Exception in thread "main" java.lang.RuntimeException: don't run elasticsearch as root.
        at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:93)
        at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:144)
        at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:285)
        at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:35)
        Refer to the log for complete error details. 

-- 4.解决办法

需要创建elasearch用户和用户组
创建组： groupadd elasearch
创建对应的用户： useradd elasearch -g elasearch -p rootsearch
回到elasticsearch-2.3.4的上级目录执行以下命令：
chown -R elasearch:elasearch elasticsearch-2.3.4/
切换到elasearch用户： su elasearch
进入bin目录执行 ：./elasticsearch -d
成功后，执行命令： curl localhost:9200,结果如下：

    {
    "name" : "Thog",
    "cluster_name" : "elasticsearch",
    "version" : {
    "number" : "2.3.4",
    "build_hash" : "e455fd0c13dceca8dbbdbb1665d068ae55dabe3f",
    "build_timestamp" : "2016-06-30T11:24:31Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.0"
    },
    "tagline" : "You Know, for Search"
    }


