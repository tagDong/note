## Mac安装PostgreSQL
最近在学习rails，记录下安装psql的过程

### 安装及初始化
这里使用homebrew安装

```brew install postgresql```

等待安装完成后，初始化：

```initdb /usr/local/var/postgres```

启动服务：

```
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
brew services start postgresql // brew 启动
```

设置开机启动

```
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```


### 创建数据库和账户
mac安装postgresql后不会创建用户名数据库，执行命令：

`createdb`

然后登录PostgreSQL控制台：

`psql`

使用\l命令列出所有的数据库，看到已存在用户同名数据库、postgres数据库，但是postgres数据库的所有者是当前用户，没有postgres用户。按:q退出查看

之后需要做以下几件事：

1.创建postgres用户

` CREATE USER postgres WITH PASSWORD 'password';`

2.删除默认生成的postgres数据库

` DROP DATABASE postgres;`

3.创建属于postgres用户的postgres数据库

` CREATE DATABASE postgres OWNER postgres;`

4.将数据库所有权限赋予postgres用户

` GRANT ALL PRIVILEGES ON DATABASE postgres to postgres;`

5.给postgres用户添加创建数据库的属性

` ALTER ROLE postgres CREATEDB;`

这样就可以使用postgres作为数据库的登录用户了，并可以使用该用户管理数据库

### 登录控制台指令
`psql -U [user] -d [database] -h [host] -p [post]`

-U指定用户，-d指定数据库，-h指定服务器，-p指定端口

上方直接使用psql登录控制台，实际上使用的是缺省数据

```
user：当前mac用户
database：用户同名数据库
主机：localhost
端口号：5432，postgresql的默认端口是5432
```
完整的登录命令，比如使用postgres用户登录

`psql -U postgres -d postgres`

常用控制台命令
```
\password：设置当前登录用户的密码
\h：查看SQL命令的解释，比如\h select。
\?：查看psql命令列表。
\l：列出所有数据库。
\c [database_name]：连接其他数据库。
\d：列出当前数据库的所有表格。
\d [table_name]：列出某一张表格的结构。
\du：列出所有用户。
\e：打开文本编辑器。
\conninfo：列出当前数据库和连接的信息。
\password [user]: 修改用户密码
\q：退出
```

```
1. *创建数据库： create database [数据库名]; 
2. *查看数据库列表： \d 
3. *删除数据库： . drop database [数据库名]; 
创建表： create table ([字段名1] [类型1] <references 关联表名(关联的字段名)>;,[字段名2] [类型2],......<,primary key (字段名m,字段名n,...)>;); 
*查看表名列表： \d 
*查看某个表的状况： \d [表名] 
*重命名一个表： alter table [表名A] rename to [表名B]; 
*删除一个表： drop table [表名]; 

*在已有的表里添加字段： alter table [表名] add column [字段名] [类型]; 
*删除表中的字段： alter table [表名] drop column [字段名]; 
*重命名一个字段： alter table [表名] rename column [字段名A] to [字段名B]; 
*给一个字段设置缺省值： alter table [表名] alter column [字段名] set default [新的默认值]; 
*去除缺省值： alter table [表名] alter column [字段名] drop default; 
在表中插入数据： insert into 表名 ([字段名m],[字段名n],......) values ([列m的值],[列n的值],......); 
修改表中的某行某列的数据： update [表名] set [目标字段名]=[目标值] where [该行特征]; 
删除表中某行数据： delete from [表名] where [该行特征]; 
delete from [表名];--删空整个表
```

ANSI SQL 标准规定不加双引号的表名大小写不敏感，加双引号大小写敏感。