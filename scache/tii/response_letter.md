# Response to Reviewers

Dear Editor and Reviewers,

Thank you for the reviewers’ comments concerning our manuscript entitled *“Efficient Shuffle Management for DAG Computing Frameworks Based on the FRQ Model”* (ID TII-19-1638).
Those comments are all valuable and helpful for revising and improving our paper.
We have studied the comments carefully and have made our best to improve the paper.
The main corrections in the paper and the responses to the reviewer’s comments are as following:


### List of Revisions

1. We add a new subsection about fault tolerance in the Implementation Section.
2. We extend the related work Section and add more discussion about shuffle optimization and DAG scheduling.
3. We add new content and modify each subsection in Section II to better detail the shuffle optimization.
4. We modify some paragraphs and add some new ones in Section 5 to better explain the FRQ model.
5. We update the description of Algorithm 1&2 and also discuss their complexity in Section II.
6. We modify Section V to explain how and why we choose TPC-DS as a standard benchmark to simulate the realistic industrial workload.
7. We modify Subsection V.C to discuss the different performance gain between Spark and Hadoop MapReduce.
8. In addition to the above modifications, we also paraphrase all other parts of the paper to ensure the similarity is low enough.

### Responses to Reviewer #1

<!-- - Discussion about fault tolerant:
   - What happens if some workers fail before reading the shuffle data by getBlock API?
   - Do you need to change the code of MapReduce for guaranteeing the fault tolerance? -->
- **Comment #1:**
    The proposed approach uses the system memory for keeping the shuffle data. But, the impact of this strategy on “fault tolerance” is not clear. What happens if some workers fail before reading the shuffle data by getBlock API? Do you need to change the code of MapReduce for guaranteeing the fault tolerance?

    **Response:** We add a new subsection about fault tolerance in the Implementation Section.
    SCache only restarts failed workers without recovering the data.
    If a failure happens, the *getBlock* API called by DAG frameworks  will return a data not found error. This will cause the DAG frameworks to re-compute the current stage.
    We leave the fault handling to the DAG frameworks.
    As a result, we do not need any changes in MapReduce for guaranteeing the fault tolerance.

- **Comment #2:**
    In Page 4, we read: “After scheduling, if the new assigned id of a reduce task did not equal the original one, a re-shuffle will be triggered to transfer data to the new host.”. Why the assigned id of a reduce task may be different than the original one? If happened, what is the impact of this re-shuffling on performance?

    **Response:**
    1. In the multi-shuffle dependencies, when a new shuffle starts, the predicted parameters are accumulated with previous shuffles. And then SCache uses these parameters to re-schedule the assignment. Therefore, the new assignment may be different from the original ones.
    2. If re-shuffle happens, it will cause extra data shuffle overhead. But this re-shuffle is rare and can only affect the performance slightly.
    <!-- **Response**: 1.在多轮shuffle的情况下，每当预测完新的shuffle数据分布，新的预测会与先前的结果叠加，算法会重新进行计算。因此有可能出现原本分配的结果不同。
    2.如果re-shuffle发生会产生额外网络开销用于重新传输数据，但是这种情况出现几率很低，我们认为并不会影响整体性能，在实际benchmark实验结果中也没有看到平凡re-shuffle造成的额外网络开销。 -->

- **Comment #3:** I don’t understand why SCache obtains better performance gains in Spark than MapReduce. It is known that Spark is more optimized than MapReduce, so logically there should be less place for more optimizations in Spark than MapReduce. Please discuss this in the paper.

    <!-- different workflow and benchmarks -->
    **Response**:
    The main reason causes the different performance gain in Spark and MapReduce is that they have quite different DAG workflows.
    Unlike Spark's complex DAG computing, Hadoop MapReduce has a simple DAG computing workflow which has only one Map phase and one Reduce phase.
    Such simple DAG of MapReduce alleviates the performance gain of SCache's shuffle data prediction.
    Besides, the Terasort benchmark we used generates input data by its own, which means that heavy data skew is unlikely to happen. 
    Therefore, the performance gain of in MapReduce is mainly rely on SCache' pre-fetching strategy and this is also why we choose Terasort benchmark which is a shuffle-heavy task.
    We also modify Subsection V.C to discuss the different performance gain between Spark and Hadoop MapReduce.
    <!-- **Response**: (较难回答)*根本原因是因为Hadoop实验时的优化在pre-shuffle的数据没有放在Memory，而是放在disk，所以效果没那么明显，只体现了pre-shuffle的优化。而Spark的实验中shuffle数据是in-memory，所以省去了disk读，实验结果不仅体现了pre-shuffle，还体现了in-memory的优化。* -->

- **Comment #4:** I think, the subsection “Model evaluation” should be moved to the “evaluation” Section. If you agree, pleas move it there.

    **Response**: OK, we move the subsection into the Evaluation.

### Responses to Reviewer #2

