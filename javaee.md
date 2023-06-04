1、数据表

user

+ id
+ name
+ password
+ phone



message

+ id
+ from_id
+ con_id
+ content
+ time



comment

+ id
+ user_id
+ con_id
+ content



artemis create D:\JAVA\activemq\apache-artemis-2.10.0\bin\myartemis



使用jsp代码帮我写一个session-query.jsp，还有相应css样式，需要和index.jsp样式适配，我需要实现以下功能：

1.标题"所有会话"

2.来到revise.jsp时本身会带有一个user_id属性，提交表单时，我需要默认把这个属性传到后端。

3.来到revise.jsp时本身会带有一个sessions属性，是当前分页的session对象的集合，session对象包括id、user_id,comment，time这四个属性。

3.来到revise.jsp时本身会带有一个page属性，用于实现分页，里面保存了sesseion的总数信息，当前页码，每页的session数量，当前访问路径等。

4.居中显示一个列表，展示sessions集合里的所有元素，每个元素需要展示sesseion的id,comment,time。点击元素向后端servlet发送一个get请求，携带当前session的id，还有这个页面带有的user_id。

5.列表下边有一个分页的导航栏，显示当前页码，以及页面总数，提供上一页，下一页，首页和尾页的选项，分页逻辑通过page类属性完成，在请求路径后面带一个参数表示当前查询的是那个个页码的数据。

5.提供一个首页按钮，首页返回main.jsp





