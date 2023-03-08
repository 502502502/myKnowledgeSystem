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

### 10、子查询

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

### 11、连接

[题目](https://leetcode.cn/problems/customers-who-never-order/description/)

```sql
select Name as Customers
from Customers left join Orders
on Customers.Id = Orders.CustomerId
where Orders.Id is null;
```

### 12、条件

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

### 13、分组，组内计数

[题目·](https://leetcode.cn/problems/actors-and-directors-who-cooperated-at-least-three-times/description/)

```sql
select actor_id,director_id
from ActorDirector
group by actor_id,director_id
having count(*) >= 3
```

### 14、连接，分组，过滤分组，组内最值

[题目](https://leetcode.cn/problems/sales-analysis-iii/description/)

```sql
select Product.product_id, Product.product_name
from Product inner join Sales
on Product.product_id=Sales.product_id
group by product_id
having max(Sales.sale_date) <= '2019-03-31' and min(Sales.sale_date) >= '2019-01-01'
```

### 15、分组，过滤分组，组内去重

[题目](https://leetcode.cn/problems/user-activity-for-the-past-30-days-i/description/)

```sql
select activity_date as day, count(distinct user_id) as active_users
from Activity
group by activity_date
having activity_date <= '2019-07-27' and activity_date > '2019-06-27'
```

### 16、过滤，排序，去重

[题目](https://leetcode.cn/problems/article-views-i/description/)

```sql
select distinct author_id as id
from Views
where author_id=viewer_id
order by author_id  asc
```

### 17、连接，分组，排序，条件

[题目](https://leetcode.cn/problems/top-travellers/description/)

```sql
select u.name as name, ifnull(sum(r.distance),0) as travelled_distance
from Users as u left join Rides as r
on u.id=r.user_id
group by u.id
order by sum(r.distance) desc, u.name asc
```

### 18、分组，展示分组，分组内排序，分组内去重

[题目](https://leetcode.cn/problems/group-sold-products-by-the-date/description/)

```sql
select sell_date, count(distinct product) as num_sold, 
        group_concat(distinct product order by product asc) as products
from Activities
group by sell_date
```

### 19、模糊过滤

[题目](https://leetcode.cn/problems/patients-with-a-condition/description/)

```sql
select *
from Patients
where conditions like 'DIAB1%' or conditions like '% DIAB1%'
```

### 20、连接，过滤行，分组

[题目](https://leetcode.cn/problems/customer-who-visited-but-did-not-make-any-transactions/description/)

```sql
select v.customer_id as customer_id, count(*) as count_no_trans
from Visits as v left join Transactions as t
on v.visit_id=t.visit_id
where t.visit_id is null
group by v.customer_id
```

### 21、连接，分组，过滤分组，组内统计

[题目](https://leetcode.cn/problems/bank-account-summary-ii/description/)

```sql
select u.name as name,sum(amount) as balance
from Users as u join  Transactions as t
on u.account=t.account
group by u.account
having sum(amount) > 10000
```

### 22、字符操作函数，排序

[题目·](https://leetcode.cn/problems/fix-names-in-a-table/description/)

```sql
select user_id,concat(upper(left(name,1)), lower(right(name, length(name) -1))) as name
from Users
order by user_id asc
```

### 23、分组，组内统计

[题目](https://leetcode.cn/problems/daily-leads-and-partners/description/)

```sql
select date_id, make_name, count(distinct lead_id) unique_leads, 
    count(distinct partner_id) unique_partners
from DailySales
group by date_id, make_name
```

### 24、分组，组内统计，组间排序

[题目](https://leetcode.cn/problems/find-followers-count/description/)

```sql
select user_id, count(follower_id) followers_count
from Followers
group by user_id
order by user_id
```

### 25、分组，组内求和

[题目](https://leetcode.cn/problems/find-total-time-spent-by-each-employee/description/)

```sql
select event_day day, emp_id, sum(out_time -in_time) total_time
from Employees
group by event_day, emp_id
```

### 26、过滤

[题目](https://leetcode.cn/problems/recyclable-and-low-fat-products/description/)

```sql
select product_id
from Products
where low_fats='Y' and recyclable='Y'
```

### 27、行转列，列转行

[题目](https://leetcode.cn/problems/rearrange-products-table/description/)

```sql
select product_id, 'store1' store, store1 price
from Products
where store1 is not null
union
select product_id, 'store2' store, store2 price
from Products
where store2 is not null
union
select product_id, 'store3' store, store3 price
from Products
where store3 is not null

select product_id,
    sum(if(store='store1',price,null)) store1,
    sum(if(store='store2',price,null)) store2,
    sum(if(store='store3',price,null)) store3
from Products
group by product_id
```

### 28、对结果计算

[题目](https://leetcode.cn/problems/calculate-special-bonus/description/)

```sql
select employee_id,if(employee_id%2=1 and left(name,1) != 'M',salary,0) bonus
from Employees
order by employee_id asc
```

### 29、日期函数

[题目](https://leetcode.cn/problems/the-latest-login-in-2020/description/)

```sql
select user_id, max(time_stamp) last_stamp from Logins
where year(time_stamp)='2020'
group by user_id
```

### 30、合并，排序

[题目](https://leetcode.cn/problems/employees-with-missing-information/description/)

```sql
select * from(
    select  a.employee_id employee_id
    from Employees a left join Salaries s
    on a.employee_id=s.employee_id
    where s.employee_id is null
    union
    select  s.employee_id employee_id
    from Employees a right join Salaries s
    on a.employee_id=s.employee_id
    where a.employee_id is null
    ) t
order by employee_id;
```

### 31、前几

[题目](https://leetcode.cn/problems/second-highest-salary/description/)

[题目](https://leetcode.cn/problems/nth-highest-salary/description/)

```sql
select if(count(*)=2,min(salary),null) SecondHighestSalary
from(
    select distinct salary
    from Employee
    order by salary desc
    limit 2
) a


CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  RETURN (
      # Write your MySQL query statement below.
      select if(count(*)=N,min(salary),null)
      from(
          select distinct salary from Employee
          order by salary desc limit N
      )a
  );
END
```

### 32、排名问题

[题目](https://leetcode.cn/problems/rank-scores/description/)

```sql
select a.score score,
    (
        select count(distinct b.score) 
        from Scores b 
        where b.score >= a.score
        )`rank`
from Scores a
order by a.score desc
```

### 33、连续出现问题

连续出现的数字，id是连续的，将其分组后，组内编号和id编号的差一致

[题目](https://leetcode.cn/problems/consecutive-numbers/description/)

```sql
select distinct num ConsecutiveNums from(
    select id,num, 
            row_number() over(order by id)-
            row_number() over(partition by num order by id) subno
    from Logs
    )t
group by num, subno
having count(1) >= 3
```

### 34、子查询

[题目](https://leetcode.cn/problems/department-highest-salary/description/)

```sql
select d.name Department, e.name Employee, e.salary Salary
from Employee e join Department d
on e.departmentId=d.id
#各部门最大薪资者id的集合
where e.id in(
    select emp.id id from Employee emp
    #找这个部门的最大薪资，比较
    where emp.salary >= (
        select max(employ.salary) from Employee employ
        where emp.departmentId=employ.departmentId
    )
)
```

### 35、多层子查询，对查询结果处理

[题目](https://leetcode.cn/problems/tree-node/description/)

```sql
select id, if(p is null,'Root',if(snum=0,'Leaf','Inner')) Type
from(
    select tr.id id, p_id p, (select count(id) from tree t where t.p_id=tr.id)snum
    from tree tr
)a
```



#### 36、交换相邻位置

[题目](https://leetcode.cn/problems/exchange-seats/description/)

```sql
select if(id%2=0,id-1,
        if(id=(select max(id) from Seat),id,id+1)
        ) id,student 
from Seat
order by id
```



#### 37、连接过滤后的表，分组，对分组结果使用条件

[题目](https://leetcode.cn/problems/market-analysis-i/description/)

```sql
select u.user_id buyer_id,u.join_date,
        if(buyer_id is null,0,count(1)) orders_in_2019
from Users u left join (
    select buyer_id from Orders where year(order_date)='2019'
    ) o
on u.user_id=o.buyer_id
group by u.user_id
```



#### 38、对子查询结果进行分组，统计

[题目](https://leetcode.cn/problems/capital-gainloss/description/)

```sql
select stock_name,sum(price)capital_gain_loss
from(
    select stock_name,if(operation='Buy',-price,price) price
    from Stocks
)a
group by stock_name
```

