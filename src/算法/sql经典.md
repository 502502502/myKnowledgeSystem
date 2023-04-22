#### 1、日期

+ datediff(a,b)：返回a比b大的**天数**
+ min(日期字段)：返回分组或整个表中该日期字段的最小日期
+ max(日期字段)：返回分组或整个表中该日期字段的最大日期
+ year
+ month
+ quarter
+ adddate(date, interval expr type)
+ subdate(date, interval expr type)
+ date_format('2021-03-27 15:21:00','%Y-%m-%d');
+ curdate()
+ curtime()
+ now()
+ 

#### 2、分组

+ where：在分组前对数据进行过滤，挑选满足条件的数据进行分组

+ group by （字段1，字段2...）：按照字段值组合进行分组

+ having：将分组进行条件过滤，挑选满足条件的分组

+ order by：将分组进行排序

+ limit：将分组的结果数量进行限制

+ count：对分组内元素数量进行统计

+ distinct：去重

+  group_concat(字段1，字段2...  过滤，排序等)：展示组内信息

  





#### 3、连接

+ a  inner join  b：交集
+ a  left join  b：a全部，b没有为nulll
+ a  right join  b：b全部，a没有为null
+ a，b：a，b全部，没有为null
+ on   =  ：连接条件





#### 4、子查询

+ from(子查询表)a：查询子查询的结果表，需要起别名
+  a = (子查询值)：子查询为确定值，可以作为比较
+  in() / notin() ：子查询为单列元素，可作为范围



#### 5、表达式

+ if(a,b,c)：如果a成立，值为b，否则为c
+ Ccase when expr1 then expr2



#### 6、字符操作

+ concat(s，t)：拼接
+ concat_ws(sep, s,t)：连接，加入分隔符
+ left(s，k)：左截取k个字符
+ right(s，k)：右截取k个字符
+ lower(s)：全部小写
+ upper(s)：全部大写
+ replace(s,old,new)：用new替换s中的old
+ ltrim(s)/rtrim(s)/trim(s)：去除两边空格
+ substring(s, st, len)：截取



#### 7、模糊查询

+ like '%'：%代表0个或多个字符



#### 8、数值操作

+ format(k, t)：将k保留t位小数
+ sum(): 求和，里面可以使用表达式，加上判断，过滤
+ ceiling(x)
+ round(x,y)：四舍五入x，保留y位
+ sqrt(x)：平方根
+ abs(x)：绝对值
+ log(x)：求对数
+ mod(x,y)：x对y取余



#### 9、行列变换

+ union合并实现行变成列



#### 10、创建表

```sql
create table if not exist 'tablename'(
	'k1' type 约束
    'k2' type 约束
    'k3' type 约束
    primary key(columnName)
    index indexName(columnName(length))
)engine=innodb default charset=utf8
```



#### 11、创建索引

```sql
create index index_name on tableName(columnName(length))
alter table tableName add index indexName on (columnName(length))
drop index indexName on tableName
```



#### 12、修改数据

+ update  table  set   字段名=(表达式)



#### 13、删除数据

+ delete from tableName where：一行一行删除数据，不释放表空间，不删除表结构，可回滚
+ drop table tableName：删除所有数据，释放表空间，删除表结构
+ truncate table tableName：删除所有数据，释放表空间，保留表结构



#### 14、插入数据

+ insert into tableName(k1,k2,k3) values(v1,v2,v3)



#### 15、常用函数

+ isnull(字段)：判断当前字段是否为空
+ k  is null / k is not null：判断是否为空
+ count()：有分组，组内统计，没有分组全表统计
+ distinct：有分组，组内去重，没有分组全表去重
+ order by：有分组，分组排序，没有分组全表排序
+ limit：有分组，组内限制数量，没有分组，全表限制数量
+ union：将查询结果合并在一张表，只需在第一个表声明字段名称，字段匹配不对会报错；可以使用分组、排序等对集合进行再处理





#### 16、排名问题

+ 通过**子查询**查找比当前大或者小的**记录的数量**，有必要时可以去重



#### 17、连续出现问题

+ row_number() over(partition by num order by Id)：为记录按顺序编号，可以分组，在组内编号
+ 连续出现，说明全表编号和组内编号的差值保持一定



#### 18、前几问题

+ 使用order by 和limit得到前几
+ 再对前几的集合使用min()得到第几
+ 注意使用distinct去重

 

#### 19、存储过程

```sql
create function functionName(paramName type) returns type 
begine
	return(
        #sql
    );
end
```

