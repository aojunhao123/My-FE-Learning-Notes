# 客户端与服务器

## 服务器

上网过程中，负责存放和对外提供资源的电脑，叫做服务器

![image-20220711162354707](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711162354707.png)

## 客户端

上网过程中，负责获取和消费资源的电脑(用户电脑)，叫做客户端

![image-20220711162458183](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711162458183.png)

# URL地址

## 概念

![image-20220711162539152](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711162539152.png)

## 组成部分

![image-20220711162610016](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711162610016.png)

# 客户端与服务器的通信过程

## 图解

![image-20220711163329873](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711163329873.png)

# 服务器对外提供的资源

## 1.网页中常见的资源

文字内容，图片，音频，视频……

## 2.数据

![image-20220711164923062](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711164923062.png)

## 网页中请求数据的方式

![image-20220711165004471](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711165004471.png)

# Ajax是什么

AJAX = *A*synchronous *J*avaScript *A*nd *X*ML.

AJAX 并非编程语言。

AJAX 仅仅组合了：

- 浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）
- JavaScript 和 HTML DOM（显示或使用数据）

## Ajax的核心

Ajax的核心是 XMLHttpRequest

# XMLHttpRequest

所有现代浏览器都支持 XMLHttpRequest 对象。

XMLHttpRequest 对象用于同幕后服务器交换数据。这意味着可以更新网页的部分，而不需要重新加载整个页面。

## 创建 XMLHttpRequest 对象

```js
variable = new XMLHttpRequest();
```

老版本的 Internet Explorer（IE5 和 IE6）使用 ActiveX 对象：

```js
variable = new ActiveXObject("Microsoft.XMLHTTP");
```

## XMLHttpRequest 对象方法

| 方法                                          | 描述                                                         |
| :-------------------------------------------- | :----------------------------------------------------------- |
| new XMLHttpRequest()                          | 创建新的 XMLHttpRequest 对象                                 |
| abort()                                       | 取消当前请求                                                 |
| getAllResponseHeaders()                       | 返回头部信息                                                 |
| getResponseHeader()                           | 返回特定的头部信息                                           |
| open(*method*, *url*, *async*, *user*, *psw*) | 规定请求method：请求类型 GET 或 POSTurl：文件位置async:true（异步）或 false（同步）user：可选的用户名称psw：可选的密码 |
| send()                                        | 将请求发送到服务器，用于 GET 请求                            |
| send(*string*)                                | 将请求发送到服务器，用于 POST 请求                           |
| setRequestHeader()                            | 向要发送的报头添加标签/值对                                  |

## XMLHttpRequest 对象属性

| onreadystatechange | 定义当 readyState 属性发生变化时被调用的函数                 |
| :----------------- | :----------------------------------------------------------- |
| 属性               | 描述                                                         |
| readyState         | 保存 XMLHttpRequest 的状态。0：请求未初始化1：服务器连接已建立2：请求已收到3：正在处理请求4：请求已完成且响应已就绪 |
| responseText       | 以字符串返回响应数据                                         |
| responseXML        | 以 XML 数据返回响应数据                                      |
| status             | 返回请求的状态号200: "OK"403: "Forbidden"404: "Not Found"如需完整列表请访问 [Http 消息参考手册](https://www.w3school.com.cn/tags/html_ref_httpmessages.asp) |
| statusText         | 返回状态文本（比如 "OK" 或 "Not Found"）                     |



![image-20220715144617282](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220715144617282.png)



## 查询字符串

![image-20220715145205500](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220715145205500.png)

# 跨域访问

出于安全原因，现代浏览器不允许跨域访问。

这意味着尝试加载的网页和 XML 文件都必须位于相同服务器上。



# form表单

## 1.表单的功能

表单在网页中主要负责数据采集功能

## 2.表单的组成部分

- 表单标签(<form></form>)
- 表单域(文本框,密码框,单选复选框……)
- 表单按钮

![image-20220713145106576](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220713145106576.png)

## 3.<form>标签的属性

1.action

action属性用来规定当提交表单时,向何处发送表单数据

action属性的值应为后端提供的一个URL地址,这个地址专门负责接收表单提交过来的数据

当表单在未指定action属性值的情况下,action默认值为当前页面的URL地址

**注:当提交表单后,页面会立即跳转到action属性指定的URL地址**

2.target

target属性用来规定在何处打开actionURL

可选值:

![image-20220713151521755](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220713151521755.png)

3.method

method属性用来规定以何种方式把表单数据提交到actionURL

可选值：get(默认值)，post

![image-20220713154326381](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220713154326381.png)

4.enctype

![image-20220713155802441](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220713155802441.png)

# JSON

JavaScript Object notation	 (JavaScript对象表示法)



# 同源策略

## 同源

![image-20220721091558864](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220721091558864.png)

## 同源策略

![image-20220721091825485](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220721091825485.png)

# 跨域

非同源则为跨域

![image-20220721092806028](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220721092806028.png)

# http交互模型

请求/响应模型

# http请求消息/http请求报文

## 组成部分

![image-20220919195126201](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220919195126201.png)

### 请求行

由请求方式,URL,http协议版本三部分组成

### 请求头

用来描述客户端的基本信息,从而告知服务器

![image-20220919193540275](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220919193540275.png)

### 空行

最后一个请求头字段后面是一个空行,告诉服务器请求头部至此结束

### 请求体

存放着要通过*POST方式*提交到服务器的数据

GET**请求没有请求体**

# http响应消息/响应报文

## 组成部分

![image-20220919195202168](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220919195202168.png)

### 状态行

由协议版本,状态码,状态码描述组成

### 响应头

携带服务端描述信息,由键值对组成

### 空行

最后一个响应头字段后面跟着一个空行,告诉客户端响应头至此结束

### 响应体

存放着服务端响应给客户端的资源内容

# http请求方法

![image-20220919201223915](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220919201223915.png)
