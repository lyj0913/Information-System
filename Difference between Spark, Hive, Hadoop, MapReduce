### Spark, Hadoop MapReduce, Kafka的区别

Spark runs faster

Spark运算比Hadoop的MapReduce框架快的原因是因为Hadoop在一次MapReduce运算之后，会将数据的运算结果从内存写入到磁盘中，第二次MapReduce运算时再从磁盘中读取数据，所以其瓶颈在2次运算间的多余I/O消耗。Spark则是将数据一直缓存在内存中，直到计算得到最后的结果，再将结果写入到磁盘，所以多次运算的情况下，Spark是比较快的

Hadoop MapReduce can process datasets that are much larger than Spark does. 

Apache Hive使SQL开发人员使用Hive查询语言 (HQL) 语句，类似于用于数据查询和分析的标准SQL。Hive可以在HDFS上运行，最适合数据仓库任务，例如提取、转换和加载 (ETL)、报告和数据分析。

