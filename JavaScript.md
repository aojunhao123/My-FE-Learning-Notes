

# JavaScript基础

 1.

```javascript
    // 控制浏览器弹出警告框
    alert("hello world!");
    // 让计算机在页面中输出一个内容
    document.write("ybb")
    // 向控制台输出内容
    console.log("Carol")
```

2.

```html
!-- 可以将js代码写在标签的onclick属性中
    当我们点击按钮时，执行js代码
    但不推荐这种写法，改写法属于结构与行为耦合，不便维护-->
    <button onclick="alert('别点我 ')">点一下</button>
    <!-- 也可写在超链接的herf属性中，点击超链接时执行代码 -->
    <a href="javascript:alert('hhh')">点一下</a>
```

## 数据类型

### 基本数据类型

#### 	1.String 字符串

#### 	2.Number 数值

​		1.在js中所有数值都是Number类型,

​			包括整数和浮点数

​		2.js中可以表示的数字最大值

​			Number.MAX_VALUE

​			若Number表示的数字超过最大值,则表示为Infinity

​		3.NaN 是一个特殊的数字，表示 Not a Number

​		4.如果使用js进行浮点数运算，可能得到一个不精确的结			果

​			所以不要使用js进行对精确度要求较高的运算

```js
var a = 0.1+0.2;
    console.log(a)
```

#### 	3.Boolean 布尔值

​		true false

#### 	4.Null 空值

​		Null类型的值只有一个：null

**	注：使用typeof检查null值时，返回object**

#### 	5.Undefined 未定义

​		当声明一个变量，但并不给变量赋值时，它的值就是		undefined

### 引用数据类型

#### 	6.Object 对象

##### 	对象的分类：

###### 	1.内建对象

​		由ES标准中定义的对象，在任何的ES实现中都可以使用

​		例：Math	String	Number	Boolean	Function	      			Object...

###### 	2.宿主对象

​		由JS的运行环境提供的对象，目前来讲主要指由浏览器提		供的对象

​		例：DOM	BOM

###### 	3.自定义对象

​		由开发人员自己创建的对象

##### 属性

在对象中保存的值称为属性

###### 向对象中添加属性

语法：对象.属性名 = 属性值

```js
obj.name = "Carol"
    obj.gender = "female"
    obj.age = "18"
```

属性名： 

对象的属性名不强制要求遵守标识符的规范

但是我们使用时还是尽量按照标识符的规范去做

**注：如果要使用特殊的属性名，不能采用"对象.属性名 = 属性		值"的方法，需要另一种方法**

​		语法：对象["属性名"] = 属性值

​		读取时也要采用该方式

**使用对象["属性名"] = 属性值的方法去操作属性更加灵活**

**在[]中可以直接传递一个变量，这样变量值是多少就会读取那个属性**

例：

```js
 	obj["Diana"] = "小羽毛球"
    var n = "Diana"
    console.log(obj["Diana"])
    console.log(obj[n])
```

控制台输出结果一样

###### 读取对象中的属性

语法：对象.属性名

```javascript
console.log(obj.name)
```

若读取对象中没有的属性，不会报错而是返回undefined

###### 修改对象的属性值

语法：对象.属性名 = 新值

###### 删除对象的属性

delete 对象.属性名



###### 属性值

JS对象的属性值，可以是任意的数据类型，也可以是一个对象

##### 使用对象字面量创建对象

```js
var obj = {}
```

使用对象字面量，可以在创建对象时，直接指定对象中的属性

语法：{属性名:属性值,属性名:属性值......}

例：

```js
var obj = {name:"Ava",age:18,sex:"male"}
```

​	对象字面量的属性名可以加引号也可以不加，建议不加

​	如果要使用一些特殊的名字，则必须加引号

##### 使用工厂方法创建对象

通过该方法可以大批量的创建对象

```js
function creatPerson(name,age,gender){
            // 创建一个新的对象
            var obj = new Object()
            // 向对象中添加属性
            obj.name = name
            obj.age = age
            obj.gender = gender
            obj.sayName = function(){
                alert(this.name)
            }
            // 将新的对象返回
            return obj
        }
```

##### 使用 构造函数 创建对象

构造函数创建时首字母习惯大写

```js
 function Person(){
            name = "孙悟空"
            age = 18
            gender = "男"
        }
```

###### 构造函数与普通函数的区别

调用方式的不同：

​		普通函数是直接调用，而构造函数需要使用**new关键字**来调用

```js
var per = new Person()
```

###### 构造函数执行流程

1. 立即创建一个新的对象
2. 将新建的对象设置为函数中this，在构造函数中可以使用this来引用新建对象
3. 逐行执行函数中的代码
4. 将新建的对象作为返回值返回

使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个类

我们将通过一个构造函数创建的对象，称为是该类的实例



##### 原型(prototype)

###### x function v1() { ... }function v2() { ... }​export {  v1 as streamV1,  v2 as streamV2,  v2 as streamLatestVersion};javascript

我们所创建的每一个函数，解析器都会向函数中添加一个属性prototype;这个属性对应(指向)一个对象，这个对象就是我们所谓的**原型对象**

函数作为普通函数调用prototype没有任何作用

当函数以构造函数的形式调用时，它所创建的对象中都会有一个隐含的属性，该属性指向该构造函数的原型对象，我们可以通过_ _ __proto__ _ _来访问该属性

原型对象相当于一个公共区域，所有同一个类的实例都可以访问到这个原型对象，我们可以将对象中共有的内容统一设置到原型对象中

当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找，如果找到则直接使用

以后我们创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中

**使用in检查对象中是否有某个属性时，如果对象中没有但是原型中有，一样返回true**

###### hasOwnProperty()

可以使用对象的hasOwnProperty()来检查对象自身中是否含有该属性;只有当对象自身中含有属性时，才会返回true

###### 原型链

![image-20220523141243331](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220523141243331.png)





##### 方法(method)

​	当函数作为对象的属性保存，那么我们称该函数是这个对象	的方法;调用函数就是调用对象的方法

​	但只是名称上的区别，与函数没有实际区别

##### 枚举对象中的属性

使用for...in语句

语法：

​		for(var 变量 in 对象){

​		}

对象中有几个对象，循环体就会执行几次

