## Handy commands on _PostgreSQL_ for starters.
### psql
*	**none**  
 		connects to currently running PostgreSQL database with default host (localhost) and default port (5432)
*	**-U _username_**  
 		connects to currently running PostgreSQL database with default host (localhost) and default port (5432) with user name - _username_  

### pg_ctl
*	**start**  
                 start PostgreSQL database with default host (localhost) and default port (5432)
*	**-D DATA_DIRECTORY start**  
                 start PostgreSQL database with default host (localhost) and default port (5432); the data would be saved in DATA_DIRECTORY

### PostgreSQL Shell Commands
*	**create database dbname;**  
                 creates a database with dbname as the database name
*	**\q**  
                 Exit the PostgreSQL
*	**\l  or \list**  
                 List the databases
*	**\c  or \connect**  dbname  
                 connect to database with name - dbname
*	**\dt**  
                 List the tables in current database
*	**\d table_name**  
                 Describes table with name table_name
