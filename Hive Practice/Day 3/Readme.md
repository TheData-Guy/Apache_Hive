# Day 3 

## Creating the Table Schema for Loading rthe Data in CSV Format.

                 create table sales_data_v2                                                                                                              
                    (                                                                                                                                       
                     p_type string,                                                                                                                          
                     total_sales int                                                                                                                         
                     )                                                                                                                                       
                     row format delimited                                                                                                                    
                     fields terminated by ','; 

 ##  Loading the Data from Local Path in the Table
 
    load data local inpath 'file:///tmp/hive_class/sales_data_raw.csv' into table sales_data_v2; 

## Creating the Copy OR Backup of One Table From Anothe Table.

    create table sales_data_v2_bkup as select * from sales_data_v2;

## The describe extended command will show the detailed information of the table such as list of columns , data type of the columns,table type,location of the table,table size and so on.

    describe extended sales_data_v2;


## Create a Table Schema which will store data in Parquet Format.

    create table sales_data_pq_final                                                                                                        
         (                                                                                                                                       
         product_type string,                                                                                                                    
         total_sales int                                                                                                                         
         )                                                                                                                                       
         stored as parquet;  

## Loading data in parquet file from Another Table.

    from sales_data_v2 insert overwrite table sales_data_pq_final select *;

