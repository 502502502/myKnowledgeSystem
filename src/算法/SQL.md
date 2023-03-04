### 1、组合表

[题目](https://leetcode.cn/problems/combine-two-tables/description/)

inner join：2表值都存在

outer join：附表中值可能存在null的情况。

总结：

　　①A inner join B：取交集

　　②A left join B：取A全部，B没有对应的值，则为null

　　③A right join B：取B全部，A没有对应的值，则为null

　　④A full outer join B：取并集，彼此没有对应的值为null

```sql
select LastName, FirstName, City, State
from Person left join Address
on Person.PersonId = Address.PersonId;
```

### 2、组合表

[题目](https://leetcode.cn/problems/employees-earning-more-than-their-managers/description/)

起别名

```sql
select A.name as Employee
from Employee as A, Employee as B
where A.managerId = B.Id
and A.salary > B.salary;
```

### 3、分组查询数量

[题目](https://leetcode.cn/problems/duplicate-emails/description/)

```sql
select email
from Person
group by email
having count(email) > 1;
```

### 4、子查询

[题目](https://leetcode.cn/problems/customers-who-never-order/description/)

```sql
select Name as Customers
from Customers
where Id not in(
    select CustomerId
    from Orders
)
```

### 5、删除

[题目](https://leetcode.cn/problems/delete-duplicate-emails/description/)

```sql
delete from Person
where id not in(
    select id from(
        select min(id) as id
        from Person
        group by email
    )a
)
```

### 6、日期类型

[题目](https://leetcode.cn/problems/rising-temperature/description/)

```sql
select a.id as id 
from Weather a , Weather b
where datediff(a.recordDate, b.recordDate) = 1 and a.Temperature > b.Temperature;
```

### 7、日期类型

[题目](https://leetcode.cn/problems/game-play-analysis-i/description/)

```sql
select player_id, min(event_date) as first_login
from Activity
group by player_id
```

### 8、空值判断

[题目](https://leetcode.cn/problems/find-customer-referee/description/)

```sql
select name
from customer
where referee_id is null or referee_id != 2	
```

### 9、分组排序

[题目](https://leetcode.cn/problems/customer-placing-the-largest-number-of-orders/description/)

```sql
select customer_number
from Orders
group by customer_number
order by count(order_number) desc
limit 1
```

### 4、子查询

[题目](https://leetcode.cn/problems/sales-person/description/)

```sql
select name from SalesPerson 
where sales_id not in(
    select sales_id from Orders
    where com_id in(
        select com_id from Company where name ='RED'
    )
)
```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

### 4、



```sql

```

