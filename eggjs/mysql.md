# mysql

## Host '****' is not allowed to connect to this MySQL server

```sql
use mysql;

select user,host,plugin,authentication_string from user;

update user set host='****'where user='root';
```

## Mysql 解决1251- Client does not support authentication protocol requested by server...的问题

最新的mysql模块并未完全支持MySQL 8的“caching_sha2_password”加密方式，而“caching_sha2_password”在MySQL 8中是默认的加密方式。因此，下面的方式命令是默认已经使用了“caching_sha2_password”加密方式，该账号、密码无法在mysql模块中使用。

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY password;
```