- **Comment #1&3:** The paper optimizes data shuffling. However, little related work has been reviewed on this dimension. Please check out the below paper and its related papers and review them in related work section.

    - S. Ibrahim, H. Jin, L. Lu, S. Wu, B. He and L. Qi, "LEEN: Locality/Fairness-Aware Key Partitioning for MapReduce in the Cloud," 2010 IEEE Second International Conference on Cloud Computing Technology and Science, Indianapolis, IN, 2010, pp. 17-24.

    DAG-aware scheduling is also a hot research area. The author should review the related work carefully. Below paper is a paper for you to start with.

    - A. C. Zhou and B. He, "Transformation-Based Monetary CostOptimizations for Workflows in the Cloud," in IEEE Transactions on Cloud Computing, vol. 2, no. 1, pp. 85-98, Jan.-March 2014.

    **Response**: We extend the related work Section and add more discussion about shuffle optimization and DAG scheduling.
    We add a new subsection which names _DAG Optimization_.
    In the new subsection, we discuss the related work in shuffle optimization, including slow-start, Starfish [24], iHadoop [26], iShuffle [15], and so on.
    We also discuss some related work about DAG scheduling, including Skewtune [14], LIBRA [27], and ToF [28].

- **Comment #2:** About experiments:
   - it is unclear which versions of Hadoop and Spark are used.
   - please evaluate the scalability of the proposed approach with the number of nodes varied.
   - can you evaluate your approach with more realistic industrial workload besides benchmarks like TPC-DS?

    <!-- big data in industry -> TPC family -> we choose DS -> we believe  -->
    <!-- In industry, big data technologies are widely used on these systems, such as data mining, predictive analytics, text analytics, and statistical analysis. -->
    **Response**: 
    SQL-Based Big Data Systems are widely used in industrial application, such as data mining, predictive analytics, text analytics, and statistical analysis.
    Because of the limited time and resource, we use standard benchmark instead of realistic industrial workload to evaluate SCache.
    TPC-DS models queries and data maintenance of a retail product supplier and provide a representative evaluation of performance.
    We believe using TPC-DS is able to evaluate the performance of SCache in industry.
    We also modify the first paragraph of Section V to explain how and why we choose TPC-DS as a standard benchmark to simulate the realistic industrial workload.

### Responses to Reviewer #3

- **Comment #1:** The shuffle optimization is an important contribution in this work. In particular, it is interesting to present the detailed mathematical problem formulation regarding the shuffle optimization. Thus, could this paper provide a detailed mathematical optimization framework in Section II regarding the shuffle optimization, e.g., by specifying the detailed objective function and the constraints?

    **Response**: We add new content and modify each subsection in Section II to better detail the shuffle optimization. We first discuss the reason which causes the shuffle overhead.
    Then we detail our _Heuristic MinHeap Scheduling_ algorithm with mathematical formulas and explain how we mitigate the overhead.
    <!-- We propose the _Heuristic MinHeap Scheduling_ algorithm to predict shuffle and do pre-scheduling. -->
    <!-- Then we present how we use proposed methodologies to mitigate the overhead. -->
    <!-- , so that gain the optimization. -->
- **Comment #2:** It will be more convincing if the paper could discuss the complexity of Algorithm 1 and Algorithm 2 in Section II.

    **Response**: We update the description of Algorithm 1&2 and also discuss their complexity in Section II. 
    For Algorithm 1, the algorithm needs *O(n)* operations because it maintains a min-heap and traverses *reduce* for swapping.
    For Algorithm 2, the algorithm also needs *O(n)* operations because it only traverses *reduce* for accumulating previous *shuffle* and re-shuffling data.

- **Comment #3:** The proposed FRQ model (for evaluating the performance of DAG frameworks) in Figure 5 needs more explanations. In particular, what the rationale behind the proposed FRQ model? How can it fit the requirements for evaluating the proposed SCache? More explanations are thus required.

   **Response**: As discussed in the paper, the FRQ model focuses on describing the shuffle overhead which is significantly affected by the scheduling strategy.
   <!-- This overhead not only depends on the input data size, computation speed and other parameters but also on which scheduling strategy it takes. -->
   By using the FRQ model, we can analyze the advantages of each strategy in various situations.
   Take the pre-fetching strategy in SCache as an example, the FRQ model shows that pre-fetching can get better performance than the original Hadoop MapReduce strategy by reducing T<sub>P_Shuffle</sub>.
   We also modify some paragraphs and add some new ones in Section 5 to better explain the FRQ model.

### Responses to Reviewer #4

- **Comment #1:** Although authors declare that this is an extended paper, there is a high similarity score (roughly 50%) with previous published papers. This score is too high and must be reduced before to reconsider the paper.

     **Response**: To ensure the similarity is low enough, we paraphrase all other parts of the paper, including Introduction, Optimization, and Implementation.

We have tried our best to improve the manuscript and make sure these changes will not influence the content and framework of the paper.
We appreciate for Editors/Reviewers’ warm work earnestly, and hope that the correction will meet with approval.

Once again, thank you very much for your comments and suggestions.

