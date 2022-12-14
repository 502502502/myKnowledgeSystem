##### html代码：

```html

```



##### js代码：

```js

```



##### 运行结果：



# 一、

1.什么是XML？XML的设计目的是什么？

XML是一门可扩展标记语言，可以用来传输和存储数据；



2.XML和HTML，以及JavaScript、ASP、PHP是什么关系？

HTML是超文本标记语言，包括显示数据；

JavaScript是一本脚本语言，通过对象和事件机制控制页面；

ASP可以用来创建动态交互web网页；

PHP与ASP类似；

XML可以向这些语言传输数据，存储数据



3.XML和HTML有什么共同点和区别？

XML和HTML都是通过标签和结构，xml可以自定义标签，html不可以；

xml不需要展示页面，html需要展示页面



4.XML有什么特性?

简介易懂；

数据的描述和显示分离；

使用自定义的标签来描述数据；

易于数据的交换和共享



5、

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>表格</title>
</head>
<body>
    <h1 align="center">进货单</h1>
    <table width="400" border="1" style="margin-left: auto;margin-right: auto;">
        <tr>
            <td>部门</td>
            <td>货号</td>
            <td>货名</td>
            <td>数量</td>
            <td>产地</td>
        </tr>
        <tr>
            <td rowspan="3" width="50" align="center">超市</td>
            <td width="50" align="center">sm1101</td>
            <td width="50" align="center">白糖</td>
            <td width="50" align="center">180</td>
            <td width="50" align="center">南宁</td>
        </tr>
        <tr>
            <td width="50" align="center">sm1107</td>
            <td width="50" align="center">牙膏</td>
            <td width="50" align="center">90</td>
            <td width="50" align="center">天津</td>
        </tr>
        <tr>
            <td width="50" align="center">sm1122</td>
            <td width="50" align="center">保温杯</td>
            <td width="50" align="center">83</td>
            <td width="50" align="center">临忻</td>
        </tr>
        <tr>
            <td width="50" align="center">家电部</td>
            <td width="50" align="center">fe-5201</td>
            <td width="50" align="center">液晶电视</td>
            <td width="50" align="center">82</td>
            <td width="50" align="center">青岛</td>
        </tr>
    </table>
</body>
</html>
```

6、

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>公司员工个人信息</title>
</head>
<body>
<div style="margin-left: auto;margin-right: auto;width: fit-content;margin-top: 30px">
  <form method="post" action="">
    <span  style="text-align: center;background-color: #dcdadc;width: auto">公司员工个人信息</span><br/>
      <label style="width: 10px">工号：</label>
      <input type="text"><br/>
      <label>姓名：</label>
      <input type="text"><br/>
      <label>性别：</label>
      <input type="radio" name="sex" checked>女
      <input type="radio" name="sex" checked>男<br/>
      <label>出生日期：</label>
      <input type="text"><br/>
      <label>所学专业：</label>
      <select name="zhuanye" id="zhaunye">
          <option>机械制造</option>
          <option>计算机</option>
          <option>电子</option>
      </select><br>
      <label>所属部门：</label>
      <select name="department" id="department">
          <option>安全部</option>
          <option>一车间</option>
          <option>质检部</option>
      </select><br/>
      <label>家庭情况:</label>
      <textarea rows="5"></textarea><br/>
      <label>特长:</label>
      <input type="checkbox" name="run">跑步
      <input type="checkbox" name="read">阅读
      <input type="checkbox" name="buy">购物<br/>
      <input type="submit" value="提交">
      <input type="button" value="重置" onclick="">
  </form>
</div>
</body>
</html>
```





# 二、

#### 1、选择题

ADCAB

CBCAD

BBCAC

DBDBB

#### 2、简答题

(1)、

JavaScript是一种具有函数优先的轻量级、解释型或即时编译型的编程语言；

用途：广泛用于Web应用开发，常用来在网页中添加一些动态效果与交互功能；

特点：1、解释型脚本语言；2、基于对象；3、弱类型；4、动态性；5、跨平台

Java与JavaScript 的区别： Java 是一种 OOP 编程语言，而 Java Script 是一种 OOP 脚本语言。 Java 创建在虚拟机或浏览器中运行的应用程序，而 JavaScript 代码仅在浏览器中运行。

(2)、

json是一种轻量级的数据交互格式；

语法：json的语法与js中object的语法几乎一致，json数据以键值对形式存在，多个键值对之间用逗号,隔开，键值对的键和值之间用冒号:连接；

用途：提供了一种优秀的面向对象的方法，以便将元数据缓存到客户机上；帮助分离了验证数据和逻辑；帮助为 Web 应用程序提供了 Ajax 的本质。

数组：[{"name":"JSON","address":"北京市西城区","age":25}]

对象：{"name":"JSON","address":"北京市西城区","age":25}

字符串转对象：

​	var test2 = '{"name": "jessie", "sex": "female"}';

​	var obj2 = JSON.parse(test2);

对象转字符串：

​	var object1 = {"name": "jessie"};
​	JSON.stringify(object1));

#### 3、

##### html代码：

```html
<h1 align="center">课程表</h1>
    <table width="400" border="1" style="margin-left: auto;margin-right: auto;">
        <tr>
            <td>
                <-->设置id，用于js获取元素</-->
                <button type="button" id="com" >计算机组成原理</button>
            </td>
            <td>
                <button type="button" id="jav">Java程序性设计</button>
            </td>
            <td>
                <button type="button" id="ana">算法分析</button>
            </td>
            <td>
                <button type="button" id="mat">微积分</button>
            </td>
            <td>
                <button type="button" id="web">计算机网络</button>
            </td>
        </tr>
    </table>

	<-->外部链接js脚本；使用jquery函数库</-->
    <script src="https://code.jquery.com/jquery-3.3.1.min.js" crossorigin="anonymous"></script>
    <script th:src="@{/js/xmlWork.js}"></script>
```

