# Revision Response

### Modification List

1. We add a new subsection about fault tolerance in the Implementation Section.
2. We extend the related work Section and add more discussion about shuffle optimization and DAG scheduling.
3. We add new content and modify each subsection in Section II to better detail the shuffle optimization.
4. We modify some paragraphs and add some new ones in Section 5 to better explain the FRQ model.
5. We update the description of Algorithm 1&2 and also discuss their complexity in Section II.
6. We modify Section V to explain how and why we choose TPC-DS as a standard benchmark to simulate the realistic industrial workload.
7. We modify Subsection V.C to discuss the different performance gain between Spark and Hadoop MapReduce.
8. In addition to the above modifications, we also paraphrase all other parts of the paper to ensure the similarity is low enough.

### Reviewer 1

6. Discussion about fault tolerant:
   - What happens if some workers fail before reading the shuffle data by getBlock API?
   - Do you need to change the code of MapReduce for guaranteeing the fault tolerance?

    **Answer**: We add a new subsection about fault tolerance in the Implementation Section.
    SCache only restarts failed workers without recovering the data.
    If a failure happens, the $getBlock$ API called by DAG frameworks  will return a data not found error. This will cause the DAG frameworks to re-compute the current stage.
    We leave the fault handling to the DAG frameworks.
    As a result, we do not need any changes in MapReduce for guaranteeing the fault tolerance.

7. we read: “After scheduling, if the new assigned id of a reduce task did not equal the original one, a re-shuffle will be triggered to transfer data to the new host.”.Why the assigned id of a reduce task may be different than the original one? If happened, what is the impact of this re-shuffling on performance?

    Answer: 
    1. In the multi-shuffle dependencies, when a new shuffle starts, the predicted parameters are accumulated with previous shuffles. And then SCache uses these parameters to re-schedule the assignment. Therefore, the new assignment may be different from the original ones.
    2. If re-shuffle happens, it will cause extra data shuffle overhead. But this re-shuffle is rare and can only affect the performance slightly.
    <!-- **Answer**: 1.在多轮shuffle的情况下，每当预测完新的shuffle数据分布，新的预测会与先前的结果叠加，算法会重新进行计算。因此有可能出现原本分配的结果不同。
    2.如果re-shuffle发生会产生额外网络开销用于重新传输数据，但是这种情况出现几率很低，我们认为并不会影响整体性能，在实际benchmark实验结果中也没有看到平凡re-shuffle造成的额外网络开销。 -->

8. Why SCache obtains better performance gains in Spark than MapReduce?

    <!-- different workflow and benchmarks -->
    **Answer**:
    The main reason causes the different performance gain in Spark and MapReduce is that they have quite different DAG workflows.
    Unlike Spark's complex DAG computing, Hadoop MapReduce has a simple DAG computing workflow which has only one Map phase and one Reduce phase.
    Such simple DAG of MapReduce alleviates the performance gain of SCache's shuffle data prediction.
    Besides, the Terasort benchmark we used generates input data by its own, which means that heavy data skew is unlikely to happen. 
    Therefore, the performance gain of in MapReduce is mainly rely on SCache' pre-fetching strategy and this is also why we choose Terasort benchmark which is a shuffle-heavy task.
    We also modify Subsection V.C to discuss the different performance gain between Spark and Hadoop MapReduce.
    <!-- **Answer**: (较难回答)*根本原因是因为Hadoop实验时的优化在pre-shuffle的数据没有放在Memory，而是放在disk，所以效果没那么明显，只体现了pre-shuffle的优化。而Spark的实验中shuffle数据是in-memory，所以省去了disk读，实验结果不仅体现了pre-shuffle，还体现了in-memory的优化。* -->

9. Move "Model evaluation" to the "Evaluation" section.

    **Answer**: OK, we move the subsection into the Evaluation.

### Reviewer 2

6. Add related work:
   - "LEEN: Locality/Fairness-Aware Key Partitioning for MapReduce in the Cloud," 2010
   - Transformation-Based Monetary CostOptimizations for Workflows in the Cloud
   - the origin paper in PPoPP

    **Answer**: We extend the related work Section and add more discussion about shuffle optimization and DAG scheduling.
    We add a new subsection which names _DAG Optimization_.
    In the new subsection, we discuss the related work in shuffle optimization, including slow-start, Starfish [24], iHadoop [26], iShuffle [15], and so on.
    We also discuss some related work about DAG scheduling, including Skewtune [14], LIBRA [27], and ToF [28].

7. About experiments:
   - version of Hadoop and Spark
   - evaluate the scalability of the proposed approach with the number of nodes varied.
   - evaluate your approach with more realistic industrial workload besides benchmarks like TPC-DS

    <!-- big data in industry -> TPC family -> we choose DS -> we believe  -->
    <!-- In industry, big data technologies are widely used on these systems, such as data mining, predictive analytics, text analytics, and statistical analysis. -->
    **Answer**: 
    SQL-Based Big Data Systems are widely used in industrial application, such as data mining, predictive analytics, text analytics, and statistical analysis.
    Because of the limited time and resource, we use standard benchmark instead of realistic industrial workload to evaluate SCache.
    TPC-DS models queries and data maintenance of a retail product supplier and provide a representative evaluation of performance.
    We believe using TPC-DS is able to evaluate the performance of SCache in industry.
    We also modify the first paragraph of Section V to explain how and why we choose TPC-DS as a standard benchmark to simulate the realistic industrial workload.

### Reviewer 3

1. Provide a detailed mathematical optimization framework in Section II regarding the shuffle optimization, e.g., by specifying the detailed objective function and the constraints

    **Answer**: We add new content and modify each subsection in Section II to better detail the shuffle optimization. We first discuss the reason which causes the shuffle overhead.
    Then we detail our _Heuristic MinHeap Scheduling_ algorithm with mathematical formulas and explain how we mitigate the overhead.
    <!-- We propose the _Heuristic MinHeap Scheduling_ algorithm to predict shuffle and do pre-scheduling. -->
    <!-- Then we present how we use proposed methodologies to mitigate the overhead. -->
    <!-- , so that gain the optimization. -->

2. Discuss the complexity of Algorithm 1 and Algorithm 2.

    **Answer**: We update the description of Algorithm 1&2 and also discuss their complexity in Section II. 
    For Algorithm 1, the algorithm needs $O(n)$ operations because it maintains a min-heap and traverses $reduce$ for swapping.
    For Algorithm 2, the algorithm also needs $O(n)$ operations because it only traverses $reduce$ for accumulating previous $shuffle$ and re-shuffling data.

3. Figure 5 needs more explanations:
   - what the rationale behind the proposed FRQ model?
   - How can it fit the requirements for evaluating the proposed SCache?

   **Answer**: As discussed in the paper, the FRQ model focuses on describing the shuffle overhead which is significantly effected by the scheduling strategy.
   <!-- This overhead not only depends on the input data size, computation speed and other parameters but also on which scheduling strategy it takes. -->
   By using the FRQ model, we can analyze the advantages of each strategy in various situations.
   Take the pre-fetching strategy in SCache as an example, the FRQ model shows that pre-fetching can get better performance than the original Hadoop MapReduce strategy by reducing T<sub>P_Shuffle</sub>.
   We also modify some paragraphs and add some new ones in Section 5 to better explain the FRQ model.

### Reviewer 4

1. Similarity score is too high.

     **Answer**: To ensure the similarity is low enough, we paraphrase all other parts of the paper, including Introduction, Optimization, and Implementation.
