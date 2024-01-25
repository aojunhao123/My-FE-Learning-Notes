# TypeScript

`核心要领`:**定义**任何类型的时候要注明类型;**调用**任何东西的时候要检查类型

## 基础类型

### 布尔值

```typescript
let isDone:boolean = false
```

### 数字

除了支持十进制和十六进制字面量，TypeScript还支持ECMAScript 2015中引入的二进制和八进制字面量。

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

### 字符串

```typescript
let name: string = "bob";
name = "smith";
```

### 数组

```typescript
let list: number[] = [1, 2, 3];
// 数组泛型 Array<元素类型>
let list: Array<number> = [1, 2, 3];
```

### 元组

元组类型允许表示一个**已知元素数量和类型**的**数组**，各元素的类型不必相同。

比如，你可以定义一对值分别为 `string`和`number`类型的元组。

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

### 枚举

`enum`类型是对JavaScript**标准数据类型**的一个补充。 像C#等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

### Any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量：

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

### void

某种程度上来说，`void`类型像是与`any`类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`：

### Null 和 Undefined

默认情况下`null`和`undefined`是所有类型的子类型。 就是说你可以把 `null`和`undefined`赋值给`number`类型的变量。

### Object

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。例如：

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 类型断言

有时候你会遇到这样的情况，你会比TypeScript更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为`as`语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

**在jsx中使用typescript时，只能选用as语法进行断言**

## 接口(interface)

在 TypeScript 中，接口（Interfaces）是一种用于定义对象的结构或形状的类型约束。它们允许你定义某个对象应该包含哪些属性，以及每个属性的类型。接口提供了一种方式，可以在代码中定义和确保数据的结构一致性。

定义接口

```typescript
interface Person {
  name: string;
  age: number;
  gender: string;
}

```

使用该接口约束对象结构

```typescript
const person1: Person = {
  name: "Alice",
  age: 30,
  gender: "female",
};

const person2: Person = {
  name: "Bob",
  age: 25,
  gender: "male",
};

```

### 实现(implement)

- 实现是指类或对象实际应用接口的过程。当一个类或对象满足了接口定义的所有属性和方法时，就称它们为实现了该接口。
- 在 TypeScript 中，类可以通过 `implements` 关键字来实现一个或多个接口。这意味着类**必须**实现接口中声明的所有属性和方法，并提供它们的具体实现。
- 一旦类实现了接口，它就可以被视为符合该接口的数据类型，可以在需要接口类型的地方使用该类的实例。

```typescript
// 定义接口
interface Shape {
  name: string;
  area(): number; //area为返回值为number的函数
}

// 实现接口
class Circle implements Shape {
  constructor(public name: string, public radius: number) {}

  area(): number {
    return Math.PI * this.radius * this.radius;
  }
}

const circle1: Shape = new Circle("Circle 1", 5);
console.log(circle1.area()); // 输出：78.53981633974483
```

### 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}
```

该示例表示color和width并非必须参数

### 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly`来指定只读属性:

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

你可以通过赋值一个对象字面量来构造一个`Point`。 赋值后， `x`和`y`再也不能被改变了。

```typescript
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

如何将readonly与const区分开？

最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为**变量**使用的话用 `const`，若做为**属性**则使用`readonly`。

## 泛型

软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。

在像C#和Java这样的语言中，可以使用`泛型`来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

示例:

```typescript
function identity<T>(arg: T): T {
    return arg;
}
```

我们给identity函数添加`类型变量`T,T帮我们捕获传入参数的类型(例如number)

我们将这个identity函数叫做泛型,因为它可以适用于多个类型。 不同于使用 `any`，它不会丢失信息，像第一个例子那像保持准确性，传入数值类型并返回数值类型。

### 泛型类型

如何创建泛型接口

泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样：

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <T>(arg: T) => T = identity;
```

## 类型别名

我们可以使用`type`关键字给类型起个别名,方便使用

```typescript
type userID = number | string
```



## demo

```typescript
const url: string = "https://api.thecatapi.com/v1/images/search";
/* 
需求：
点击按钮后向后端发送请求，将返回的数据渲染到表格中；
且表格中有一个删除按钮，点击后可以删除相应数据
思路：
向后端发送请求获取数据
将获取的数据渲染到HTML页面
删除
*/
// 获取按钮
// 使用联合类型
const button: HTMLButtonElement | null = document.querySelector("button");
// 需要渲染数据的表格
// 使用断言
const tbody = document.querySelector("#table-body") as HTMLTableElement;
// 定义一个接口用于规范数据对象的属性
interface CatType {
  id: number;
  url: string;
  height: number;
  width: number;
  test?: boolean;
}

class Cat implements CatType {
  id: number;
  url: string;
  height: number;
  width: number;
  // 构造器用于初始化对象
  constructor(id: number, url: string, height: number, width: number) {
    this.id = id;
    this.url = url;
    this.height = height;
    this.width = width;
  }
}

class WebDisplay {
  // 声明静态方法
  public static addData(data: CatType) {
    // 渲染数据
    const cat: Cat = new Cat(data.id, data.url, data.height, data.width);
    // 被渲染的元素
    const tableRow: HTMLTableRowElement = document.createElement("tr");
    tableRow.innerHTML = `
        <td>${cat.id}</td>
        <td><img src="${cat.url}"></td>
        <td>${cat.height}</td>
        <td>${cat.width}</td>
        <td>${url}</td>
        <td><a href="#">X</a></td>
        `;
    // 插入元素
    tbody.appendChild(tableRow);
  }
  // 删除数据
  public static deleteData(deleteButton: HTMLAnchorElement) {
    const td = deleteButton.parentElement;
    const tr = td?.parentElement;
    tr?.remove();
  }
}

// 获取json数据
// 使用泛型
async function getJson<T>(url: string): Promise<T> {
  // 获取数据
  const response: Response = await fetch(url);
  // 使用泛型的原因：不知道返回的数据类型
  const json: Promise<T> = await response.json();
  // 返回json数据
  return json;
}

// 获取数据并渲染
async function getData(): Promise<void> {
  try {
    const json = await getJson<CatType[]>(url);
    const data: CatType = json[0];
    // 渲染数据
    WebDisplay.addData(data);
  } catch (error: Error | unknown) {
    let message: string;
    if (error instanceof Error) {
      message = error.message;
    } else {
      message = String(error);
    }
    console.log(message);
  }
}

// 为按钮绑定事件
button?.addEventListener<"click">("click", getData);
// 删除数据
tbody.addEventListener<"click">("click", (event: MouseEvent) => {
  const target = event.target as HTMLAnchorElement;
  if (target.tagName === "A") {
    WebDisplay.deleteData(target);
  }
});

```



# Fetch

`fetch()` 是一个现代的 Web API，在 JavaScript 中用于发送网络请求并获取数据。它提供了一种简单和灵活的方式来进行网络通信，取代了传统的 XMLHttpRequest（XHR）方法。

fetch返回一个promise对象

```js
fetch(url)
  .then(response => {
    // 处理响应
    return response.json(); // 解析响应数据为 JSON 格式
  })
  .then(data => {
    // 在这里处理解析后的数据
    console.log(data);
  })
  .catch(error => {
    // 处理请求错误
    console.error('Error:', error);
  });
```

当fetch结合async...await使用时

```js
async function (url) {
            // 返回response对象
            const result = await fetch(url)
            // console.log(result)
            // 转换成json格式
            // json()方法返回一个promise对象
            const data = await result.json() // 使用await获取promise对象的值
        }
```

通过await直接拿到promise对象的值,使代码可读性更好