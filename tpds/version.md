# Version TODO List

## v1.1 TODO

## v1.2 TODO

- ~~delete fig 7~~
- ~~move online ref to footnote: 11, 14, 15, 20, 21~~
- ~~format subsection4~~
- ~~add biography~~
- ~~add acknowledge~~
- At least 3 tpds ref
- ~~model也是创新点，在conclusion 摘要  引言中都要提及~~

## v1.3 TODO

- complete biography
- At least 3 tpds ref

## v2.1

- FRQ model displays utilization of resources in time dimension and calculate exectuion  -》 execution
- 统一SCache写法
- , shuffle phase delay the execution time of reduce phase.  -> delays
- which might introduces extra network traffic in cluster.  => introduce
- , the shuffle dependecies are determined based on job configuration.  =》 dependencies

## v2.2

- . shuffles is the previous schedule result -》 are
- 统一MapReduce
- n Spark, We mainly modified the DAGScheduler and the corresponding data fetcher. It only takes about 500 lines of code.  -》 we
- Such hundreds of lines of code modification is very small compared to the hundreds of thousands of lines of code in DAG framework.   -> are
- The overhead depends on the parallel time of shuffle phase and the Reduce phase.The    -》 phase.  The
- Because the computatinon of the Reduce phase relies on the data transfer results of the Shuffle phase  -》 computation
- Fig. 10: FRQ Model With Different Scheduling Strategies -》 with  介词小写
- Another optimization method is to use the idle IO resources in the Map phase for pre-fetching(see Figure 9). -》 pre-fetching (see Figure 9）
- 括号右侧空格
- Fig. 11: FRQ Model With SCache -》 with
- we use Hadoop Mapreduce as framwork  -》 framework
- We deploye Hadoop with SCache and without SCache on both environments.  -> deployed
- The network utilization is 5 regular peaks, this situation is also consistent with Figure 11b  逗号不能连两个句子 -> The network utilization has 5 regular peaks. This situation is also consistent with
- such errors are within tolerance .  -》 such errors are within tolerance. 句号前多了一个空格
- Therefore, We believe that FRQ model can accurately describe DAG framework.  -》 we
- which is consistent with the results in Section ??.  有问号
- Because the large amount of shuffle data reaches the network bottleneck, The beginning part of reduce needs to wait needs to wait for network transfer  -》 the
- pre-fetching utilize the idle IO throughput in the Map phase.   -》 utilizes
- The execution is seperated into the phases: Read, Map, Collect, Spill, Merge, Shuffle, Merge, and Reduce.  -》 separated
- 完成bio
- 完成作者，添加通讯作者

## fzw revision

- shuffle-sensitive -> shuffle-heavy
- textit groupbytest
- varied  application  -scenarios-.
- modify section4 description.
- original scheduler in framework -master-.
- When a DAG job is submitted, the DAG information will be generated in framework task scheduler.
- Before the computing tasks begin, the shuffle dependencies are determined based on DAG.
- modification in subsection Analysis of cross-framework capability.
- taking a simple MapReduce job.
- IO -> I/O