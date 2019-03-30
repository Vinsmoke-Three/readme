### DB2 命令
* 查看执行计划  
  ```
  db2 connect to [db_name]
  db2expln -d [db_name] -q “sql文" -g -t
  ```
