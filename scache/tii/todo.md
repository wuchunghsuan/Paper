# Revision

### Reviewer 1

1. Discuss about fault tolerant:
   - What happens if some workers fail before reading the shuffle data by getBlock API?
   - Do you need to change the code of MapReduce for guaranteeing the fault tolerance?
2. we read: “After scheduling, if the new assigned id of a reduce task did not equal the original one, a re-shuffle will be triggered to transfer data to the new host.”.Why the assigned if of a reduce task may be different than the original one? If happened, what is the impact of this re-shuffleing on performance?
3. Why Scache obtains better performance gains in Spark than MapReduce?
4. Move "Model evaluation" to the "Evaluation" section.

### Reviewer 2

1. Add related work:
   - "LEEN: Locality/Fairness-Aware Key Partitioning for MapReduce in the Cloud," 2010
   - Transformation-Based Monetary CostOptimizations for Workflows in the Cloud
   - the origin paper in PPoPP
2. About experiments:
   - version of Hadoop and Spark
   - evaluate the scalability of the proposed approach with the number of nodes varied.
   - evaluate your approach with more **realistic industrial workload** besides benchmarks like TPC-DS

### Reviewer 3

1. Provide a detailed mathematical optimization framework in Section II regarding the shuffle optimization, e.g., by specifying the detailed objective function and the constraints
2. Discuss the complexity of Algorithm 1 and Algorithm 2
3. Figure 5 needs more explanations:
   - what the rationale behind the proposed FRQ model?
   - How can it fit the requirements for evaluating the proposed SCache?

### Reviewer 4

1. Similarity score is too high.