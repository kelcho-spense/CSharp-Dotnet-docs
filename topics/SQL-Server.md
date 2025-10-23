# SQL Server

Visit https://www.microsoft.com/en-us/sql-server/sql-server-downloads

and download 
![image_71.png](image_71.png)

click custom

![image_72.png](image_72.png)

click install 

![image_73.png](image_73.png)

click new sql server standalone installation

![image_74.png](image_74.png)

after installation open Sql server Configuration Manager and go to SQL Server Network Configuration, under protocols

![image_75.png](image_75.png)

right click and go to properties to enable,

![image_76.png](image_76.png)

switch to Ip address tab and enable ip1 
![image_77.png](image_77.png)

and ip12 (127.0.0.1 == localhost)
![image_78.png](image_78.png)

 click window key and search for services, then scroll down to a service called SQL Server, we need to restart it to make sure the changers we made take effect. Right click and press restart
![image_79.png](image_79.png)