##### js代码：

```js
$(function(){
    //绑定点击事件
	$("#com").click(click1);
	$("#jav").click(click2);
	$("#mat").click(click3);
	$("#web").click(click4);
	$("#ana").click(click5);
});
function click1(){
    //打开新的窗口，指定页面文件位置，以及窗口大小、位置信息
	myWindow = window.open('component', '', 'height=300, width=400, top=300,left=500, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no');
}
function click2(){
	myWindow = window.open('java', '', 'height=300, width=400, top=300,left=500, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no');
}
function click3(){
	myWindow = window.open('math', '', 'height=300, width=400, top=300,left=500, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no');
}
function click4(){
	myWindow = window.open('web', '', 'height=300, width=400, top=300,left=500, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no');
}
function click5(){
	myWindow = window.open('analize', '', 'height=300, width=400, top=300,left=500, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no');
}
```

##### 运行结果：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105120153695.png" alt="image-20221105120153695" style="zoom: 80%;" />

#### 4、

##### html代码：

```html
<div style="margin-left: auto;margin-right: auto;width: fit-content;margin-top: 30px">
    <form method="post" action="">
        <span style="text-align: center;background-color: #dcdadc;width: auto">购货订单</span><br/>
        <label style="width: 10px">姓名：</label>
        <input type="text" id="name"><br/>
        <label>电话：</label>
        <input type="text" id="phone"><br/>
        <label>商品：</label>
        <select id="type" name="type">
            <-->设置商品单价</-->
            <option value="10">苹果: 10r /kg</option>
            <option value="5">香蕉: 5r /kg</option>
            <option value="20">葡萄: 20r /kg</option>
        </select><br/>
        <label>数量：</label>
        <input type="text" id="num"><br/>
        <label>金额：</label>
        <input type="text" id="sum"><br/>
        <input type="button" value="提交" id="submit">
    </form>
</div>
<-->链接外部js脚本</-->
<script src="https://code.jquery.com/jquery-3.3.1.min.js" crossorigin="anonymous"></script>
<script th:src="@{/js/xmlWork.js}"></script>
```

##### js代码：

```js
$(function(){
	//点击事件
	$("#submit").click(click6);
});

function click6(){
    //检查数量是否为0
	if($("#num").val() == 0){
		alert("数量不能为0！");
	}else{
    //检查金额是否正确
		var total = $("#type").val() *$("#num").val();
		if(total != $("#sum").val()){
			alert("商品总价值与所填金额不符！，总价值为：" +total);
		}else{
            //弹出成功页面
			myWindow = window.open('success', '', 'height=300, width=400, top=300,left=500, 
                                   toolbar=no, menubar=no, scrollbars=no, 
                                   resizable=no,location=no,status=no');
		}
	}
}
```



##### 运行结果：

数量为0提示：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105124119857.png" alt="image-20221105124119857" style="zoom: 80%;" />

金额不符合提示：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105124215010.png" alt="image-20221105124215010" style="zoom: 80%;" />



订购成功提示：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105124049359.png" alt="image-20221105124049359" style="zoom: 80%;" />

#### 5、

##### html代码：

```html
<div style="margin-left: auto;margin-right: auto;width: fit-content;margin-top: 200px">
    <form method="post" action="">
        <span style="text-align: center;background-color: #dcdadc;width: auto">用户登记</span><br/>

        <label style="width: 10px">用户名：</label>
        <input type="text" id="name"><br/>

        <label>口令：</label>
        <input type="text" id="password"><br/>

        <input type="button" value="提交" id="submit">
    </form>
</div>
```

##### js代码：

```js
<script type="text/javascript">
    $(function () {
        $("#submit").click(click);
    });

    function click() {
        //获取用户名和密码
        var name = $("#name").val();
        var pas = $("#password").val();

        //检查账号是否为空
        if (name.length == 0) {
            alert("用户名不能为空！");
            return;
        }
        //检查账号是否存在
        if (localStorage.getItem(name) != null) {
            alert("该用户名已存在！");
            return;
        }
        //检查密码长度
        if (pas.length < 10) {
            alert("密码长度不能小于10！");
            return;
        }
        //检查密码是否包含数字
        var regex = new RegExp('(?=.*[0-9])');
        if (!regex.test(pas)) {
            alert("密码中必须包含数字，请及时改密码！");
            return;
        }
        //是否包含大写字母
        regex = new RegExp('(?=.*[A-Z])');
        if (!regex.test(pas)) {
            alert("密码中必须包含大写字母，请及时改密码！");
            return;
        }
        //是否包含小写字母
        regex = new RegExp('(?=.*[a-z])');
        if (!regex.test(pas)) {
            alert("密码中必须包含小写字母，请及时改密码！");
            return;
        }

        //密码合格，存入本地
        localStorage.setItem(name,pas);
        alert("登记成功！");
    }
</script>
```



##### 运行结果：

用户名为空：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105132525124.png" alt="image-20221105132525124" style="zoom: 80%;" />

用户名已存在：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105133122547.png" alt="image-20221105133122547" style="zoom: 80%;" />



密码长度小于10：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105132443517.png" alt="image-20221105132443517" style="zoom: 80%;" />

密码不包含数字：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105132632708.png" alt="image-20221105132632708" style="zoom: 80%;" />



密码不包含大写字母：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105132653455.png" alt="image-20221105132653455" style="zoom: 80%;" />



