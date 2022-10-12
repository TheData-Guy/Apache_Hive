create database hive_class_b1;
use hive_class_b1;

create table department_data                                                                                                            
    > (                                                                                                                                       
    > dept_id int,                                                                                                                            
    > dept_name string,                                                                                                                       
    > manager_id int,                                                                                                                         
    > salary int)                                                                                                                             
    > row format delimited                                                                                                                    
    > fields terminated by ','; 
    
describe department_data;

describe formatted department_data;

# For data load from local
load data local inpath 'file:///tmp/hive_class/depart_data.csv' into table department_data;