每次执行时，会将对象中的一个属性名赋值给变量



##### 数组(Array)

**数组也是一个对象(属于内建对象)**

它和我们普通对象功能类似，也是用来存储一些值的

不同的是普通对象是使用字符串作为属性名，而数组是使用数字来作为索引操作元素

数组的存储性能比普通对象更好，开发中经常使用数组来储存一些数据

数组中的元素可以是任意的数据类型

使用typeof检查数组返回Object



**创建数组对象**

```js
var arr =  new Array()
```

```js
var arr =  new Array(1,2,3)
```

创建固定长度数组

```js
var arr =  new Array(10)
```

创建只有一个元素的数组

```js
arr = [10]
```

**使用字面量创建数组(更方便)**

使用字面量创建数组时，可以在创建时就指定数组中的元素

```js
        var arr2 = [1,2,3,4]
```

向数组中添加元素

```js
 arr[0] = 10
```

设置(修改)数组长度

```js
arr.length = 10
```

![image-20220523162641844](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220523162641844.png)

向数组最后一个位置添加元素

```js
arr[arr.length] = 99
```

如果读取不存在的索引，不会报错而是返回undefined

###### 数组的方法 

![image-20220524152254028](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524152254028.png)

![image-20220524152547437](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524152547437.png)

![image-20220524152913081](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524152913081.png)

![image-20220524152849311](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524152849311.png)

![image-20220524203236484](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524203236484.png)

![image-20220524203513372](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524203513372.png)

![image-20220524203855507](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524203855507.png)



-----

###### filter()

`filter()` 方法创建数组，其中填充了所有通过测试的数组元素（作为函数提供）。

**注释：**`filter()` 不会对没有值的数组元素执行该函数。

**注释：**`filter()` 不会改变原始数组。

语法

```js
array.filter(function(currentValue, index, arr), thisValue)
```



###### forEach()

![image-20220524155220014](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524155220014.png)

![image-20220524155406969](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524155406969.png)

第一个参数，就是当前正在遍历的数组的元素

第二个参数，就是当前正在遍历的元素的索引

第三个参数，就是正在遍历的数组

实例

列出数组中的每一项：

```js
var fruits = ["apple", "orange", "cherry"];
fruits.forEach(myFunction);

function myFunction(item, index) {
  document.getElementById("demo").innerHTML += index + ":" + item + "<br>"; 
}
```

-------

###### slice()

可以用来从数组中提取指定元素

该方法**不会改变原来的数组**，而是将截取的元素封装到一个新数组中返回

参数：

1. 截取开始位置的索引，包含开始索引

2. 截取结束位置的索引，**不包含**结束索引

   ​	第二个参数可以省略不写，此时会截取从开始索引	往后的所有元素

   索引可以传递一个负值，如果传递一个负值，则从后往前计算

###### splice()

可以用于删除数组中的指定元素

使用splice()**会影响到原数组**，会将指定元素从原数组中删除，并将被删除的元素作为返回值返回

参数：

1. 第一个表示开始位置的索引
2. 第二个表示删除的数量
3. 第三个及之后的参数，可以传递新的元素，这些元素将会自动插入到开始位置索引前面

###### sort()

![image-20220524203945779](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524203945779.png)

若要用sort()对数字进行排序，则需要创建回调函数

```js
  var arr = [11,27,8,9,4,7,32]
        var result = arr.sort()
        arr.sort(function(a,b){
            return a-b
        })
```

回调函数中需要定义两个形参，浏览器将会分别使用数组中的元素作为实参去调用回调函数

![image-20220524212418042](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220524212418042.png)

###### call()和apply()

![image-20220527083148079](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220527083148079.png)

```JavaScript
 function fn(){
      this.age = 18
    }
    var obj = {}
    // obj.fn();  //无法调用，obj中没有fn()方法 
    fn.call(obj)//修改this指向，使obj调用fn()
    console.log(obj.age)
```

**fn.call/apply(obj):临时让fn成为obj的方法进行调用**

##### arguments

在调用函数时，浏览器每次都会传递两个隐含参数

1. 函数的上下文对象this
2. 封装实参的对象arguments

![image-20220527084450269](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220527084450269.png)

它里面有一个属性叫做callee，该属性对应一个函数对象，就是当前正在指向的函数对象

##### Date对象

在JS中使用Date对象来表示一个时间

如果直接使用构造函数创建一个Date对象，则会直接封装为当前代码执行的时间

![image-20220527091235446](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220527091235446.png)

##### 字符串(String)

###### 字符串方法

indexOf()

indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。

**若字符串中不包含要检索的值，则返回值为-1**

```js
stringObject.indexOf(searchvalue,fromindex)
```

| 参数        | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| searchvalue | 必需。规定需检索的字符串值。                                 |
| fromindex   | **可选的**整数参数。规定在字符串中开始检索的位置。它的合法取值是 0 到 stringObject.length - 1。如省略该参数，则将从字符串的首字符开始检索。 |

----

trim()

trim() 方法用于删除字符串的头尾空白符，空白符包括：空格、制表符 tab、换行符等其他空白符等。

trim() 方法不会改变原始字符串。

trim() 方法不适用于 null, undefined, Number 类型。

------

split()



### 包装类

在js中为我们提供了三个包装类，通过这三个包装类可以将基本数据类型的数据转换为对象

#### String()

​	可以将基本数据类型字符串转换为String对象

#### Number()

​	可以将基本数据类型的数字转换为Number对象

#### Boolean()

​	可以将基本数据类型的布尔值转换为Boolean对象

### 内存地址

JS的变量都是保存到栈内存中的

​		基本数据类型的值直接在栈内存中存储;值与值之间是独立		存在，修改一个变量不会影响其他变量

​		对象是保存到堆内存中的，每创建一个新的对象，就会在		堆内存中开辟出一个新的空间;而变量保存的是对象的内存		地址(对象的引用)

当比较两个基本数据类型的值时，直接进行值比较

当比较两个引用数据类型时，比较的是对象的内存地址

如果两个对象是一模一样的，但是地址不同，也会返回false

## 标识符

​	1.在js中所有的可以由我们自主命名的都可以称为标识符

​		例:变量名、函数名、属性名

