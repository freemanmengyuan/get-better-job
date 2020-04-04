##### 事务的隔离级别
```
ANSI/ISO SQL标准定义了4中事务隔离级别：
未提交读（read uncommitted）
    未提交读的意思就是比如原先name的值是小刚，然后有一个事务B`update table set name = '小明' where id = 1`,它还没提交事务。同时事务A也起了，有一个select语句`select name from table where id = 1`，在这个隔离级别下获取到的name的值是小明而不是小刚。那万一事务B回滚了，实际数据库中的名字还是小刚，事务A却返回了一个小明，这就称之为脏读
已提交读（read committed）
    按照上面那个例子，在已提交读的情况下，事务A的select name 的结果是小刚，而不是小明，因为在这个隔离级别下，一个事务只能读到另一个事务修改的已经提交了事务的数据。但是有个现象，还是拿上面的例子说。如果事务B 在这时候隐式提交了时候，然后事务A的select name结果就是小明了，这都没问题，但是事务A还没结束，这时候事务B又`update table set name = '小红' where id = 1`并且隐式提交了。然后事务A又执行了一次`select name from table where id = 1`结果就返回了小红。这种现象叫幻读。
可重复读（repeatable read）
    可重复读就是一个事务只能读到另一个事务修改的已提交了事务的数据，但是第一次读取的数据，即使别的事务修改的这个值，这个事务再读取这条数据的时候还是和第一次获取的一样，不会随着别的事务的修改而改
串行化（serializable）
对于不同的事务，采用不同的隔离级别分别有不同的结果。不同的隔离级别有不同的现象。
mysql 默认的事务隔离级别是 repeatable read
        SELECT @@GLOBAL.tx_isolation, @@tx_isolation;  //查看数据库的默认隔离级别
SET TRANSACTION ISOLATION LEVEL
```