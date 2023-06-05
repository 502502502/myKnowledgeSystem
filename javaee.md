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



使用jsp代码帮我写一个ai.jsp，还有相应css样式，需要和index.jsp样式适配，我需要实现以下功能：

1.标题"AI_对话"

2.来到ai.jsp时本身会带有一个user_id属性，向后端发送请求时，需要默认把这个属性传到后端。

3.message对象是消息对象，message包含id,from_id,cotent,time属性，其中cotent是消息的的内容，time是消息发送的时间，from_id是消息的发送者，如果from_id是0，代表这个消息是AI对象发送的。

4.居中显示一个聊天对话框的样式，展示聊天发送的message，聊天消息需要展示cotent，在cotent上边用小字显示发送时间time。当前用户发送的消息显示在对话框右边，AI对象回复的消息显示在左边。聊天对话框的大小需要合适并且固定，如果message太多，则提供一个滚动条进行查看。

5.聊天对话框上边右侧提供一个结束对话按钮，向后端发送post请求，携带参数"user_id"

6.聊天对话框下边显示一个发送消息的输入框，输入框右边提供一个发送消息的按钮，点击后通过javascript代码向后端发送异步post请求，携带输入框输入内容，还有获取系统当前时间作为time属性提交到后端。当前页面不需要刷新，只需要把当前发送的消息追加到聊天对话框的最下边，此时禁止发送消息，也就是发送消息按钮点击失效。直到收到后端的响应为止，后端servlet会响应一个message对象，表示AI对象的回复，收到响应后将消息追加显示到聊天对话框最下边。不要忘记每条消息显示都要在上边显示一个消息发送的time属性，同时接触发送消息按钮的禁用，恢复发送功能。

7.总之效果就像我和你聊天的这个窗口差不多。



使用jsp代码帮我写一个comment.jsp，还有相应css样式，需要和index.jsp样式适配，我需要实现以下功能：

1.标题"会话评价"

2.来到comment.jsp时本身会带有一个user_id属性，一个con_id属性

3.居中显示一个表单，提示输入会话评价，提供一个按钮提交评价，向后端发送post请求，携带con_id，user_id，以及输入的评价内容。

4.表单上边提供一个返回首页的超链接，携带参数user_id





```
我这段代码能实现依赖注入吗
@Startup
@Singleton
public class Config {
   @Provides
   public UserDao provideUserDao() {
      return new UserDao();
   }
   @Provides
   public SessionDao provideSessionDao() {
      return new SessionDao();
   }
   @Provides
   public MessageDao provideMessageDao() {
      return new MessageDao();
   }
}

public class MyTest {
	@Inject
	private UserDao userDao;
	@Test
	public void  testJPA(){
		System.out.println(userDao.findById(1));
	}
}
```