​	2.标识符可以含有字母、数字、下划线、$

​	3、标识符开头不能是数字

## 强制类型转换  

### 转换成字符串类型

#### 方式一：

​	调用被转换数据类型的toString()方法

​	该方法不会影响到原变量，而是返回转换的结果

​	**注：null和undefined这两个值无toString()方法**

#### 方式二：

​	调用String()函数，并将被转换的数据作为参数传递给函数

​	使用String()函数做强制类型转换时，对于Number和Boolean实际上就是调用的toString()方法

​	但是对于null和undefined，就不会调用toString()方法

​	它会将null直接转换为"null",将undefined直接转换为"undefined"

### 转换成数值类型

#### 方式一

调用Number()函数

#### 方式二

该方式专门针对字符串类型

调用parseInt()函数

parseInt()可以将字符串中的有效整数内容取出，然后转换成Number

调用parseFloat()函数，可以获取有效小数

### 转换成布尔类型

​	调用Boolean()函数

​	数字-->布尔

​	除了0和NaN,其余的都是true

​	字符串-->布尔

​	除了空串，其余的都是true

​	null和undefined都会转换为false

​	对象也会转换为true

## 运算符

​	运算符也叫操作符，通过运算可以对一个或多个值进行运算，并获取	运算结果

### typeof

​	检查变量类型

​	语法: typeof 变量

注:使用typeof返回数据类型的**字符串**表达

**typeof不能判断array与object	null与object**

### instanceof

​	判断对象的具体类型,返回true或false

### in运算符

​	通过该运算符可以检查一个对象中是否含有指定的属性

​	如果有则返回true，没有则返回false

​	语法：

​	"属性名"	in	对象

### 算数运算符

​	当对非Number类型的值进行运算时，会将这些值转换为Number后运算

​	任何值与NaN运算后的结果都是NaN

​	两个字符串相加后会拼接为一个字符串后返回

​	**任何值与字符串相加都会转换成字符串后拼串**，可以利用该特点将任意数据类型+""转换为String类型。这是一种隐式的类型转换。

​	**除了与字符串相加的运算以外，其余与字符串的运算都会先将字符串转换成数值类型后再进行运算**

**任何值做- / *运算都会自动转换为Number**

我们可以利用该特点做隐式类型转换

### 一元运算符

​	+      - 

​	只需要一个操作数

#### 自增自减

i++和++i的区别

i++先用后加;++i先加后用

i--和--i同理

### 逻辑运算符

#### 1.! 非

若对非布尔值进行非运算，则会将其转换为布尔值，然后再取反

原理和Boolean()函数一样

#### 2.&& 与

js的与属于短路与，第一个值为false则不会看第二个值而直接返回结果



#### 3.|| 或

短路或，第一个值为true则不会检查第二个值

### &&   ||非布尔值的情况

​	对于非布尔值进行与或运算时，会先将其转换为布尔值，再进行运算，并返回原值

#### 	与运算

如果两个值都为true，则返回后面的值

```js
var result = 5 && 6
```

如果两个值中有false，则返回靠前的false

#### 	或运算

如果第一个值为true，则直接返回第一个值

如果第一个值为false，则直接返回第二个值

### 赋值运算符

​	=	+=	-=	*=	%=	/=

### 关系运算符

​	>	<	>=	<=

​	若关系成立，返回true，不成立返回false

#### 	非数值情况

​	对于非数值情况，会先将其转换成数值后再运算

​	如果符号两侧的值都是字符串，不会将其转换成数字后进行比较，

​	而会分别比较字符串中字符的Unicode编码

​	比较字符编码时是一位一位进行比较

​	如果两位一样，则比较下一位

```js
console.log("abc"<"b");//true
```

​	若比较的两个字符串型的数字，可能得到错误的结果

​	**所以一定要转型后再比较**

```js
console.log("123"<"5");//true
```

### Unicode编码

​	在字符串中使用转义字符输入Unicode编码

​	\u四位编码

```js
console.log("\u2620");
```

 	在网页中使用Unicode编码

​	&#编码;这里的编码需要十进制

```html
<p>&#9760;</p>
```

### 相等运算符

​	**==**	**!=**	**===	!==**

​	用来比较两个值是否相等，若相等返回true，否则返回false

​	若两个值的类型不同，则会自动进行类型转换后再运算

​	**undefined 衍生自null，所以这两个值做相等判断会返回true**

```js
console.log(undefined == null);//true
```

​	**NaN不和任何值相等，包括它本身**

```js
console.log(NaN == NaN);//false
```

​	通过isNaN()函数来判断一个值是否为NaN

#### ===	全等

​	用来判断两个值是否全等，和相等类似，不同的是它不会做自动类型转换;如果两个值的类型不同，则直接返回false

```js
console.log(undefined === null);//false
```



#### !==	不全等

​	用法规则和全等类似

### 条件运算符

​	也叫三元运算符

​	语法：条件表达式?语句1:语句2;

​	执行流程:

​		如果该值为true,则执行语句1,并返回执行结果

​		如果该值为false,执行语句2,并返回执行结果

#### 运算符优先级

<img src="C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220513102933054.png" alt="image-20220513102933054" style="zoom: 50%;" />

可以使用()改变运算符优先级

```js
var result = (1 || 2) && 3;
```

## 流程控制语句

### 	1.条件判断语句

​		语法1:

​			if(条件表达式)

​				语句

​		语法2:

​			if(条件表达式){

​					语句...}

​			else{

​					语句...}

​		语法3:

​			if(条件表达式){

​					语句...}

​			else if(条件表达式){

​					语句...}

### 	2.条件分支语句

​	switch语句

​	语法：switch(条件表达式){

​						case 表达式:

​									语句...

​									break;

​						case 表达式:

​									语句...

​									break;

​						default:

​									语句...

​									break;

}

执行流程：

​	switch...case...语句

​	在执行时会依次将case后的表达式的值与switch后的条件表	达式的值进行**全等比较**;

​	若比较结果为true，则从当前case处开始执行代码;当前case	后的所有代码都会被执行，我们可以在case后加一个break关	键字，这样可以确保只会执行当前case后的语句

​	若比较结果为false，则继续向下比较

​	若所有比较结果都为false，则执行default后的语句			

