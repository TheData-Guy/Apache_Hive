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

- Thrift Clients: The Hive server can handle requests from a thrift client by using Apache Thrift.
- JDBC client: A JDBC driver connects to Hive using the Thrift framework. Hive Server communicates with the Java applications using the JDBC driver.
- ODBC client: The Hive ODBC driver is similar to the JDBC driver in that it uses Thrift to connect to Hive. However, the ODBC driver uses the Hive Server to communicate with it instead of the Hive Server.
