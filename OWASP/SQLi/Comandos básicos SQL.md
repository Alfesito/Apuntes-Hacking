----------
Tag: #sql #database #db

-----------
```mysql
show databases;
use <database>;
show tables;
describe <table>;
```

```mysql
select * from <table>;
select <column1>,<column2> from <table>;
select <column1>,<column2> from <table> where <column1>='root';
```

```mysql
create table users(id int(32), username varchar(32), password varchar(32));
insert into users(id, username, password) values(1, 'admin', 'admin123$!p@$$');
insert into users(id, username, password) values(2, 's4vitar', 's4vitarlammer123');
update users id=2 where username='s4vitar';
```