### 	3.循环语句

#### 	while循环

​			语法：

​				while(条件表达式){

​					语句...

​				}

​	while语句在执行时，先对条件表达式进行求值判断，

 	如果值为true，则执行循环体

​		循环体执行完毕后，继续对表达式进行判断

​		若为true，则继续执行循环体，以此类推

​	如果值为false，则终止循环

#### 	do...while循环

​			语法：

​					do{

​							语句...

​					}while(条件表达式)

​			do...while会先执行一次语句再判断是否循环

#### 	for循环

​			语法:

​					for(初始化表达式;条件表达式;更新表达式){

​							语句...

​					}

​					执行流程：

1. 执行初始化表达式，将变量初始化

2. 执行条件表达式，判断是否执行循环

   若为true，执行循环

   若为false，终止循环

​			3.执行更新表达式，执行完毕继续重复第二步

### label标识

可以为循环语句创建一个label，来标识当前循环

label:循环语句



## 函数

函数也是一个对象

函数中可以封装一些功能(代码),在需要时可以执行这些功能(代码)

函数中可以保存一些代码在需要的时候调用

使用typeof检查一个函数对象时，会返回function

### 创建函数

#### 使用	构造函数	创建函数

```js
var fun = new Function()
```

可以将要封装的代码以字符串形式传递给构造函数

例：

```js
 var fun = new Function("console.log('Hello 这是我的第一个函数')")
```

封装到函数中的代码不会立即执行，而是在函数被调用的时候执行

**注：实际开发中很少使用构造函数来创建一个函数对象**

#### 使用	**函数声明**	创建函数

​	语法：

​			function	函数名(形参1，形参2...形参n){

​						语句...

​			}

```js
function fun(){
        console.log("abc")
        alert("def")
        document.write("ghi")
    }
```

#### 使用	函数表达式	创建函数

```js
 var fun = function(){
        console.log("匿名函数")
    } 
```

### 调用函数

​	调用函数时

1. 解析器不会检查实参的类型，所以要注意，是否有可能接收到非法参数，如果有可能则需要对参数进行类型检查

2. 解析器不会检查实参的数量，多余实参不会被赋值;若实参的数量少于形参的数量，则没有对应实参的形参将是undefined

语法：函数对象()

当函数调用时，函数中封装的代码会按顺序执行

### 立即执行函数

```js
( function(){
        console.log("这是一个匿名函数")
    })();
   ( function(a,b){
        console.log("a = "+a)
        console.log("b = "+b)
    })(3,4)
```

控制台输出结果：

​					<img src="C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220520151936865.png" alt="image-20220520151936865"  />

立即执行函数

​	函数定义完，立即被调用， 这种函数叫做立即执行函数

​	这种函数往往只会执行一次

### 形参与实参

可以在函数的()中指定一个或多个形参，多个形参用,隔开，声明形参就相当于在函数内部声明了对应的变量，但并不赋值

调用函数时，可以在()中指定实参，实参将会赋值给函数中对应的形参

**另：函数的实参可以是任意的数据类型，可以是对象，也		可以是函数**

```js
 function sum(a,b){
      console.log(a+"+"+b+"="+(a+b))
    }
    sum(2,3)
```

```js
 	function fun(a){
       	 console.log("a = "+a)
    	}
     // 将匿名函数作为实参传递给另一个函数
    fun(function(){alert("hello")})
```

#### 实参为fun()与实参为fun的区别

```js
 function fun(a){
        console.log("a = "+a)
    }
 function fun2(b){
        console.log(b)
    }
```

实参为fun()

```js
fun2(fun())
```

控制台输出结果：

a = undefined
undefined

实参为fun

```js
 fun2(fun)
```

控制台输出结果：

fun(a){
        console.log("a = "+a)
    }



区别:

​		fun()	调用函数；相当于使用函数的返回值

​		fun	函数对象，相当于直接使用函数对象



### 函数返回值

可以使用return 来设置函数返回值

语法：

​		return	值

return后的值将会作为函数的执行结果返回;可以定义一个变量来接收该结果

如果return语句后不跟任何值就相当于返回一个undefined

如果函数中不写return，则也会返回undefined

return后可以跟任意类型的值 

**return后的语句将不会被执行**



### 函数对象的方法





### this

解析器在调用函数时每次都会向函数内部传递一个隐含的参数

这个隐含的参数就是this.this指向的是一个对象

这个对象我们称为函数执行的上下文对象

根据函数的调用方式的不同，this会指向不同的对象

1. 以函数的形式调用时，this永远都是window
2. 以方法的形式调用时，this就是调用方法的那个对象

注：以上两种情况本质一样，谁调用的this就指向谁



### prompt()

prompt()弹出一个提示框,提示框中有一个文本框,可输入字符串作为参数,该参数会作为提示框的提示文字

### parseInt()函数

​		语法：parseInt(string, radix)

parseInt() 函数可解析一个字符串，并返回一个整数。

当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

### console.time()

可以用来开启一个计时器

该函数需要一个字符串作为参数，这个字符串将会作为计时器的标识

### console.timeEnd()

用来停止一个计时器，需要一个计时器的名字作为参数

### Math.sqrt()

开方函数

### toString()

当我们直接在页面中打印一个对象时，实际上是输出对象的toString()方法的返回值

如果我们希望在输出对象时不输出[object Object],可以重写该方法

## 关键字

### break

break关键字可以用来退出switch或循环语句

break关键字，会立即终止离它最近的循环

使用break语句时，可在break后跟着一个label，

这样break将会结束指定的循环，而不是最近的

**注：不能用于if语句**

### continue

终止当次循环

默认只会对离它最近的循环起作用

使用continue语句时，可在continue后跟着一个label，

这样continue将会结束指定的循环，而不是最近的

## 作用域

作用域指一个变量的作用范围

在JS中一共有两种作用域

### 1.全局作用域

直接编写在script标签中的JS代码，都在全局作用域

全局作用域在页面打开时创建，在页面关闭时销毁

全局作用域中有一个全局对象window

它由浏览器创建，代表的是一个浏览器的窗口，我们可以直	接使用

在全局作用域中：

​	创建的变量都会作为window对象的属性保存

