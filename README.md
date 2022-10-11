![](/images/hive.png)

# Apache Hive

**Apache Hive** is an open source data warehouse system built on top of Hadoop used for querying and analyzing large datasets stored in Hadoop files. It process structured and semi-structured data in Hadoop.

## Hive History 
 
Data Infrastructure Team at Facebook developed Hive. Apache Hive is also one of the technologies that are being used to address the requirements at Facebook. It is very popular with all the users internally at Facebook.

## Hive Architecture

![](/images/hive2.png)

### Hive Client

Hive drivers support applications written in any language like Python, Java, C++, and Ruby, among others, using JDBC, ODBC, and Thrift drivers, to perform queries on the Hive. Therefore, one may design a hive client in any language of their choice.

The three types of Hive clients are referred to as Hive clients:

- **Thrift Clients:** The Hive server can handle requests from a thrift client by using Apache Thrift.
- **JDBC client:** A JDBC driver connects to Hive using the Thrift framework. Hive Server communicates with the Java applications using the JDBC driver.
- **ODBC client:** The Hive ODBC driver is similar to the JDBC driver in that it uses Thrift to connect to Hive. However, the ODBC driver uses the Hive Server to communicate with it instead of the Hive Server.

### Hive Services 

Hive provides numerous services, including the Hive server2, Beeline, etc. The services offered by Hive are:

- Beeline: HiveServer2 supports the Beeline, a command shell that which the user can submit commands and queries to. It is a JDBC client that utilises SQLLINE CLI (a pure Java console utility for connecting with relational databases and executing SQL queries). The Beeline is based on JDBC.
- Hive Server 2: HiveServer2 is the successor to HiveServer1. It provides clients with the ability to execute queries against the Hive. Multiple clients may submit queries to Hive and obtain the desired results. Open API clients such as JDBC and ODBC are supported by HiveServer2

**Note**

- Hive server1, which is also known as a Thrift server, is used to communicate with Hive across platforms. Different client applications can submit requests to Hive and receive the results using this server.
- HiveServer2 handled concurrent requests from more than one client, so it was replaced by HiveServer1.

### Hive Driver

The Hive driver receives the HiveQL statements submitted by the user through the command shell and creates session handles for the query.

- It Acts Like a Controller For HQL Statements.
- It Create a Session for Query.
- It Maintain a Life Cycle of HQL
- It Maintain MetaData For Execution.
- It Collects Output and Display.

### Hive Compiler 

It performs the compilation of the HiveQL query. This converts the query to an execution plan. The plan contains the tasks. It also contains steps needed to be performed by the MapReduce to get the output as translated by the query. The compiler in Hive converts the query to an Abstract Syntax Tree (AST). First, check for compatibility and compile-time errors, then converts the AST to a Directed Acyclic Graph (DAG).

### Hive Optimizer :

It performs various transformations on the execution plan to provide optimized DAG. It aggregates the transformations together, such as converting a pipeline of joins to a single join, for better performance. The optimizer can also split the tasks, such as applying a transformation on data before a reduce operation, to provide better performance.

### Hive Executor 

After the compilation and optimization steps, the execution engine uses Hadoop to execute the prepared execution plan, which is dependent on the compiler’s execution plan.

### Hive Metastore 

- Metastore stores metadata information about tables and partitions, including column and column type information, in order to improve search engine indexing.
- The metastore also stores information about the serializer and deserializer as well as HDFS files where data is stored and provides data storage. It is usually a relational database. Hive metadata can be queried and modified through Metastore.
- Internally Hive Use Derby Database for Metastore


## Feature of Hive 

- Hive supports external tables which make it possible to process data without actually storing in HDFS.
- Apache Hive fits the low-level interface requirement of Hadoop perfectly.
- It also supports partitioning of data at the level of tables to improve performance.
- Hive has a rule based optimizer for optimizing logical plans.
- It is scalable, familiar, and extensible.
- Using HiveQL doesn’t require any knowledge of programming language, Knowledge of basic SQL query is enough.
- We can easily process structured data in Hadoop using Hive.
- Querying in Hive is very simple as it is similar to SQL.
- We can also run Ad-hoc queries for the data analysis using Hive.
- Hive provides data summarization, query, and analysis in much easier manner.

## Limitation of Apache Hive 

- Apache does not offer real-time queries and row level updates.
- Hive also provides acceptable latency for interactive data browsing.
- It is not good for online transaction processing ( OLTP ).
- Latency for Apache Hive queries is generally very high.
Not used for row-level updates for real-time systems.
