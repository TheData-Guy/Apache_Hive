# Day 3 

## Creating the Table Schema for Loading rthe Data in CSV Format.

                 create table sales_data_v2                                                                                                              
                    > (                                                                                                                                       
                    > p_type string,                                                                                                                          
                    > total_sales int                                                                                                                         
                    > )                                                                                                                                       
                    > row format delimited                                                                                                                    
                    > fields terminated by ','; 

 ##  Loading the Data from Local Path in the Table
 
    load data local inpath 'file:///tmp/hive_class/sales_data_raw.csv' into table sales_data_v2; 

## command to create identical table
create table sales_data_v2_bkup as select * from sales_data_v2;

## describe command for a table
describe extended sales_data_v2;


## create a table which will store data in parquet

create table sales_data_pq_final                                                                                                        
    > (                                                                                                                                       
    > product_type string,                                                                                                                    
    > total_sales int                                                                                                                         
    > )                                                                                                                                       
    > stored as parquet;  
    
## load data in parquet file
from sales_data_v2 insert overwrite table sales_data_pq_final select *;