​	创建的函数都会作为window对象的方法保存

全局作用域中的变量都是全局变量;在页面的任意部分都可以访问到

### 2.函数作用域

调用函数时创建函数作用域，函数执行完毕后，函数作用域销毁

每调用一次函数 就会创建一个新的函数作用域，它们之间是互相独立的

**全局作用域中无法访问函数作用域中的变量**

当在函数作用域中操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用;如果没有则向上一级作用域寻找，直到全局作用域。

若全局作用域中仍未找到，则报错ReferenceError

在函数作用域中若想直接访问全局变量，可以使用window对象

**注：在函数中，不使用var声明的变量都会成为全局变量**

​		**定义形参就相当于在函数作用域中声明了变量**



###  函数的声明提前

使用函数声明形式创建的函数	function	函数(){}

它会在所有的代码执行之前就被创建，所以我们可以在函数声明前来调用函数

使用函数表达式创建的函数，不会被声明提前，所以不能在函数声明前调用

## 垃圾回收(GC)

程序运行过程中会产生垃圾，垃圾积攒过多会导致程序运行速度变慢

垃圾回收机制可以处理程序运行中产生的垃圾

### 垃圾

当一个对象**没有任何变量或属性对它进行引用**，此时我们将永远无法操作该对象。此时该对象就是一个垃圾，垃圾过多会占用大量内存空间

![image-20220523144319817](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220523144319817.png)

### 自动垃圾回收机制

![image-20220523152005839](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220523152005839.png)

## 正则表达式(RegExp)

### 语法：

```js
var patt=new RegExp(pattern,modifiers);
```

或者更简单的方式:

```js
var patt=/pattern/modifiers;
```



- pattern（模式） 描述了表达式的模式
- modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配

### 量词

通过量词可以设置一个内容出现的次数

量词只对它前边的一个内容起作用

<img src="C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220529224932104.png" alt="image-20220529224932104"  />

若要在正则表达式中同时使用^和$,则要求字符串必须完全符合正则表达式

```js
reg = /^a$/;
console.log(reg.test("a"))
```

此时控制台输出为true

## 变量

### 变量的声明提前

使用var关键字声明的变量，会在所有的代码执行之前被声明(但是不会赋值)

但是如果声明变量时不使用var关键字，则变量不会被声明提前

**注：函数作用域中的变量也有声明提前的特性**

### 重复声明 JavaScript 变量

如果再次声明某个 JavaScript 变量，将不会丢失它的值。

```js
var carName = "porsche";
var carName; 
```

carName的值不变

## DOM

### 什么是DOM

![image-20220530140255617](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530140255617.png)

*HTML DOM 是关于如何获取、修改、添加或删除 HTML 元素的标准。*

### 模型

![image-20220530140541530](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530140541530.png)

### 节点

![image-20220530140846711](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530140846711.png)

![image-20220530140948880](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530140948880.png)

![image-20220530141122077](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530141122077.png)

### 事件

![image-20220530142234800](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530142234800.png)

![image-20220530143531189](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530143531189.png)

![image-20220530150133761](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530150133761.png)

### 获取元素节点

![image-20220530150607369](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220530150607369.png)

getElementsByTagName()，getElementsByName() 会返回*一个类数组对象，所有查询到的元素都会封装到对象中*

### 获取元素节点的子节点

![image-20220531225759013](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220531225759013.png)

### Element对象

在 HTML DOM 中，Element 对象表示 HTML 元素。

Element 对象可以拥有类型为元素节点、文本节点、注释节点的子节点。

#### childNodes

childNodes 属性返回节点的子节点集合，以 NodeList 对象。

childNodes属性会获取包括文本节点在呢的所有节点
根据DOM标签，标签间空白也会当成文本节点
注意:在IE8及以下的浏览器中，不会将空白文本当成子节点， 所以该属性在IE8中会返回4个子元素而其他浏览器是9个

#### children

children属性可以获取当前元素的所有子元素

#### firstChild

