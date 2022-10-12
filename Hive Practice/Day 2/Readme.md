
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
            
### For data load from local

          load data local inpath 'file:///tmp/hive_class/depart_data.csv' into table department_data;
