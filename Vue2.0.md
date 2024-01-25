# Vue是什么

![image-20220629094659246](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220629094659246.png)

# 模板语法

插值语法

1. 功能: 用于解析标签体内容 
2.  语法: {{xxx}} ，xxxx 会作为 js 表达式解析

指令语法

1. 功能: 解析标签属性、解析标签体内容、绑定事件
2.  举例：v-bind:href = 'xxxx' ，xxxx 会作为 js 表达式被解析

# 数据绑定

## 单向数据绑定

1. 语法：v-bind:href ="xxx" 或简写为 :href
1.  特点：数据只能从data流向页面

## 双向数据绑定

1. 语法：v-mode:value="xxx" 或简写为 v-model="xxx"
1.  特点：数据不仅能从data流向页面，还能从页面流向 data

# MVVM模型

![image-20220702113124928](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220702113124928.png)