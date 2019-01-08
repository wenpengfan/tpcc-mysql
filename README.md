# tpcc-mysql
mysql基准测试

```
一、安装
cd tpcc-mysql/src

make

[root tpcc-mysql]$ ls
add_fkey_idx.sql  count.sql  create_table.sql  drop_cons.sql  load.sh  README  schema2  scripts  src  tpcc_load  tpcc_start

二、使用
tpcc-mysql的业务逻辑及其相关的几个表作用如下：
New-Order：新订单，一次完整的订单事务，几乎涉及到全部表
Payment：支付，主要对应 orders、history 表
Order-Status：订单状态，主要对应 orders、order_line 表
Delivery：发货，主要对应 order_line 表
Stock-Level：库存，主要对应 stock 表
其它表说明:
客户：主要对应 customer 表
地区：主要对应 district 表
商品：主要对应 item 表
仓库：主要对应 warehouse 表

三、TPCC测试前准备
初始化测试库环境，先创建一个测试库然后导入create_table.sql,顾名思义是创表的sql语句：

[root tpcc-mysql]$mysql -uroot -p123456 -S /data/mysql-5.5/mysql.sock -e "create database tpcctest"
[root tpcc-mysql]$ mysql -uroot -p123456 -S /data/mysql-5.5/mysql.sock tpcctest <./create_table.sql 
#[root tpcc-mysql]$ mysql -uroot -p123456 -S /data/mysql-5.5/mysql.sock tpcctest < add_fkey_idx.sql
[root tpcc-mysql]$ mysql -uroot -p123456 -S /data/mysql-5.5/mysql.sock -e "show tables from  tpcctest"

初始化完毕后，就可以开始加载测试数据了

tpcc_load用法如下：
执行下面的命令，开始灌入测试数据：（本人的是虚拟机，仓库数就弄了10,省得压死了，哈哈）
[root tpcc-mysql]$ ./tpcc_load 127.0.0.1:3308 tpcctest root 123456 10（仓库数）
造数据成功后，会提示：...DATA LOADING COMPLETED SUCCESSFULLY.

四、进行TPCC测试及结果解读

选项说明：
-w 指定仓库数量
-c 指定并发连接数
-r 指定开始测试前进行warmup的时间，进行预热后，测试效果更好
-l 指定测试持续时间
-i  指定生成报告间隔时长
-f 指定生成的报告文件名

[root tpcc-mysql]$ ./tpcc_start -h127.0.0.1 -P3308 -d tpcctest -u root -p 123456 -w 10 -c 10 -r 120 -l 120 
真实测试场景中，建议预热时间不小于5分钟，持续压测时长不小于30分钟，否则测试数据可能不具参考意义。

```
