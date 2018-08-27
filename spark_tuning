    仔细阅读官方指导文档：https://spark.apache.org/docs/latest/tuning.html
    仔细阅读美团相关博客：https://tech.meituan.com/spark_tuning_basic.html https://tech.meituan.com/spark_tuning_pro.html
    学会阅读spark web ui : http://shiyanjun.cn/archives/category/opensource/spark

读懂上面几篇文章后，具有一定spark开发经验、了解spark任务计算模型的同学已经有了一定的调优能力，下面是我在日常开发中一些小技巧，积少成多可以省下不少时间：

    减少编译时间：使用mvn -T 2C package代替 mvn clean package
    减少上传时间：一些非必要的依赖包scope选择provided, 压缩上传可以通过脚本一键执行等等工作
    打印spark配置参数： spark-submit后加上 --verbose
    配置好本地测试环境(可以在本地连接hdfs/hbase, 运行spark程序等), 构建本地测试数据集，写好单元测试， 尽量避免跑全量数据进行测试

在实际的spark任务开发中， 遇到不少疑难杂症，也吸取了一些值得记录的经验教训：

    知道在spark程序中print与LOG日志的区别， 以及了解print日志打印的位置位于driver上还是excutor上
    知道如何看yarn cantainer的日志： yarn logs -applicationId application_id (必须有对应队列权限)
    通过web ui ThreadDump 观察当前线程堆栈，找出瓶颈在哪
    学会使用jvm监控工具：jstack(堆栈日志)， jmap(内存对象)， jstat(GC)， 当task阻塞时找到对应线程观察其状态，定位并解决问题
    设置jvm -XX:G1GC, 代替原生CMS垃圾收集器(非必须， default is pretty good)
    读取hdfs文件时， 默认partition数是hdfs block个数，可以通过设置spark.default.parallelism 增加输入partition个数， 从而减少每个partition的数据量
    shuffle的时候， partition个数默认是父RDD个数，通过设置spark.sql.shuffle.partitions增加shuffle partition个数
    了解spark内存管理： https://www.ibm.com/developerworks/cn/analytics/library/ba-cn-apache-spark-memory-management/index.html
    当大部分task在shuffle read的时候运行缓慢， 或者GC时间长， 或者shuffle failed, 可以通过调高spark.shuffle.memoryFraction来解决问题
    与上一条相反，当某个spark job shuffle任务较少， 但缓存数据较多时，调高spark.shuffle.storageFraction

以上的一些建议， 已经能够解决我遇到的大部分spark性能瓶颈问题，当然具体情况还得具体分析， 如果以后有什么新的调优收获，也会在这里补上