密码不包含小写字母：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105132714118.png" alt="image-20221105132714118" style="zoom: 80%;" />



存储成功：

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221105133158963.png" alt="image-20221105133158963" style="zoom: 80%;" />



# 三、

#### 1、选择题

CBABC

BACDA

ABBCC

DADBC

#### 2、简答题

##### ①、xml发展历史和主要应用领域

SGML复杂、昂贵，几个主要的浏览器厂 商都明确拒绝支持SGML，SGML在网上传播遇到巨大障碍；

TML是专门为描述主页的表现形式而设计的，它疏于对信息语义及 其内部结构的描述；无法描述矢 量图形、科技符号和其他的一些特殊显示效果；松散的语法要求使得文档结构混乱而 缺乏条理性；

因此，1996年人们开始致力于描述一个标记语言，它既具有 SGML的强大功能和可扩展性，同时又具有HTML的简单性。最后，W3C于1998年2月批准了XML的1.0 版本，一个崭新而大有前途的语言诞生了；

主要应用于设计标记语言、文件保值、数据交换、替代传统的 EDI、智能代理、Web应用。

##### ②、xml和html的区别，xml的保值性是什么

 HTML 不具有扩展性 ，XML是元标记语言，可用于定义新的标 记语言;

 HTML侧重于如何表现信息 ,XML侧重于如何结构化地描述信息 ;

HTML不要求标记的嵌套、配对，不要求标记之间具有一定的顺序 ，XML严格要求嵌套、配对，和遵循DTD的 树形结构 ；

HTML难于阅读、维护 ，XML结构清晰，便于阅读、维护 ；

HTML内容描述与显示方式整合为一体 ，XML内容描述与显示方式相分离；

HTML不具有保值性，XML 具有保值性 ；

HTML已有大量的编辑、浏览工具，XML 编辑、浏览工具尚不成熟；

XML不但能够长期作为一种通用的标准，而且很容易向其他格式的文档转化，所以文档可以轻易的保存较长的时间，不会因为格式迭代问题打不开，这就是XML的保值性。



