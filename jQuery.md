# jQuery入口函数与JavaScript入口函数的区别

jQuery 入口函数:

```js
$(document).ready(function(){
    // 执行代码
});
或者
$(function(){
    // 执行代码
});
```

JavaScript 入口函数:

```js
window.onload = function () {
    // 执行代码
}
```

jQuery 入口函数与 JavaScript 入口函数的区别：

-  jQuery 的入口函数是在 html 所有标签(DOM)都加载之后，就会去执行。
-  JavaScript 的 window.onload 事件是等到所有内容，包括外部图片之类的文件加载完后，才会执行。

# jQuery选择器

![image-20220711160127263](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711160127263.png)

![image-20220711160230833](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220711160230833.png)

# 属性操作

## attr()

### 定义和用法

attr() 方法设置或返回被选元素的属性值。

根据该方法不同的参数，其工作方式也有所差异。

### 返回属性值

返回被选元素的属性值。

```javascript
$(selector).attr(attribute)
```

### 设置属性/值

设置被选元素的属性和值。

### 语法

```
$(selector).attr(attribute,value)
```

# val()

## 定义和用法

val() 方法返回或设置被选元素的 value 属性。

**当用于返回值时：
**该方法返回第一个匹配元素的 value 属性的值。

**当用于设置值时：
**该方法设置所有匹配元素的 value 属性的值。

**注意：**val() 方法通常与 HTML 表单元素一起使用。