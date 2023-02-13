add file /tmp/hive_class/multiply_udf.py;


# statement to execute python udf

select transform(year_id,quantityordered) using 'python multicol_python_udf.py' as (year_id string, square int) from sales_order_data_orc
 limit 5;


# add udf in hive shell

add file /tmp/hive_class/many_column_udf.py;


# statement to execute python udf for multiple columns

hive> select transform(country,ordernumber,quantityordered)                                                                                   
    > using 'python many_column_udf.py' as (country string, ordernumber int, multiplied_quantity int)                                         
    > from sales_order_data_orc limit 10;
    
    
# use below hive statements for bucketing

hive> create table users                                                                                                                      
    > (                                                                                                                                       
    > id int,                                                                                                                                 
    > name string,                                                                                                                            
    > salary int,                                                                                                                             
    > unit string                                                                                                                             
    > )row format delimited                                                                                                                   
    > fields terminated by ','; 
  
load data local inpath 'file:///tmp/hive_class/users.csv' into table users;
    
hive> create table locations                                                                                                                  
    > (                                                                                                                                       
    > id int,                                                                                                                                 
    > location string                                                                                                                         
    > )                                                                                                                                       
    > row format delimited                                                                                                                    
    > fields terminated by ','; 
    
load data local inpath 'file:///tmp/hive_class/locations.csv' into table locations; 

set hive.enforce.bucketing=true;
    
    
 hive> create table buck_users                                                                                                                 
    > (                                                                                                                                       
    > id int,                                                                                                                                 
    > name string,                                                                                                                            
    > salary int,                                                                                                                             
    > unit string                                                                                                                             
    > )                                                                                                                                       
    > clustered by (id)                                                                                                                       
    > sorted by (id)                                                                                                                          
    > into 2 buckets;
    
insert overwrite table buck_users select * from users;
    
hive> create table buck_locations                                                                                                             
    > (                                                                                                                                       
    > id int,                                                                                                                                 
    > location string                                                                                                                         
    > )                                                                                                                                       
    > clustered by (id)                                                                                                                       
    > sorted by (id)                                                                                                                          
    > into 2 buckets; 
    
 insert overwrite table buck_locations select * from locations;
 
 
 
 
 ------------------------------------------------------------------------------------------------------------------------------
Reduce-Side Join
------------------------------------------------------------------------------------------------------------------------------

SET hive.auto.convert.join=false;
SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;

Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1

------------------------------------------------------------------------------------------------------------------------------
Map Side Join
------------------------------------------------------------------------------------------------------------------------------

SET hive.auto.convert.join=true;
SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;

Mapred Local Task Succeeded . Convert the Join into MapJoin
Number of reduce tasks is set to 0 since there's no reduce operator
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 0

------------------------------------------------------------------------------------------------------------------------------
Bucket Map Join
------------------------------------------------------------------------------------------------------------------------------
set hive.optimize.bucketmapjoin=true;
SET hive.auto.convert.join=true;

SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;

------------------------------------------------------------------------------------------------------------------------------
Sorted Merge Bucket Map Join
------------------------------------------------------------------------------------------------------------------------------
set hive.enforce.sortmergebucketmapjoin=false;
set hive.auto.convert.sortmerge.join=true;
set hive.optimize.bucketmapjoin = true;
set hive.optimize.bucketmapjoin.sortedmerge = true;


SET hive.auto.convert.join=false;
SELECT * FROM buck_users u INNER JOIN buck_locations l ON u.id = l.id;

No MapLocal Task to create hash table.
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 0
 