#### 3、成绩单描述

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--DTD定义-->
<!--根节点为学生列表-->
<!--        学生列表包含若干个学生-->
<!--        每个学生包括姓名，平均成绩和一个学科成绩列表-->
<!--        学科成绩包括课程名称和单科成绩-->
<!DOCTYPE 学生列表 [
        <!ELEMENT 学生列表 (学生)*>
        <!ELEMENT 学生 (姓名,平均成绩,学科成绩*)>
        <!ELEMENT 学科成绩 (课程名称,单科成绩)>
        <!ELEMENT 姓名 (#PCDATA)>
        <!ELEMENT 平均成绩 (#PCDATA)>
        <!ELEMENT 课程名称 (#PCDATA)>
        <!ELEMENT 单科成绩 (#PCDATA)>
        ]>

<!--XML描述-->
<学生列表>
    <学生>
        <姓名>张三</姓名>
        <平均成绩>85.76</平均成绩>
        <学科成绩>
            <课程名称>编译方法</课程名称>
            <单科成绩>79</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>C程序设计</课程名称>
            <单科成绩>85</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>数据结构</课程名称>
            <单科成绩>93</单科成绩>
        </学科成绩>
    </学生>
    <学生>
        <姓名>李四</姓名>
        <平均成绩>84.33</平均成绩>
        <学科成绩>
            <课程名称>计算复杂性</课程名称>
            <单科成绩>72</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>偏微分方程</课程名称>
            <单科成绩>86</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>计算方法</课程名称>
            <单科成绩>95</单科成绩>
        </学科成绩>
    </学生>
    <学生>
        <姓名>王五</姓名>
        <平均成绩>86.25</平均成绩>
        <学科成绩>
            <课程名称>分子轨道理论</课程名称>
            <单科成绩>79</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>有机化学</课程名称>
            <单科成绩>80</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>分子生物学</课程名称>
            <单科成绩>88</单科成绩>
        </学科成绩>
        <学科成绩>
            <课程名称>无机化学</课程名称>
            <单科成绩>98</单科成绩>
        </学科成绩>
    </学生>
</学生列表>
```

#### 4、班级通讯录

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--DTD定义-->
<!--根节点为通讯录列表-->
<!--        通讯录列表包含若干个学生-->
<!--        每个学生包括姓名（必选），学号（必选），联系电话（必选），
                                QQ（可选）、微信（可选）、邮箱（可选）-->
<!DOCTYPE 通讯录列表 [
        <!ELEMENT 通讯录列表 (学生)*>
        <!ELEMENT 学生 (姓名,学号,联系电话,QQ?,微信?,邮箱?)>
        <!ELEMENT 姓名 (#PCDATA)>
        <!ELEMENT 学号 (#PCDATA)>
        <!ELEMENT 联系电话 (#PCDATA)>
        <!ELEMENT QQ (#PCDATA)>
        <!ELEMENT 微信 (#PCDATA)>
        <!ELEMENT 邮箱 (#PCDATA)>
        ]>

<!--XML描述-->
<通讯录列表>
    <学生>
        <姓名>张三</姓名>
        <学号>10000</学号>
        <联系电话>20000</联系电话>
        <QQ>30000</QQ>
        <微信>40000</微信>
        <邮箱>50000</邮箱>
    </学生>
    <学生>
        <姓名>李四</姓名>
        <学号>10001</学号>
        <联系电话>20001</联系电话>
        <QQ>30000</QQ>
    </学生>
    <学生>
        <姓名>王五</姓名>
        <学号>10002</学号>
        <联系电话>20002</联系电话>
        <微信>40002</微信>
    </学生>
    <学生>
        <姓名>赵六</姓名>
        <学号>10003</学号>
        <联系电话>20003</联系电话>
        <邮箱>50003</邮箱>
    </学生>
    <学生>
        <姓名>苏七</姓名>
        <学号>10005</学号>
        <联系电话>20005</联系电话>
        <QQ>30005</QQ>
        <微信>40005</微信>
    </学生>
</通讯录列表>
```





# 四

**作业思路：**

①、写DTD，写结点定义，若有必要，给结点写属性定义；

②、接着根据DTD写XML，结构一搭好，数据全靠编；

③、然后根据XML写XSL，有循环用for-each标签，有条件用if标签，条件多用choose标签；

④、最后测试，使用火狐浏览器显示XML与XSL渲染；

⑤、编码思路在注释里；编码的时候先写注释，再根据注释去分别实现，条理清晰，老人孩子都爱用；



#### 1、唱片表

**dtd**

```xml-dtd

        <!--根结点，唱片列表-->
        <!ELEMENT catalog (cd)*>
<!--        唱片结点，包含名称。作者，国家，出版公司，价格，发行年份-->
        <!ELEMENT cd (title,artist,country,company,price,year)>
        <!ELEMENT title (#PCDATA)>
        <!ELEMENT artist (#PCDATA)>
        <!ELEMENT country (#PCDATA)>
        <!ELEMENT company (#PCDATA)>
        <!ELEMENT price (#PCDATA)>
        <!ELEMENT year (#PCDATA)>
```

**xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE catalog SYSTEM "catalog.dtd">
<!--引用XSL显示数据-->
<?xml-stylesheet type="text/xsl" href="catalog.xsl"?>


<!--XML描述-->
<catalog>
    <cd>
        <title>Emre Burlee</title>
        <artist>Bob Dylan  1</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>7.90</price>
        <year>1985</year>
    </cd>
    <cd>
        <title>Uncn my hea</title>
        <artist>Joe Cocker</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>8.20</price>
        <year>1987</year>
    </cd>
    <cd>
        <title>Empire lesque</title>
        <artist>Bob Dyl</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>10.90</price>
        <year>1977</year>
    </cd>
    <cd>
        <title>hain my heart</title>
        <artist>Joe Cocr</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>9.20</price>
        <year>1987</year>
    </cd>
    <cd>
        <title>Empdve Bcsj lue</title>
        <artist>Bob Dyn</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>10.70</price>
        <year>1976</year>
    </cd>
    <cd>
        <title>dcfb hear  dt</title>
        <artist>Joe Cocker</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>9.22</price>
        <year>1990</year>
    </cd>
    <cd>
        <title>Empurles que</title>
        <artist>Bb Dylan</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>7.45</price>
        <year>1991</year>
    </cd>
    <cd>
        <title>aicdsjvn my heart</title>
        <artist>Joe Cocker</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>8.89</price>
        <year>1994</year>
    </cd>
    <cd>
        <title>Empire  Gjdkc Burlescdsjke</title>
        <artist>Bob Dylan</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>9.50</price>
        <year>1999</year>
    </cd>
    <cd>
        <title>Unchain cdnsj heart</title>
        <artist>Joe Cocker</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>7.64</price>
        <year>1998</year>
    </cd>
    <cd>
        <title>Empire Burbjdb</title>
        <artist>Bob Dylan</artist>
        <country>USA</country>
        <company>Columbia</company>
        <price>10.90</price>
        <year>1997</year>
    </cd>
    <cd>
        <title>no my heart</title>
        <artist>Joe Cocker</artist>
        <country>USA</country>
        <company>EMI</company>
        <price>7.20</price>
        <year>1996</year>
    </cd>
</catalog>
```

**xsl**

```html
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl ="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="/">
        <html>
            <body>
<!--                表名-->
                <h2>My CD Collection</h2>
<!--                列名-->
                <table border="1">
                    <tr><th bgcolor="#98fb98">Title</th>
                        <th bgcolor="#98fb98">Artist</th>
                        <th bgcolor="#98fb98">country</th>
                        <th bgcolor="#98fb98">company</th>
                        <th bgcolor="#98fb98">price</th>
                        <th bgcolor="#98fb98">year</th>
                    </tr>
<!--                    循环显示唱片-->
                    <xsl:for-each select="catalog/cd">
<!--                        按照年份降序-->
                        <xsl:sort select="year" order="descending"/>
<!--                        如果年份在1980以后才显示-->
                        <xsl:if test="year &gt; 1980">
                            <tr>
<!--                                显示标题-->
                                <td>
                                    <xsl:value-of select="title"/>
                                </td>
<!--                                显示作者-->
                                <td>
                                    <xsl:value-of select="artist"/>
                                </td>
<!--                                显示国家-->
                                <td>
                                    <xsl:value-of select="country"/>
                                </td>
<!--                                显示公司-->
                                <td>
                                    <xsl:value-of select="company"/>
                                </td>
<!--                                判断价格区间,显示不同颜色-->
                                <xsl:choose>
<!--                                    在10以上-->
                                    <xsl:when test="price &gt; 10">
                                        <td bgcolor="#f08080">
                                            <xsl:value-of select="price"/>
                                        </td>
                                    </xsl:when>
<!--                                    在9~10-->
                                    <xsl:when test="price &gt; 9">
                                        <td bgcolor="#ffa07a">
                                            <xsl:value-of select="price"/>
                                        </td>
                                    </xsl:when>
<!--                                    在8~9-->
                                    <xsl:when test="price &gt; 8">
                                        <td bgcolor="#ffc0cb">
                                            <xsl:value-of select="price"/>
                                        </td>
                                    </xsl:when>
<!--                                    其余价格-->
                                    <xsl:otherwise>
                                        <td bgcolor="#e0ffff">
                                            <xsl:value-of select="price"/>
                                        </td>
                                    </xsl:otherwise>
                                </xsl:choose>
<!--                                显示年份-->
                                <td>
                                    <xsl:value-of select="year"/>
                                </td>
                            </tr>
                        </xsl:if>
                    </xsl:for-each>
                </table>
            </body>
        </html>
    </xsl:template>
</xsl:stylesheet>

```



#### 2、成绩表



**dtd**

```xml-dtd
        <!--DTD定义-->
        
<!--        根元素，包含若干学生-->
        <!ELEMENT students (student)*>
<!--        学生元素，包含姓名，平均成绩和课程成绩-->
        <!ELEMENT student (name,avgscore,course*)>
<!--        课程成绩包含课程名称，课程成绩，录入时间-->
        <!ELEMENT course (title,score,date)>

        <!ELEMENT name (#PCDATA)>
        <!ATTLIST name rowspan CDATA  "1">
        <!ELEMENT avgscore (#PCDATA)>
        <!ATTLIST avgscore rowspan CDATA  "1">
        <!ELEMENT title (#PCDATA)>
        <!ATTLIST title rowspan CDATA  "1">
        <!ELEMENT score (#PCDATA)>
        <!ATTLIST score rowspan CDATA  "1">
        <!ELEMENT date (#PCDATA)>
        <!ATTLIST date rowspan CDATA  "1">

```

**xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE students SYSTEM "score.dtd">
<!--引用XSL显示数据-->
<?xml-stylesheet type="text/xsl" href="score.xsl"?>

<!--XML描述-->
<students>
    <student>
        <name rowspan="3">张三</name>
        <avgscore rowspan="3"></avgscore>
        <course>
            <title>编译方法</title>
            <score>79</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>C程序设计</title>
            <score>85</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>数据结构</title>
            <score>93</score>
            <date>2022/11/19</date>
        </course>
    </student>
    <student>
        <name rowspan="3">李四</name>
        <avgscore rowspan="3"></avgscore>
        <course>
            <title>计算复杂性</title>
            <score>72</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>偏微分方程</title>
            <score>86</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>计算方法</title>
            <score>95</score>
            <date>2022/11/19</date>
        </course>
    </student>
    <student>
        <name rowspan="4">王五</name>
        <avgscore rowspan="4"></avgscore>
        <course>
            <title>分子轨道理论</title>
            <score>79</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>有机化学</title>
            <score>80</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>分子生物学</title>
            <score>88</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>无机化学</title>
            <score>98</score>
            <date>2022/11/19</date>
        </course>
    </student>
</students>
```

**xsl**

```html
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl ="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="/">
        <html>
            <body>
                <table border="1">
<!--                    表名-->
                    <caption>学生成绩表</caption>
<!--                    列明-->
                    <tr>
                        <th>姓名</th>
                        <th>平均成绩</th>
                        <th>课程名称</th>
                        <th>单科成绩</th>
                    </tr>
<!--                    循环显示学生-->
                    <xsl:for-each select="//student">
<!--                        每个学生循环显示课程-->
                        <xsl:for-each select="course">
<!--                            课程按照成绩升序-->
                            <xsl:sort select="score"/>
                            <tr>
<!--                                显示姓名和成绩，只显示一次，从属性中获取跨行的数量-->
                                <xsl:if test="position()=1">
<!--                                    姓名-->
                                    <td>
<!--                                        设置跨行数-->
                                        <xsl:attribute name = "rowspan">
                                            <xsl:value-of select = "../name/@rowspan"/>
                                        </xsl:attribute>
<!--                                        显示姓名-->
                                        <xsl:value-of select="../name"/>
                                    </td>
<!--                                    平均成绩-->
                                    <td align="right">
<!--                                        设置跨行数-->
                                        <xsl:attribute name="rowspan">
                                            <xsl:value-of select = "../avgscore/@rowspan"/>
                                        </xsl:attribute>
<!--                                        计算平均成绩-->
                                        <xsl:value-of select='format-number(sum(../course/score) div count(../course), "##.00")'/>
                                    </td>
                                </xsl:if>
<!--                                课程名称-->
                                <td><xsl:value-of select="title"/></td>
<!--                                课程成绩-->
                                <td align="right"><xsl:value-of select="score"/></td>
                            </tr>
                        </xsl:for-each>
                    </xsl:for-each>
                </table>
            </body>
        </html>
    </xsl:template>
</xsl:stylesheet>

```



#### 3、通讯录

**dtd**

```xml-dtd

<!--      人员列表,包含若干人-->
        <!ELEMENT persons (person)*>
<!--      人元素,包含姓名,编号,性别,地址和若干电话-->
        <!ELEMENT person (name,no,sex,address,phone*)>
<!--      电话号码的数量,用于设置跨行数-->
        <!ATTLIST person phoneCount CDATA  "1">
<!--      电话,包含号码和类型-->
        <!ELEMENT phone (phonenumber,phonetype)>
        <!ELEMENT name (#PCDATA)>
        <!ELEMENT no (#PCDATA)>
        <!ELEMENT sex (#PCDATA)>
        <!ELEMENT address (#PCDATA)>
        <!ELEMENT phonenumber (#PCDATA)>
        <!ELEMENT phonetype (#PCDATA)>

```

**xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE persons SYSTEM "personList.dtd">
<!--引用XSL显示数据-->
<?xml-stylesheet type="text/xsl" href="personList.xsl"?>


<!--XML描述-->
<persons>
    <person phoneCount="2">
        <name>李倩倩</name>
        <no>10191101</no>
        <sex>女</sex>
        <address>前卫胡同 126 号</address>
        <phone>
            <phonenumber>88662255</phonenumber>
            <phonetype>住宅</phonetype>
        </phone>
        <phone>
            <phonenumber>13012345678</phonenumber>
            <phonetype>移动电话</phonetype>
        </phone>
    </person>
    <person phoneCount="1">
        <name>王晓明</name>
        <no>10191203</no>
        <sex>男</sex>
        <address>北京大街 103 号</address>
        <phone>
            <phonenumber>85943212</phonenumber>
            <phonetype>办公</phonetype>
        </phone>
    </person>
</persons>
```

**xsl**

```html
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl ="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="/">
        <html>
            <body>
                <table border="1">
<!--                    表名-->
                    <caption>通讯录</caption>
<!--                    列名-->
                    <tr>
                        <td>姓名</td>
                        <td>会员编号</td>
                        <td>性别</td>
                        <td>地址</td>
                        <td>电话号码</td>
                        <td>电话类型</td>
                    </tr>
<!--                    循环显示每个人-->
                    <xsl:for-each select="//person">
<!--                        每个人循环显示电话-->
                        <xsl:for-each select="phone">
                            <tr>
<!--                                显示除了电话之外的属性,若电话有多个,这些属性只显示一次-->
                                <xsl:if test="position()=1">
<!--                                    显示姓名-->
                                    <td>
<!--                                        根据电话数量设置跨行数-->
                                        <xsl:attribute name = "rowspan">
                                            <!--                                            <xsl:value-of select = "count(../course)"/>-->
                                            <xsl:value-of select = "../../person/@phoneCount"/>
                                        </xsl:attribute>
                                        <xsl:value-of select="../name"/>
                                    </td>
<!--                                    显示编号-->
                                    <td align="right">
                                        <xsl:attribute name="rowspan">
                                            <!--                                            <xsl:value-of select = "count(../course)"/>-->
                                            <xsl:value-of select = "../../person/@phoneCount"/>
                                        </xsl:attribute>
                                        <xsl:value-of select="../no"/>
                                    </td>
<!--                                    显示性别-->
                                    <td>
                                        <xsl:attribute name = "rowspan">
                                            <!--                                            <xsl:value-of select = "count(../course)"/>-->
                                            <xsl:value-of select = "../../person/@phoneCount"/>
                                        </xsl:attribute>
                                        <xsl:value-of select="../sex"/>
                                    </td>
<!--                                    显示地址-->
                                    <td align="right">
                                        <xsl:attribute name="rowspan">
                                            <!--                                            <xsl:value-of select = "count(../course)"/>-->
                                            <xsl:value-of select = "../../person/@phoneCount"/>
                                        </xsl:attribute>
                                        <xsl:value-of select="../address"/>
                                    </td>
                                </xsl:if>
                                <td><xsl:value-of select="phonenumber"/></td>
                                <td align="right"><xsl:value-of select="phonetype"/></td>
                            </tr>
                        </xsl:for-each>
                    </xsl:for-each>
                </table>
            </body>
        </html>
    </xsl:template>
</xsl:stylesheet>

```



**运行结果**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210193218531.png" alt="image-20221210193218531" style="zoom:80%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210193318177.png" alt="image-20221210193318177" style="zoom:67%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210193412905.png" alt="image-20221210193412905" style="zoom:67%;" />



# 五、

DTD定义数据后，xml编写数据，xsl用表格展示成绩，html用下拉框展示姓名，学院，使用JavaScript动态根据xml生成下拉框的选项，使用xsl将选中的学生成绩进行展示。当下拉框选中发生变化后，使用JavaScript更新学院和成绩表单。

**dtd**

```xml-dtd
        <!--DTD定义-->

<!--        根元素，包含若干学生-->
        <!ELEMENT students (student)*>
<!--        学生元素，包含姓名，平均成绩，专业和课程成绩-->
        <!ELEMENT student (name,avgscore,dept,course*)>
<!--        课程成绩包含课程名称，课程成绩，考试日期-->
        <!ELEMENT course (title,score,date)>

        <!ELEMENT name (#PCDATA)>
        <!ATTLIST name rowspan CDATA  "1">
        <!ELEMENT avgscore (#PCDATA)>
        <!ATTLIST avgscore rowspan CDATA  "1">
        <!ELEMENT dept (#PCDATA)>
        <!ELEMENT title (#PCDATA)>
        <!ATTLIST title rowspan CDATA  "1">

        <!ELEMENT score (#PCDATA)>
        <!ATTLIST score rowspan CDATA  "1">
        <!ELEMENT date (#PCDATA)>
        <!ATTLIST date rowspan CDATA  "1">

```



**xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE students SYSTEM "students_exam_results.dtd">
<!--引用XSL显示数据-->
<?xml-stylesheet type="text/xsl" href="students_exam_results.xsl"?>

<!--XML描述-->
<students>
    <student>
        <name rowspan="3">张三</name>
        <avgscore rowspan="3"></avgscore>
        <dept>计算机</dept>
        <course>
            <title>编译方法</title>
            <score>79</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>C程序设计</title>
            <score>85</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>数据结构</title>
            <score>93</score>
            <date>2022/11/19</date>
        </course>
    </student>
    <student>
        <name rowspan="3">李四</name>
        <avgscore rowspan="3"></avgscore>
        <dept>数学</dept>
        <course>
            <title>计算复杂性</title>
            <score>72</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>偏微分方程</title>
            <score>86</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>计算方法</title>
            <score>95</score>
            <date>2022/11/19</date>
        </course>
    </student>
    <student>
        <name rowspan="4">王五</name>
        <avgscore rowspan="4"></avgscore>
        <dept>化学</dept>
        <course>
            <title>分子轨道理论</title>
            <score>79</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>有机化学</title>
            <score>80</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>分子生物学</title>
            <score>88</score>
            <date>2022/11/19</date>
        </course>
        <course>
            <title>无机化学</title>
            <score>98</score>
            <date>2022/11/19</date>
        </course>
    </student>
</students>
```



**xsl**

```html
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method='html' version='1.0' encoding='utf-8' indent='yes'/>
    <xsl:template match="student">
        
        <table border="1">
            <!-- 列名-->
            <tr>
                <th>课程名称</th>
                <th>考试日期</th>
                <th>成绩</th>
            </tr>
            <!--每个学生循环显示课程-->
            <xsl:for-each select="course">
                <!--课程按照成绩升序-->
                <xsl:sort select="score"/>
                <tr>
                    <!-- 课程名称-->
                    <td>
                        <xsl:value-of select="title"/>
                    </td>
                    <!--考试日期-->
                    <td>
                        <xsl:value-of select="date"/>
                    </td>
                    <!--课程成绩-->
                    <td>
                        <xsl:value-of select="score"/>
                    </td>
                </tr>
            </xsl:for-each>
        </table>
    </xsl:template>
</xsl:stylesheet>

```



**html**

```html
<body onload="init()">
    <!-- 表名-->
    <h3 width="100px">学生成绩单</h3>
    <span>姓名：</span>
    <!-- 姓名下拉框，当选中的姓名发生变化时，出发update函数进行重新渲染 -->
    <select onchange="update()" id="name"></select>
    <span>学院：</span>
    <span id="dept"></span>
    <!-- 成绩表单 -->
    <div id="content"></div>
```



**javascript**

```javascript
<script type="text/javascript">
        var xmlDoc;
        var xslDoc;
        var nodes;
        var maxLen = 0;
        var index = 0;
        // 初始化表单数据，加载xml和xsl文件，选择第一个studdent结点进行展示
        function init() {
            xmlDoc = loadXMLDoc("students_exam_results.xml");
            xslDoc = loadXMLDoc("students_exam_results.xsl");
            nodes = xmlDoc.getElementsByTagName('student');
            maxLen = nodes.length - 1;
            display();
        }
        //展示下拉框和第一个结点数据
        function display() {
            var sel = document.getElementById("name");
            // 下拉框选项
            for (i = 0; i <= maxLen; i++) {
                var opt = document.createElement("option");
                opt.innerHTML = nodes[i].childNodes[0].childNodes[0].nodeValue;
                sel.appendChild(opt);
            }
            // 第一个student数据
            if (maxLen >= 0) {
                document.getElementById("dept").innerHTML = nodes[index].childNodes[2].childNodes[0].nodeValue;
                var node = nodes[index];
                var cont = document.getElementById("content");
                // 使用xsl进行展示，将xsl生成的html插入到html
                cont.innerHTML = node.transformNode(xslDoc);
            }
        }
        // 根据下拉框选中的名字，改变学院，以及成绩
        function update() {
            // 获取选中
            var index = document.getElementById("name").selectedIndex;
            // 更改学院
            document.getElementById("dept").innerHTML = nodes[index].childNodes[2].childNodes[0].nodeValue;
            var node = nodes[index];
            var cont = document.getElementById("content");
            // 更改成绩
            cont.innerHTML = node.transformNode(xslDoc);
        }
        // 通用的加载xml文件模板
        function loadXMLDoc(dname) {
            var xmlDoc = null;
            try {//Internet Explorer
                xmlDoc = new ActiveXObject("Microsoft.XMLDOM")
            } catch (e) {
                try {//Firefox, Mozilla, Opera, etc.
                    xmlDoc = document.implementation.createDocument("", "", null)
                } catch (e) {
                    alert(e.message)
                }
            }
            try {
                xmlDoc.async = false;
                xmlDoc.load(dname);
                return (xmlDoc);
            } catch (e) {
                alert(e.message)
            }
            return (xmlDoc);
        }
    </script>
```

**运行结果**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210191933272.png" alt="image-20221210191933272" style="zoom: 80%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210191952409.png" alt="image-20221210191952409" style="zoom: 80%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210192018353.png" alt="image-20221210192018353" style="zoom:80%;" />



# 六、

xsl展示没有多于的技巧，该循环循环，设置好跨行和居中即可

**dtd**

```dtd
<!--DTD定义-->

<!--        根元素，包含若干基本信息-->
<!ELEMENT 学生 (个人基本信息,学历和工作简历,已修课程,已获奖励,已发表论文)>
<!--        个人基本信息-->
<!ELEMENT 个人基本信息 (照片,姓名,性别,民族,出生地,通讯地址,电子邮件)>
<!--        通讯地址-->
<!ELEMENT 通讯地址 (条目*)>
<!--        学历和工作简历-->
<!ELEMENT 学历和工作简历 (条目*)>
<!--        已修课程-->
<!ELEMENT 已修课程 (条目*)>
<!--        已获奖励-->
<!ELEMENT 已获奖励 (条目*)>
<!--        已发表论文-->
<!ELEMENT 已发表论文 (条目*)>

<!ELEMENT 照片 (#PCDATA)>
<!ELEMENT 姓名 (#PCDATA)>
<!ELEMENT 性别 (#PCDATA)>
<!ELEMENT 民族 (#PCDATA)>
<!ELEMENT 出生地 (#PCDATA)>
<!ELEMENT 电子邮件 (#PCDATA)>
<!ELEMENT 条目 (#PCDATA)>
```

**xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 外部引入DTD -->
<!DOCTYPE 学生 SYSTEM "student.dtd">
<!--引用XSL显示数据-->
<?xml-stylesheet type="text/xsl" href="student.xsl"?>
<学生>
    <个人基本信息>
        <照片>miffy.jpg</照片>
        <姓名>米菲</姓名>
        <性别>女</性别>
        <民族>兔佳族</民族>
        <出生地>大荷兰</出生地>
        <通讯地址>
            <条目>130012</条目>
            <条目>吉林省长春市前进大街 2699 号</条目>
            <条目>吉林大学计算机科学与技术学院</条目>
        </通讯地址>
        <电子邮件>miffy@yahoo.com</电子邮件>
    </个人基本信息>
    <学历和工作简历>
        <条目>2003 年毕业于吉林大学附属小学</条目>
        <条目>2006 年毕业于吉林大学附属中学初中部</条目>
        <条目>2009 年毕业于吉林大学附属中学高中部</条目>
        <条目>2013 年毕业于吉林大学计算机学院</条目>
        <条目>2012.07 至 2012.09 在吉林大学就业指导中心实习</条目>
    </学历和工作简历>
    <已修课程>
        <条目>数据结构</条目>
        <条目>数据库原理</条目>
        <条目>C 语言程序设计</条目>
        <条目>Java 语言程序设计</条目>
        <条目>Web 应用开发基础</条目>
        <条目>XML 语言</条目>
    </已修课程>
    <已获奖励>
        <条目>2012 获中国大学生创新项目一等奖</条目>
        <条目>2013 获中国大学生软件竞赛一等奖</条目>
    </已获奖励>
    <已发表论文>
        <条目>人机对话中关键技术的探索，2011 年发表于《机器与人》创刊号第 1 页</条目>
        <条目>米菲家族祖先追踪，2012 年发表于《物种起源》卷 99999 第 8888 页</条目>
    </已发表论文>
</学生>
```

**xsl**

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:template match="/">
        <html>
            <body>
                <font face="STXinwei">
                    <h3 align = "center">米菲的简历</h3>
                    <!-- 使用表格展示简历信息 -->
                    <table border="1" align = "center" cellpadding="10">                        <!--CCFFFF-->
                        <tr>
                            <td bgcolor="#5F9EA0" width = "70" align = "center">姓名</td>
                            <td bgcolor="#E0FFFF" width = "300">
                                <xsl:value-of select="/学生/个人基本信息/姓名"/>
                            </td>
                            <td rowspan="6">
                                <img>
                                    <xsl:attribute name="src">
                                        <xsl:value-of select="/学生/个人基本信息/照片"/>
                                    </xsl:attribute>
                                </img>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center">性别</td>
                            <td bgcolor="#E0FFFF">
                                <xsl:value-of select="/学生/个人基本信息/性别"/>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center">民族</td>
                            <td bgcolor="#E0FFFF">
                                <xsl:value-of select="/学生/个人基本信息/民族"/>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center">出生地</td>
                            <td bgcolor="#E0FFFF">
                                <xsl:value-of select="/学生/个人基本信息/出生地"/>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center">通讯地址</td>
                            <td bgcolor="#E0FFFF">
                                <xsl:for-each select="/学生/个人基本信息/通讯地址/条目">
                                    <ul>
                                        <li>
                                            <xsl:value-of select="."/>
                                        </li>
                                    </ul>
                                </xsl:for-each>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center">电邮</td>
                            <td bgcolor="#E0FFFF">
                                <xsl:value-of select="/学生/个人基本信息/电子邮件"/>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center" colspan = "3">学历和工作简历</td>
                        </tr>
                        <!-- 循环展示工作 -->
                        <tr>
                            <td bgcolor="#E0FFFF" colspan = "3">
                                <xsl:for-each select="/学生/学历和工作简历/条目">
                                    <ul>
                                        <li>
                                            <xsl:value-of select="."/>
                                        </li>
                                    </ul>
                                </xsl:for-each>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center" colspan = "3">已修课程</td>
                        </tr>
                        <!-- 循环展示已修课程 -->
                        <tr>
                            <td bgcolor="#E0FFFF" colspan = "3">
                                <xsl:for-each select="/学生/已修课程/条目">
                                    <ul>
                                        <li>
                                            <xsl:value-of select="."/>
                                        </li>
                                    </ul>
                                </xsl:for-each>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center" colspan = "3">已获奖励</td>
                        </tr>
                        <!-- 循环展示已获奖励 -->
                        <tr>
                            <td bgcolor="#E0FFFF" colspan = "3">
                                <xsl:for-each select="/学生/已获奖励/条目">
                                    <ol>
                                        <li>
                                            <xsl:value-of select="."/>
                                        </li>
                                    </ol>
                                </xsl:for-each>
                            </td>
                        </tr>
                        <tr>
                            <td bgcolor="#5F9EA0" align = "center" colspan = "3">已发表论文</td>
                        </tr>
                        <!-- 循环展示已发表论文 -->
                        <tr>
                            <td bgcolor="#E0FFFF" colspan = "3">
                                <xsl:for-each select="/学生/已发表论文/条目">
                                    <ol>
                                        <li>
                                            <xsl:value-of select="."/>
                                        </li>
                                    </ol>
                                </xsl:for-each>
                            </td>
                        </tr>
                    </table>>
                </font>
            </body>
        </html>
    </xsl:template>
</xsl:stylesheet>
```

**运行结果**

![image-20221210191710645](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20221210191710645.png)

