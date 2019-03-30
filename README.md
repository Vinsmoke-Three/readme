### DB2 命令
* 查看执行计划 db2expln  
  ```
  db2 connect to db_name
  db2expln -d db_name -q “sql" -g -t
  ```
* 导出数据 db2 export  
  `db2 export to test.ixf of ixf "sql"`  
* 导出表定义 db2look  
  `db2look -d db_name -e -t table_name -m -x -o tablename.ddl`  
* 导入数据不产生日志文件，适合大量数据导入 db2 load  
  `db2 load from test.ixf of ixf replace into table_name nonrecoverable`  
* db2 load 完之后，检查数据完整性  
  `SET INTEGRITY FOR table_name ALL IMMEDIATE UNCHECKED`
* 删除表数据 truncate  
  `db2 "truncate table table_name immediate"`  
* reorg table  
  `db2 REORG TABLE table_name`  
* reorg index  
  `db2 REORG INDEXES ALL FOR TABLE tabele_name`  
* runstats  
  `db2 runstats on table table_name with distribution and detailed indexes all allow write access`  
* alter userspace  
  `db2 alter tablespace USERSPACE1 reduce max`  
* alter userindex  
  `db2 alter tablespace USERINDEX1 reduce max`  
* 确认表的状态  
  `db2 load query table tr_tname`
* 检查表的状态  
  `Select TABSCHEMA, TABNAME, LOAD_STATUS from SYSIBMADM.ADMINTABINFO where load_status = 'PENDING' and tabschema = '<schema_name>'"`  
* 解除一个表的check pendding状态  
  `db2 set constraints for tabname`  
* 解除一个表的挂起状态  
  `db2 reorg table  tabname`  
* 检查表是否需要reorg
  `REORGCHK CURRENT STATISTICS ON TABLE table_name`  
* 列出表空间  
  `db2 list tablespaces show detail`  
* 列出命令选项  
  `db2 list command options`