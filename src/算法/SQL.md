### 1、连接

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

### 2、连接

[题目](https://leetcode.cn/problems/employees-earning-more-than-their-managers/description/)

起别名

```sql
select A.name as Employee
from Employee as A, Employee as B
where A.managerId = B.Id
and A.salary > B.salary;
```

### 3、分组，计数

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

### 8、空值

[题目](https://leetcode.cn/problems/find-customer-referee/description/)

```sql
select name
from customer
where referee_id is null or referee_id != 2	
```

### 9、分组，排序

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

### 4、连接

[题目](https://leetcode.cn/problems/customers-who-never-order/description/)

```sql
select Name as Customers
from Customers left join Orders
on Customers.Id = Orders.CustomerId
where Orders.Id is null;
```

### 4、条件

[题目](https://leetcode.cn/problems/swap-salary/description/)

```sql
update Salary
set sex = 
if(sex='m','f','m')

update Salary
set sex = 
case sex
    when 'm' then 'f'
    else 'm'
end
```

### 4、分组，组内计数

[题目·](https://leetcode.cn/problems/actors-and-directors-who-cooperated-at-least-three-times/description/)

```sql
select actor_id,director_id
from ActorDirector
group by actor_id,director_id
having count(*) >= 3
```

### 4、连接，分组，过滤分组，组内最值

[题目](https://leetcode.cn/problems/sales-analysis-iii/description/)

```sql
select Product.product_id, Product.product_name
from Product inner join Sales
on Product.product_id=Sales.product_id
group by product_id
having max(Sales.sale_date) <= '2019-03-31' and min(Sales.sale_date) >= '2019-01-01'
```

### 4、分组，过滤分组，组内去重

[题目](https://leetcode.cn/problems/user-activity-for-the-past-30-days-i/description/)

```sql
select activity_date as day, count(distinct user_id) as active_users
from Activity
group by activity_date
having activity_date <= '2019-07-27' and activity_date > '2019-06-27'
```

### 4、过滤，排序，去重

[题目](https://leetcode.cn/problems/article-views-i/description/)

```sql
select distinct author_id as id
from Views
where author_id=viewer_id
order by author_id  asc
```

### 4、连接，分组，排序，条件

[题目](https://leetcode.cn/problems/top-travellers/description/)

```sql
select u.name as name, ifnull(sum(r.distance),0) as travelled_distance
from Users as u left join Rides as r
on u.id=r.user_id
group by u.id
order by sum(r.distance) desc, u.name asc
```

### 4、分组，展示分组，分组内排序，分组内去重

[题目](https://leetcode.cn/problems/group-sold-products-by-the-date/description/)

```sql
select sell_date, count(distinct product) as num_sold, 
        group_concat(distinct product order by product asc) as products
from Activities
group by sell_date
```

### 4、模糊过滤

[题目](https://leetcode.cn/problems/patients-with-a-condition/description/)

```sql
select *
from Patients
where conditions like 'DIAB1%' or conditions like '% DIAB1%'
```

### 4、连接，过滤行，分组

[题目](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/description/)

```sql
select v.customer_id as customer_id, count(*) as count_no_trans
from Visits as v left join Transactions as t
on v.visit_id=t.visit_id
where t.visit_id is null
group by v.customer_id
```

### 4、连接，分组，过滤分组，组内统计

[题目](https://leetcode.cn/problems/bank-account-summary-ii/description/)

```sql
select u.name as name,sum(amount) as balance
from Users as u join  Transactions as t
on u.account=t.account
group by u.account
having sum(amount) > 10000
```

### 4、字符操作函数，排序

[题目·](https://leetcode.cn/problems/fix-names-in-a-table/description/)

```sql
select user_id,concat(upper(left(name,1)), lower(right(name, length(name) -1))) as name
from Users
order by user_id asc
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

