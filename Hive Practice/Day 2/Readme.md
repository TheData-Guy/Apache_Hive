
## Day 2 Practice

### Creating the Database.

        create database hive_class_b1;

### Using The Created Database.

        use hive_class_b1;

### Creating Table Schema in the Database.

        create table department_data                                                                                                            
            > (                                                                                                                                       
            > dept_id int,                                                                                                                            
            > dept_name string,                                                                                                                       
            > manager_id int,                                                                                                                         
            > salary int)                                                                                                                             
            > row format delimited                                                                                                                    
            > fields terminated by ','; 

### To see the primary information of the Hive table such as only the list of columns and its data types.

            describe department_data;
            
### The complete table information is displayed in a clean manner by describe formatted command.

            describe formatted department_data;
            
### The describe extended command will show the detailed information of the table such as list of columns , data type of the columns,table type,location of the table,table size and so on.

            describe extended department_data;    
            
### Loading the Data From Local Path.

          load data local inpath 'file:///tmp/hive_class/depart_data.csv' into table department_data;
          
### Display column name.

        set hive.cli.print.header = true;

### Load data from HDFS location.

        load data inpath '/tmp/hive_data_class_2/' into table department_data_from_hdfs;


### Creating External table .

        create external table department_data_external                                                                                          
             (                                                                                                                                       
             dept_id int,                                                                                                                            
             dept_name string,                                                                                                                       
             manager_id int,                                                                                                                         
             salary int                                                                                                                              
             )                                                                                                                                       
             row format delimited                                                                                                                    
             fields terminated by ','                                                                                                                
             location '/tmp/hive_data_class_2/'; 
    
    
    
### Creating Table Schema For Array data types.

        create table employee                                                                                                                   
             (                                                                                                                                       
             id int,                                                                                                                                 
             name string,                                                                                                                            
             skills array<string>                                                                                                                    
             )                                                                                                                                       
             row format delimited                                                                                                                    
             fields terminated by ','                                                                                                                
             collection items terminated by ':';   
             
### Loading the Data From Local Path. 

            load data local inpath 'file:///tmp/hive_class/array_data.csv' into table employee; 


### Get Element by index in hive Array data type.

            select id, name, skills[0] as prime_skill from employee;
            
### Using Hive Built-in Function on Array Data Type.

        select                                                                                                                                  
             id,                                                                                                                                     
             name,                                                                                                                                   
             size(skills) as size_of_each_array,                                                                                                     
             array_contains(skills,"HADOOP") as knows_hadoop,                                                                                        
             sort_array(skills) as sorted_array                                                                                                                     
             from employee; 
    
    
### Creating Table Schema For Map data types.

        create table employee_map_data                                                                                                          
             (                                                                                                                                       
             id int,                                                                                                                                 
             name string,                                                                                                                            
             details map<string,string>                                                                                                              
             )                                                                                                                                       
             row format delimited                                                                                                                    
             fields terminated by ','                                                                                                                
             collection items terminated by '|'                                                                                                      
             map keys terminated by ':';
            
### Loading the Data From Local Path. 
 
            load data local inpath 'file:///tmp/hive_class/map_data.csv' into table employee_map_data;
            
### Get Value by Key in hive Map data type. 

         select                                                                                                                                  
             id,                                                                                                                                     
             name,                                                                                                                                   
             details["gender"] as employee_gender                                                                                                    
             from employee_map_data; 
 
 Using Hive Built-in Function on Map Data Type.
 
         select                                                                                                                                  
             id,                                                                                                                                     
             name,                                                                                                                                   
             details,                                                                                                                                
             size(details) as size_of_each_map,                                                                                                      
             map_keys(details) as distinct_map_keys,                                                                                                 
             map_values(details) as distinct_map_values                                                                                              
             from employee_map_data;
