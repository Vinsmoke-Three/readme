## vinsmoke-three-wiki  

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
  ```
  Select TABSCHEMA, TABNAME, LOAD_STATUS from SYSIBMADM.ADMINTABINFO 
  where load_status = 'PENDING' and tabschema = '<schema_name>'"
  ```
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

### linux 命令
* 查看文件创建时间   
  `ll -u`
* 查看文件大小  
  `ll -h`  
* 查看文件内容(-N 显示行数)  
  `less -N test.txt`  
* 动态查看日志信息  
  `tail -f SystemOut.log`  
* 查找命令(less + grep)  
  `less test.txt | grep '关键字'`  

### shell 命令  
 ```
#date:20190101
DATESTR=`date +%Y%m%d`
TMPFILENAME='bounce550_tmp'$DATESTR'.txt'
#FILENAME1、FILENAME2はsendmagicの場合のみ
FILENAME1='bounce550_'$DATESTR'_1.csv'
FILENAME2='bounce550_'$DATESTR'_2.csv'
FILENAME='bounce550_'$DATESTR'.csv'
FORMAT=',tmp_addr@example.com'
## LOGからテキストに吐き出す
​
#grepでor関係の条件：grep -e '条件1' -e '条件2' 
find /usr/local/sendmagic/log/sm_unknown.log-$DATESTR -type f -print | xargs grep -e 'faild 42550' -e 'faild 42559' > /usr/local/batch/bin/$TMPFILENAME
cat /usr/local/batch/bin/$TMPFILENAME | awk -F"first" '{print $2}' | awk -F"end" '{print $1}' | sort -u | sed -e "s/\$/$FORMAT/" > /usr/local/batch/bin/$FILENAME1
cat /usr/local/batch/bin/$TMPFILENAME | awk -F"hello" '{print $2}' | awk -F"end" '{print $1}' | sort -u | sed -e "s/\$/$FORMAT/" > /usr/local/batch/bin/$FILENAME2
#重複している行を削除する
cat /usr/local/batch/bin/$FILENAME1 /usr/local/batch/bin/$FILENAME2 |  sort | uniq > /usr/local/batch/bin/$FILENAME
​
## 転送済みファイルを削除
rm $TMPFILENAME
rm $FILENAME1
rm $FILENAME2
rm $FILENAME
 ```