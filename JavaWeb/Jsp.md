# JSP页面元素

<%
		局部变量、Java语句
%>

<%!
		全局变量、定义方法
%>

<%=
		输出表达式														（可以直接解析html代码
%>

![image-20200924155123166](Jsp.assets/image-20200924155123166.png)

## 九大内置对象

![image-20200924155639246](Jsp.assets/image-20200924155639246.png)

![image-20200924165151245](Jsp.assets/image-20200924165151245.png)

## 请求转发和重定向

![image-20200924171448791](Jsp.assets/image-20200924171448791.png)

![](Jsp.assets/image-20200924171639006.png)

## 连接数据库

[使用JDBC连接MySql时出现：The server time zone value '�й���׼ʱ��' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration](https://www.cnblogs.com/EasonJim/p/6906713.html)

在连接字符串后面加上**?serverTimezone=UTC**

其中UTC是统一标准世界时间。

完整的连接字符串示例：**jdbc:mysql://localhost:3306/test?serverTimezone=UTC**

或者还有另一种选择：**jdbc:mysql://127.0.0.1:3306/test****?useUnicode=true&characterEncoding=UTF-8**，这个是解决中文乱码输入问题，当然也可以和上面的一起结合：**jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&\**serverTimezone=UTC\****