# Hive Day 4

## Using  this command to get details about Serilization and Deserialization

        describe formatted sales_data_pq_final;
        
            SerDe Library: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
            InputFormat:   org.apache.hadoop.mapred.TextInputFormat
            OutputFormat:  org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat    


## Creating Table Schema to Stor CSV File Data With Specific Properties.

                create table csv_table                                                                                                                  
                     (                                                                                                                                       
                     name string,                                                                                                                            
                     location string                                                                                                                         
                     )                                                                                                                                       
                     row format serde 'org.apache.hadoop.hive.serde2.OpenCSVSerde'                                                                           
                     with serdeproperties (                                                                                                                  
                      "separatorChar" = ",",                                                                                                                 
                      "quoteChar" = "\"",                                                                                                                    
                      "escapeChar" = "\\"                                                                                                                    
                     )                                                                                                                                       
                     stored as textfile                                                                                                                      
                     tblproperties ("skip.header.line.count" = "1"); 
    
## Loading the Data from the Local File System

        load data local inpath 'file:///tmp/hive_class/csv_file.csv' into table csv_table;
    

# Download hive catalog jar file , if serde libraries are not imported

        https://repo1.maven.org/maven2/org/apache/hive/hcatalog/hive-hcatalog-core/0.14.0/

# Adding jar file into your hive shell

         add jar /tmp/hive_class/hive-hcatalog-core-0.14.0.jar;


## Creating Json Table Schema 

        create table json_table                                                                                                                 
             ( name string,                                                                                                                          
             id int,                                                                                                                                 
             skills array<string>                                                                                                                    
             )                                                                                                                                       
             row format serde 'org.apache.hive.hcatalog.data.JsonSerDe'                                                                              
             stored as textfile; 
    
## Loading the Data in Json Table from Local File System.

        load data local inpath 'file:///tmp/hive_class/json_file.json' into table json_table;


## Creating The Sales Table Schema.

                create table sales_order_data_csv_v1
                (
                ORDERNUMBER int,
                QUANTITYORDERED int,
                PRICEEACH float,
                ORDERLINENUMBER int,
                SALES float,
                STATUS string,
                QTR_ID int,
                MONTH_ID int,
                YEAR_ID int,
                PRODUCTLINE string,
                MSRP int,
                PRODUCTCODE string,
                PHONE string,
                CITY string,
                STATE string,
                POSTALCODE string,
                COUNTRY string,
                TERRITORY string,
                CONTACTLASTNAME string,
                CONTACTFIRSTNAME string,
                DEALSIZE string
                )
                row format delimited
                fields terminated by ','
                tblproperties("skip.header.line.count"="1")
                ; 

## Then Loading the Data from the Local File System.

        load data local inpath 'file:///tmp/hive_class/sales_order_data.csv' into table sales_order_data_csv_v1;
        
## Creating the Table Schema for Sales Order Data To Stored in ORC Format.        
                create table sales_order_data_orc
                (
                ORDERNUMBER int,
                QUANTITYORDERED int,
                PRICEEACH float,
                ORDERLINENUMBER int,
                SALES float,
                STATUS string,
                QTR_ID int,
                MONTH_ID int,
                YEAR_ID int,
                PRODUCTLINE string,
                MSRP int,
                PRODUCTCODE string,
                PHONE string,
                CITY string,
                STATE string,
                POSTALCODE string,
                COUNTRY string,
                TERRITORY string,
                CONTACTLASTNAME string,
                CONTACTFIRSTNAME string,
                DEALSIZE string
                )
                stored as orc;

## Now Copy the Data from Sales_order_data_csv_v1 to Sales_Order_data_orc.

        from sales__order_data_csv_v1 insert overwrite table sales_order_data_orc select *;
        
## Performing Some Operation on Sales_Order_Data_Orc to Understand How Number of Mapper and Reducer Work.

        select year_id, sum(sales) as total_sales from sales_order_data_orc group by year_id; 

## here only 1 map and 1 reduce task will get created

        In order to change the average load for a reducer (in bytes):                                                                                 
          set hive.exec.reducers.bytes.per.reducer=<number>                                                                                           
        In order to limit the maximum number of reducers:                                                                                             
          set hive.exec.reducers.max=<number>                                                                                                         
        In order to set a constant number of reducers:                                                                                                
          set mapreduce.job.reduces=<number> 
  
## change number of reducers to 3
 
 set mapreduce.job.reduces=3;
 create table sales_order_grouped_orc_v1 stored as orc as select year_id, sum(sales) as total_sales from sales_order_data_orc group by ye
ar_id;

# after creating the table, check the number of files in hdfs location

# change number of reducers to 2
 
 set mapreduce.job.reduces=2;
 create table sales_order_grouped_orc_v2 stored as orc as select year_id, sum(sales) as total_sales from sales_order_data_orc group by ye
ar_id;

# after creating the table, check the number of files in hdfs location


# set this property if doing static partition
set hive.mapred.mode=strict;

# create table command for partition tables - for Static

create table sales_data_static_part                                                                                                     
    > (                                                                                                                                       
    > ORDERNUMBER int,                                                                                                                        
    > QUANTITYORDERED int,                                                                                                                    
    > SALES float,                                                                                                                            
    > YEAR_ID int                                                                                                                             
    > )                                                                                                                                       
    > partitioned by (COUNTRY string); 
    
# load data in static partition

insert overwrite table sales_data_static_part partition(country = 'USA') select ordernumber,quantityordered,sales,year_id from sales_ord
er_data_orc where country = 'USA';

# set this property for dynamic partioning
set hive.exec.dynamic.partition.mode=nonstrict;   


hive> create table sales_data_dynamic_part                                                                                                    
    > (
    > ORDERNUMBER int,                                                                                                                        
    > QUANTITYORDERED int,                                                                                                                    
    > SALES float,                                                                                                                            
    > YEAR_ID int                                                                                                                             
    > )
    > partitioned by (COUNTRY string); 

# load data in dynamic partition table

insert overwrite table sales_data_dynamic_part partition(country) select ordernumber,quantityordered,sales,year_id,country from sales_or
der_data_orc;
  

# multilevel partition

create table sales_data_dynamic_multilevel_part_v1                                                                                      
    > (
    > ORDERNUMBER int,                                                                                                                        
    > QUANTITYORDERED int,                                                                                                                    
    > SALES float                                                                                                                             
    > )
    > partitioned by (COUNTRY string, YEAR_ID int); 
    
# load data in multilevel partitions

insert overwrite table sales_data_dynamic_multilevel_part_v1 partition(country,year_id) select ordernumber,quantityordered,sales,country
,year_id from sales_order_data_orc;
