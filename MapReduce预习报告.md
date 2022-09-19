# MapReduce预习报告

分布式计算框架MapReduce
MapReduce用在大型商用硬件集群中对海量数据实施可靠，高容错的分布式计算的框架（也是经典并行计算机模型）。
其基本原理是把一个复杂的数据集分成若干个简单的数据块进行解决（Map函数）；之后对子问题的结果进行合并（Reduce函数),得到原有问题的结果
1.MapReduce编程模型
MapReduce编程模型由Mapper类和Reducer类构成，Mapper对切分过的原始数据进行处理，Reducer对Mapper的结果进行汇总，得到结果。
据工作原理，将MapReduce编程模型分为MapReduce简单模型和MapReduce复杂模型。
WordCount是最简单也是最能体现MapReduce思想的程序之一。
2.MapReduce数据流
MapReduce的编程模型中，数据以不同形式在不同节点间流动，经过本结点的分析处理，以另一形式进入下一个结点，得出结果。Mapper处理的是<key,value>形式的数据，不可以直接处理文件流。
（1）分片，格式化数据源（InputFormat)
InputFormat主要有两任务：对源文件进行分片，确定Mapper的数量；对各分片进行格式化，处理为<key,value>形式数据流传给Mapper
(2)Map过程
解析key值，若遇到空字符，就把之前累计的字符串作为输出的key值，以1作为i当前key 的value值，形成<word,1>的形式。
（3）Combiner过程
对map()端的输出先做一次合并，减少结点间的数据传输量，提高网络I/O性能。
（4）Shuffle过程
Shuffle过程分为两阶段，Mapper端的Shuffle和Reduce端的Shuffle.
该过程是从Mapper产生的直接输出结果，经处理，成最终的Reduce直接输入数据为止的全过程，是核心过程。
（5）Reduce过程
接收<key,{value list}>形式的数据流，形成
<key,value>形式的数据输出，输出数据直接写入HDFS
3.MapReduce任务运行流程
（1）MRv2基本组成
它舍弃了MRv1中的jobTrack和TaskTrack,采用新的MRAppMaster进行单一任务管理，与Yarn 中的Resource Manager和NodeManager 协同调度与控制任务。
MRv2基本组成：（1）客户端，用于向Yarn集群提交任务，通过ApplicationClient协议与Yarn的ResourceManager通信；（2）MRAppMaster,监控调度一整套MR任务流程，只负责任务管，不负责资源调配。（3）Map Task和Reduce Task,只能运行在yarn给定的资源限制下，由MRAppMaster和NodeManager协同管理调度