firstChild可以获取到当前元素的第一个子节点(包括空白文本节点）

#### firstElementChild

获取当前元素的第一个子元素

注：firstElementChild不支持工E8及以下的浏览器，如果需要兼容他们尽量不要使用

#### clientWidth和clientHeight

这两个属性可以获取元素的可见宽度和高度

获得的属性不带单位而是数值,可以直接进行计算

会获取元素宽度和高度,包括内容区和内边距

这些属性只读,不可修改

#### offsetWidth和offsetHeight

获取元素整体的宽度和高度,包括内容区、内边距和边框

#### offsetParent

用来获取元素的定位父元素

会获取到离当前元素最近的开启了定位的祖先元素

若所有祖先元素都未开启定位，则返回body

#### offsetLeft和offsetTop

offsetLeft	获取当前元素相对于其定位父元素的水平**偏移量**

offsetTop	获取当前元素相对于其定位父元素的垂直**偏移量**

#### scrollHeight和scrollWidth

可以获取元素整个滚动区域的宽度和高度

**注：当满足scrollHeight - scrollTop == clientHeight，说明		垂直滚动条滚动到底了**

​		**当满足scrollWidth - scrollLeft == clientWidth，说明		水平滚动条滚动到底了**



#### scrollLeft和scrollTop

获取水平滚动条和垂直滚动条的距离

### Event对象

当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数

在事件对象中封装了当前事件相关的一切信息；

例如:鼠标的坐标	键盘哪个按键被按下……

😅**注：在IE8中，响应函数被触发时，浏览器不会传递事件对			象，而是将事件对象作为window对象的属性保存的**

#### clientX,clientY

用于获取鼠标当前**可见窗口**的坐标

#### pageX,pageY

获取元素相对于当前页面的坐标

#### 事件的冒泡(Bubble)

所谓的冒泡指的是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的**相同(同类型)事件**也会被触发

在开发中大部分情况下，冒泡都是有用的，如果不希望发生事件冒泡可以通过事件对象来取消冒泡

取消冒泡

可以将事件对象的cancelBubble设置为true，即可取消冒泡

#### 事件的委派

指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件被触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件

事件委派利用了冒泡，通过委派可以减少事件绑定的次数，提高程序性能

#### 事件的传播

![image-20220609155039032](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220609155039032.png)

#### target

event中的target属性返回触发事件的元素。

#### addEventListener()

该方法也可为元素绑定响应函数

参数:

1.事件的字符串，不用加"on"

2.回调函数，当事件触发时该函数会被调用

3.是否在捕获阶段触发事件，需要一个布尔值，一般都传false

使用该方法可以同时为一个元素的相同事件同时绑定多个响应函数，这样当事件被触发时，响应函数将会按照函数的绑定顺序执行

### Document对象

#### document.querySelector()

需要一个选择器的字符串作为参数，可以根据一个css选择器来查询一个元素节点对象

**注：使用该方法只会返回一个元素，如果满足条件的元素有多个，那么它只会返回第一个**

#### document.querySelectorAll()

该方法和querySelector()用法类似，不同的是它会将符合条件的元素封装成数组返回

### 通过JS修改元素的样式

语法:元素.style.样式名 = 样式值

**通过style属性设置的样式都是内联样式**

**通过style属性来修改元素的样式，每修改一个样式，浏览器就需要重新渲染一次页面，这样的执行的性能是比较差的;而且当我们要修改多个样式时，也不太方便**

我们希望一行代码可以同时修改多个样式



### 读取元素的样式

1.语法:	元素.style.样式名

**注:通过style属性设置和读取的都是内联样式**

2.元素.currentStyle.样式名

它可以用来读取元素当前显示的样式;若元素当前未设置样式,则获取默认样式

**注:currentStyle只有IE浏览器支持**

其他浏览器可以使用

​	getComputedStyle()方法来获取元素当前样式

需要两个参数

​	1.要获取样式的元素

​	2.可以传递一个伪元素,一般传null

该方法会返回一个对象,对象中封装了当前元素对应的样式

可以通过对象.对象名来读取样式

```js
alert(getComputedStyle(box1, null).width);
```

## BOM

浏览器对象模型

BOM可以使我们通过js来操作浏览器

在BOM中为我们提供了一组对象，用来完成浏览器的操作

BOM对象

### 	Window

​		代表的是整个浏览器的窗口，同时Window也是网页中		的全局对象

#### setInterval()

定时调用

可以将一个函数每隔一段时间执行一次

参数：

​	1.回调函数

​	2.每次调用的时间间隔(ms)

返回值：

​	返回一个Number类型的数据

​	这个数字用来作为定时器的唯一标识

#### clearInterval()

​	该方法可以用来关闭一个定时器

​	方法中需要一个定时器标识作为参数，这样将关闭标识对	应的定时器

​	该方法可以接受任意参数

​	若参数是一个有效的定时器标识，则停止对应的定时器;

​	若参数标识一个有效的标识，则什么也不做

#### setTimeout()

​	延时调用：

​	延时调用一个函数，不马上执行，而是隔一段时间后再执	行，且**只会执行一次**

​	延时调用和定时调用的区别：

​	定时调用会执行多次，而延时调用只会执行一次

#### clearTimeout()

关闭一个延时调用

### 	Navigator

​		代表当前浏览器的信息，通过该对象可以来识别不同浏		览器

​		由于历史原因，Navigator对象中的大部分属性都已不能		帮助识别浏览器，一般我们使用userAgent来判断浏览器		的信息

​		userAgent是一个字符串，这个字符串中包含有用来描述		浏览器信息的内容

### 	Location

​		代表当前浏览器的地址栏信息，通过Location可以获取		地址栏信息，或者操作浏览器跳转页面

### 	History

​		代表浏览器的历史记录，可以通过该对象来操作浏览器		的历史记录

​			由于隐私原因，该对象不能获取到具体的历史记录，			只能操作浏览器向前或向后翻页

​			**且该操作只在当次访问有效**

### 	Screen

​		代表用户的屏幕的信息，通过该对象可以获取用户的显		示器相关信息

## JSON

JSON就是一个特殊格式的字符串，这个字符串可以被任意语言所识别;并且可以转换为任意语言中的对象,JSON在开发中主要用来数据的交互

JSON:	JavaScript Object Notation(JS对象表示法)

JSON和JS对象的格式一样，但是JSON字符串中的属性名必须加双引号，其它和js语法一致

JSON分类：

​		1.对象{}

```json
    var obj = '{"name":"星瞳official","age":18,"gender":"女"}';
```

​		2.数组[]

```json
    var arr = '[1,2,3,4,"hello",true]';
```

JSON中允许的值:

1.字符串	2.数值	3.布尔值

4.null	5.对象	6.数组

将JSON字符串转换为JS中的对象

在JS中，为我们提供了一个工具类:JSON

这个类可以帮助我们将JSON转换为js对象，也可以将js对象转换为JSON

JSON --> js对象

```js
var json = '{"name":"星瞳official","age":18,"gender":"女"}';
    // json -- >js对象
    /* 
    JSON.parse()
        可以将JSON字符串转换为js对象
        它需要一个JSON字符串作为参数,会将该字符串转换为js对象
    */
   var o = JSON.parse(json);
```

js对象 --> JSON

```js
 /* 
   js对象 --> JSON
    JSON.stringify()
        可以将一个js对象转换为JSON字符串
        需要一个js对象作为参数,会返回一个JSON字符串
   */
    var obj2 = { name: "taffy", age: 108, gender: "女" };
    var str = JSON.stringify(obj2);
```

# JavaScript高级

## 几个问题

### 1.undefined与null的区别

*undefined是定义了但未赋值的变量

*null是定义了也赋值了的变量,但赋的值为null

### 2.什么时候给变量赋值为null?

*初始赋值，表明将要赋值为对象

*结束前赋值，让对象成为垃圾对象(被垃圾回收器回收)

3.严格区别变量类型与数据类型

数据的类型

*基本类型

*对象类型

变量的类型(变量内存值的类型)

基本类型:保存的就是基本类型的数据

引用类型:保存的是地址值

### 3.JS引擎如何管理内存?

​	1.内存生命周期

​		分配小内存空间,得到它的使用权

​		存储数据,可以反复进行操作

​		释放小内存

​	2.释放内存

​		局部变量:函数执行完自动释放

​		对象:成为垃圾对象==>垃圾回收器回收

### 4.什么时候必须使用对象["属性名"]的方式？

1.属性名包含特殊字符:	-  空格

2.变量名不确定

例:

对象.属性名

```js
 	var propName = "myAge"
 	var p = {}
  	var value = 18
    p.propName = value
	console.log(p)
```

控制台输出：![image-20220623193148557](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220623193148557.png)

此方法不能得到想要的结果



对象["属性名"]

```js
var propName = "myAge"
var p = {}
var value = 18
p[propName] = value
console.log(p)
```

控制台输出：

![image-20220623194241422](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220623194241422.png)

### 5.什么函数是回调函数?

​	1.你定义的

​	2.你没有调

​	3.但最终它执行了

常见的回调函数:

1.DOM事件回调函数

2.定时器回调函数

3.Ajax请求回调函数

4.生命周期回调函数

### 6.this是什么?

1. 任何函数本质上都是通过某个对象来调用的，如果没有直接指定就是window
2. 所有函数内部都有一个变量this
3. 它的值是调用函数的当前对象

7.如何确定this的值？

1. test():window
2. p.test():p
3. new test():新建的对象
4. p.call(obj):obj

## IIFE

全称:	Immediately-Invoked	Function	Expression(立即执行函数)

作用:

*隐藏实现

*不会污染外部(全局)命名空间 

## 原型

1.函数的prototype属性

- 每个函数都有一个prototype属性,它默认指向一个Object空对象(原型对象)
- 原型对象中有一个属性constructor,它指向函数对象

2.给原型对象添加属性(一般都是方法)

- 作用:函数的所有实例对象自动拥有函数中的属性(方法)

### 显式原型和隐式原型

1. 每个函数都function都有一个prototype,即显示原型(属性)

2. 每个实例对象都有一个__ _proto_ _ _,可称为隐式原型(属性)

3. 对象的隐式原型的值为其对应构造函数的显示原型的值

4. 内存结构

   ![image-20220627142739307](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220627142739307.png)

5. 总结

   - 函数的prototype属性:在定义函数时自动添加,默认值是一个空Object对象
   - 对象的_ __proto_ _ _属性:创建对象时自动添加,默认值为构造函数的prototype属性值
   - ES6之前程序员不能直接操作隐式原型,但能直接操作显示原型

###  原型链

1.函数的显式原型指向的对象默认是Object空对象(不包括Object)

2.所有函数都是Function的实例(包括Function本身)

3.Object的原型对象是原型链尽头

```js
Object.prototype.__proto__ === null
```

## instanceof

1.instanceof是如何判断的?

​		表达式:A	instanceof	B

​		若B对象的显式原型对象在A对象的原型		链上,返回true,否则返回false

2.Function是通过new自己产生的实例

## 变量提升与函数提升

 

变量声明提升

通过var定义(声明)的变量,在定义语句之前就可以访问到值:undefined

函数声明提升

通过function声明的函数,在之前就可以直接调用值:函数定义(对象)

## 执行上下文

1. 代码分类(位置)
   - 全局代码
   - 函数(局部)代码
2. 全局执行上下文
   - 在执行全局代码前将window确定为全局执行上下文
   - 对全局数据进行预处理
     - var定义的全局变量==>undefined,**添加为window属性**
     - function声明的全局函数==>赋值(fun),添加为window的方法
     - this==>赋值(window)
   - 开始执行全局代码

## 作用域链

**注:作用域在函数定义时就确定好了**

1. 理解
  * 多个上下级关系的作用域形成的链, 它的方向是从下向上的(从内到外)
  * 查找变量时就是沿着作用域链来查找的
2. 查找一个变量的查找规则
  * 在当前作用域下的执行上下文中查找对应的属性, 如果有直接返回, 否则进入2
  * 在上一级作用域的执行上下文中查找对应的属性, 如果有直接返回, 否则进入3
  * 再次执行2的相同操作, 直到全局作用域, 如果还找不到就抛出找不到的异常

## 作用域与执行上下文

1. 区别1
  * 全局作用域之外，每个函数都会创建自己的作用域，作用域在函数定义时就已经确定了。而不是在函数调用时
  * 全局执行上下文环境是在全局作用域确定之后, js代码马上执行之前创建
  * 函数执行上下文环境是在调用函数时, 函数体代码执行之前创建
2. 区别2
  * 作用域是静态的, 只要函数定义好了就一直存在, 且不会再变化
  * 上下文环境是动态的, 调用函数时创建, 函数调用结束时上下文环境就会被释放
3. 联系
  * 上下文环境(对象)是从属于所在的作用域
  * 全局上下文环境==>全局作用域
  * 函数上下文环境==>对应的函数使用域

## 闭包

1. 如何产生闭包?
  * 当一个嵌套的内部(子)函数引用了嵌套的外部(父)函数的变量(函数)时, 就产生了闭包
2. 闭包到底是什么?
  * 使用chrome调试查看
  * 理解一: 闭包是嵌套的内部函数(绝大部分人)
  * 理解二: 包含被引用变量(函数)的对象(极少数人)
  * 注意: 闭包存在于嵌套的内部函数中
3. 产生闭包的条件?
  * 函数嵌套
  * 内部函数引用了外部函数的数据(变量/函数)

### 闭包的生命周期

产生：在嵌套内部函数定义执行完时闭包就产生

死亡：在嵌套的内部函数成为垃圾对象时

### 闭包的缺点及解决

1.缺点

- 函数执行完后,函数内部的局部变量没有释放
- 容易造成内存泄露

2.解决

- 能不用闭包就不用
- 及时释放

```js
 function fn1() {
    var a = 2;

    function fn2() {
      a++;
      console.log(a);
    }

    return fn2;
  }
  var f = fn1();

  f(); // 3
  f(); // 4

  f = null // 让内部函数成为垃圾对象-->回收闭包
```

### 内存溢出与内存泄露

1.内存溢出

- 一种程序运行出现的错误
- 当程序运行需要的内存超过了剩余的内存时,就抛出内存溢出的错误

2.内存泄露

- 占用的内存没有及时释放

- 内存泄露积累多了就容易导致内存溢出

- 常见的内存泄露:

  - 意外的全局变量

    ```js
    function fn(){
    a = 3//实际上是全局变量	console.log(a)
    }
    ```

  - 没有及时清理的定时器或回调函数

  - 闭包

## JS模块

*具有特定功能的js文件

*将所有的数据和功能都封装在一个函数内部(私有的)

*只向外暴露一个包含n个方法的对象或函数

*模块的使用者,只需要通过模块暴露的对象调用方法来实现对应的功能

## 对象创建模式

1. Object构造函数模式

   ```js
    /*
     一个人: name:"Tom", age: 12
      */
     // 先创建空Object对象
     var p = new Object()
     p = {} //此时内部数据是不确定的
     // 再动态添加属性/方法
     p.name = 'Tom'
     p.age = 12
     p.setName = function (name) {
       this.name = name
     }
   ```

   - 适用场景:起始时不确定对象内部数据
   - 问题:语句太多

2. 对象字面量模式

   - 使用{}创建对象,同时指定属性/方法
   - 适用场景:起始时对象内部数据是确定的
   - 问题:如果创建多个对象,有重复代码

3. 工厂模式

   - 通过工厂函数动态创建对象并返回
   - 适用场景:需要创建多个对象
   - 问题:对象没有一个具体的类型,都是Object类型

   ```js
    function createPerson(name, age) { //返回一个对象的函数===>工厂函数
       var obj = {
         name: name,
         age: age,
         setName: function (name) {
           this.name = name
         }
       }
   
       return obj
     }
   
     // 创建2个人
     var p1 = createPerson('Tom', 12)
     var p2 = createPerson('Bob', 13)
   
   ```

   4.*自定义构造函数模式*

   ** 套路: 自定义构造函数, 通过new创建对象*

    ** 适用场景: 需要创建多个类型确定的对象*

    ** 问题: 每个对象都有相同的数据, 浪费内存*

```js
//定义类型
  function Person(name, age) {
    this.name = name
    this.age = age
    this.setName = function (name) {
      this.name = name
    }
  }
  var p1 = new Person('Tom', 12)
```

​				5.构造函数+原型的组合模式

​						** 套路: 自定义构造函数, 属性							在函数中初始化, 方法添加到							原型上*

 						** 适用场景: 需要创建多个类							型确定的对象*

```js
 function Person(name, age) { //在构造函数中只初始化一般函数
    this.name = name
    this.age = age
  }
  Person.prototype.setName = function (name) {
    this.name = name
  }

  var p1 = new Person('Tom', 23)
  var p2 = new Person('Jack', 24)
  console.log(p1, p2)

```

## 继承模式

### 原型链继承

1. 套路
    1. 定义父类型构造函数
    2. 给父类型的原型添加方法
    3. 定义子类型的构造函数
    4. 创建父类型的对象赋值给子类型的原型
    5. 将子类型原型的构造属性设置为子类型
    6. 给子类型原型添加方法
    7. 创建子类型的对象: 可以调用父类型的方法
  2. 关键
        1. 子类型的原型为父类型的一个实例对象

```js
//父类型
  function Supper() {
    this.supProp = 'Supper property'
  }
  Supper.prototype.showSupperProp = function () {
    console.log(this.supProp)
  }

  //子类型
  function Sub() {
    this.subProp = 'Sub property'
  }

  // 子类型的原型为父类型的一个实例对象
  Sub.prototype = new Supper()
  // 让子类型的原型的constructor指向子类型
  Sub.prototype.constructor = Sub
  Sub.prototype.showSubProp = function () {
    console.log(this.subProp)
  }

  var sub = new Sub()
  sub.showSupperProp()
  // sub.toString()
  sub.showSubProp()

  console.log(sub)  // Sub
```



###  借用构造函数继承(假的)

1. 套路:
  1. 定义父类型构造函数
  2. 定义子类型构造函数
  3. 在子类型构造函数中调用父类型构造
2. 关键:
  1. 在子类型构造函数中通用call()调用父类型构造函数

```js
 function Person(name, age) {
    this.name = name
    this.age = age
  }
  function Student(name, age, price) {
    Person.call(this, name, age)  // 相当于: this.Person(name, age)
    /*this.name = name
    this.age = age*/
    this.price = price
  }

  var s = new Student('Tom', 20, 14000)
  console.log(s.name, s.age, s.price)
```

### 原型链+借用构造函数的组合继承

*1. 利用原型链实现对父类型对象的方法继承*

*2. 利用super()借用父类型构建函数初始化相同属性*

```js
function Person(name, age) {
    this.name = name
    this.age = age
  }
  Person.prototype.setName = function (name) {
    this.name = name
  }

  function Student(name, age, price) {
    Person.call(this, name, age)  // 为了得到属性
    this.price = price
  }
  Student.prototype = new Person() // 为了能看到父类型的方法
  Student.prototype.constructor = Student //修正constructor属性
  Student.prototype.setPrice = function (price) {
    this.price = price
  }

  var s = new Student('Tom', 24, 15000)
  s.setName('Bob')
  s.setPrice(16000)
  console.log(s.name, s.age, s.price)
```

## 进程与线程

### 进程

程序的一次执行,它占用一片独有的内存空间

可以通过任务管理器查看进程

### 线程

是进程内的一个独立执行单元

是程序执行的一个完整流程

是CPU最小的调度单元

![image-20220701160838971](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220701160838971.png)

**相关知识**

![image-20220701161944667](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220701161944667.png)

## 事件循环模型

1.所有代码分类

初始化执行代码(同步代码):包含绑定DOM事件监听,设置定时器,发送Ajax请求

回调执行代码(异步代码):处理回调逻辑

2.js引擎执行代码的基本流程:

初始化代码==>回调代码

3.模型的两个重要组成部分

​		事件管理模块

​		回调队列

4.模型的运转流程

执行初始化代码,将事件回调函数交给对应模块管理

当事件发生时,管理模块会将回调函数及其数据添加到回调队列中

只有当初始化代码执行完后(可能要一定时间),才会遍历读取回调队列中的回调函数执行



## JavaScript 严格模式

JavaScript 严格模式（strict mode）即在严格的条件下运行。

### 使用 "use strict" 指令

😅"use strict" 指令只允许出现在脚本或函数的开头。

"use strict" 的目的是指定代码在严格条件下执行。

严格模式下你不能使用未声明的变量。
