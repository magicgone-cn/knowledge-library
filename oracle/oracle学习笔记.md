# 登录

sqlplus /nolog

# 连接

conn / as sysdba

# 创建用户

create user root identified by a;

grant connect,resource,dba to root;