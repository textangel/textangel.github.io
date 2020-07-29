# Lecture 1a - Spark and Spark vs MapReduce
Generally spark is faster with Hadoop but with caveats
	-	Spark can process data in memory, Hadoop persists it back to disk after map and reduce.
		- So Spark generally outperforms MapReduce, but requires a lot of memory. If there is not enough memory, spark degrades.
		- MapReduce runs well along size other services, and woks well for the 1-pass jobs it was designed for.
Spark is easier to program, spark is more general.
Spark assumed your entire data fits in memory. MapReduce needs zero memory.

Hive is a map reduce SQL (bigger data) and SparkSQL is a spark based SQL.

**Problems suitable for map reduce**. One pass problems. Use MapReduce or Spark.
	- we have a large file (large web corpus), and we want to count something or 
	- Link analysis and graph processing
	- machine learning algos
	- statistical machine translation
	- giant table joins

**Problems not suitable for map reduce.** Use Spark.
MapReduce is inefficient when you have random accesses
	- Graphs
	- Interdependent data
		- Machine learning, Comparisons of many pairs of items
	- In MapReduce, we quantify the cost of an algorithm using
		- 1. Communication cost - total I/O 
		- Elapsed communication cost
		- Elapsed computation cost
	When you make MapReduce jobs in the real world, you really worry about the intermediate data size. You don’t want that to explode.

—


Spark is a fast cluster computing system, high level APIs in Java, Scala, Python, R. Built in Scala (and on a JVM).
	- can run as a standalone or on top of Hadoop YARN (Custer Manager), where it can read data directly from HDFS

Spark Core
Add-ons: Spark SQL, Spark Streaming, MLib, GraphX
	- Spark Streaming is not a real real time processing system like Apache Storm, instead based on micro-batching, but close enough for 95% scenarios.

MapReduce is Batch Processing
Apache Storm is Real-time Processing
Spark you can do both.

Spark Terms
	- RDD - Data lives here. RDD data is immutable.
	- DAG - When you run an app in Spark, it constructs a compute graph (DAG) which is the execution flow
	- SparkContext - You have a driver and the driver initiates the spark context and the spark context does a lot of the orchestration within the spark cluster
	- Transformations - When you load data from a data storage to an RDD, since RDDs are immutable, when you do a map or function operation it creates a new RDD, this is called a transformation.
	- Actions - Anything within Spark is done with lazy loading. When the DAG is created, no computation is performed until it is actually required. When it is necessary for the computation to be performed (it has not been calculated before and cached as a transformation), it is called an action.
	- The DAG is comprised of many, many transformations and a few actions.

Spark Components
	- Driver Program
		- Spark Context. The main program or client/terminal is the spark context
	- Cluster Manager (default is Spark manager, you can switch to Yarn or Apache Mesos to have a manager with special features, such as putting tasks into a queue)
	- Worker Nodes
		Worker Node
			- Cache
			- Executor
				- Task
				- Task
		Worker Node
			- Cache
			- Executor
				- Task
				- Task

Differences between Spark and Map-Reduce
	- it runs in-memory on the cluster, whereas MapReduce writes to disk after every computation
	- It follows DAG processing and doesn’t have to follow repeated map-reduce structure
	- Spark requires *a lot of memory* to function. If Spark runs on Hadoop YARN with other resource-demanding services, or if the data is too big to fit entirely into the memory, then there could be major performance degradations for Spark. Spark has the upper hand as long as we’re talking about iterative computations that need to pass over the same data many times. But when it comes to one-pass ETL-like jobs, for example, data transformation or [data integration](https://www.xplenty.com/signup/) , then MapReduce is the deal—this is what it was designed for. **Bottom line:**Spark performs better when all the data fits in the memory, especially on dedicated clusters; Hadoop MapReduce is designed for data that doesn’t fit in the memory and it can run well alongside other services.
	- In comparison to Hadoop’s MapReduce, Spark uses significantly more resources, which can interfere with other tasks that might be trying to use the cluster at the time. In essence, Spark might be a less considerate neighbor than other components that can operate on the Hadoop stack.

See also:
	- [Hadoop, Storm, Samza, Spark, and Flink: Big Data Frameworks Compared](bear://x-callback-url/open-note?id=CAB807C7-205C-4516-92F6-5FBF021DE6FA-10200-0000B138F7A869BF)