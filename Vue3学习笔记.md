# Vue3

🟢开始时间:2023‎年‎7‎月‎17‎日，‏‎20:52:24

## setup

### 是什么？

set up是Vue中一个新的配置项,值为一个函数

### 有什么用?

setup是所有组合式api(composition API)`表演的舞台`

组件中需要用到的方法、数据等等，均需要配置在setup中

### 怎么用?

```js
<template>
 <div @click="changeMsg">{{msg}}</div>
</template>

<script>
import { ref,defineComponent } from "vue";
export default defineComponent({
setup() {
    const msg = ref('hello world')
    const changeMsg = ()=>{
      msg.value = 'hello juejin'
    }
return {
  msg,
  changeMsg
};
},
});
</script>

```

若返回的是对象,则在模板中可以直接使用对象中的属性和方法

**setup语法糖**

```ts
<script setup lang="ts">
// 引入ref
import { ref } from 'vue'
// 指定初始值为0
const count = ref(0)
const addCount = () => {
  count.value++
}
</script>
```

setup语法糖可以让变量方法不需要包含在对象中return就可以**直接使用**,后面的组件和自定义指令也能在template中自动获得

**注:在setup语法糖中,不用显式注册子组件,子组件会自动地被识别并可以直接在模板中使用，无需在 `components` 选项中注册**

## ref/reactive

vue2中的响应式数据都是配置在data函数中,当数据发生变化则页面也随之变化.但vue3的组合式api中并没有data配置项,那该如何实现响应式数据呢?

```typescript
// 引入ref
import { ref, reactive } from 'vue'
// 指定初始值为0
const count = ref(0)
const pussy = reactive({
  name: '嘉然',
  kd: '烂'
})
```

如代码示例,vue3中引入了`ref`和`reactive`函数用于创建响应式数据

其中,`ref`用于创建单一响应式数据,需要使用.value属性访问和修改属性

`reactive`用于将一个普通对象转化成响应式对象,对象的所有属性都会变成响应式数据,且可直接访问和修改对象的属性

**总之**,`ref`更适用于定义简单类型的数据,`reactive`更适用于定义复杂类型的数据

### 那么,开发过程中如何选择使用呢?

虽然没有严格的规则，但在某些情况下，使用一些特定的内容才是最佳实践，当然你也能够 ref 一把梭哈

1. 如果你需要一个响应式原始值，那么使用 ref() 是正确的选择，要注意是原始值
2. 如果你需要一个响应式对象，层级不深，那么使用 ref 也可以
3. 如果您需要一个响应式可变对象，并且对象层级较深，需要深度跟踪，那么使用 reactive

你可以把 reactive 看成 ref 的子集，ref 可以解决一切烦恼

## 生命周期

|     Vue2      |      Vue3       |        描述        |
| :-----------: | :-------------: | :----------------: |
| beforeCreate  |       ——        |     实例创建前     |
|    created    |       ——        |     实例创建后     |
|  beforeMount  |  onBeforeMount  |   DOM挂载前调用    |
|    mounted    |    onMounted    |  DOM挂载完毕调用   |
| beforeUpdate  | onBeforeUpdate  |  数据更新前被调用  |
|    updated    |    onUpdated    | 数据更新完成被调用 |
| beforeDestroy | onBeforeUnMount |  组件被销毁前调用  |
|   destroyed   |   onUnmounted   |  组件销毁完成调用  |

vue3相比vue2移除了beforeCreate和created这两个钩子函数，因为setup就是围绕着两个钩子运行的，因此不再需要它们

示例

```typescript
<script setup lang="ts">
import { onMounted } from 'vue'
onMounted(() => {
  console.log('挂载完毕')
})
</script>
```

可以看出vue3中的组合式API采用hook函数引入生命周期;不仅如此,watch,computed,路由守卫也是采用hook函数实现

## watch和computed

```vue
<template>
  <div>{{ addSum }}</div>
</template>
<script setup>
import { computed, ref, watch } from "vue";
const a = ref(1)
const b = ref(2)
let addSum = computed(() => {
  return a.value + b.value
})
watch(a, (newValue, oldValue) => {
  console.log(`a从${oldValue}变成了${newValue}`)
})
</script>
```

### 关于watch的几个注意点

1. 对于`reactive`包裹的对象,watch默认强制开启深度监听
2. 监听`reactive`对象时,由于newValue和oldValue指向的是同一个引用,因此属性值是相同的
3. 你不能监听`reactive`对象中的属性值,而是要使用函数的形式传入该属性 `()=>state.obj`
4. 当你监听`reactive`对象中的某个属性(且该属性也为对象时),你需要开启深度监听才能监测到该属性的

vue3除了watch,还引入了副作用监听函数`watchEffect`

**什么是watchEffect呢?**

watchEffect会立刻执行传入的一个函数(相当于 `immediate: true`),同时响应式**追踪**该函数的**依赖**(比如下面的示例中console.log依赖于watchTarget的值),并在依赖发生改变时再次执行该函数

```vue
<template>
  <div>{{ watchTarget }}</div>
</template>
<script setup>
import { watchEffect,ref } from "vue";
const watchTarget = ref(0)
watchEffect(()=>{
  console.log(watchTarget.value)
})
setInterval(()=>{
  watchTarget.value++
},1000)
</script>
```

总结,watch和computed依赖的数据源必须得是**响应式**的.Vue3引入了watchEffect,watchEffect 相当于将 watch 的依赖源和回调函数合并，当任何你有用到的响应式依赖更新时，该回调函数便会重新执行。不同于 watch的是watchEffect的回调函数会被立即执行，即（{ immediate: true }）

## toRef和toRefs

### toRef

当你需要将响应式对象中的某个属性取出来赋值给另外一个变量时,可以使用`toRef`将该变量转化为响应式数据,且与响应式对象仍保持映射关系

```vue
<template>
    <h3>我是{{ taokouzi.name }},职业是{{ state.person.job.name }}</h3>
    <button @click="changeJob">点我修改职业</button>
</template>

<script setup lang='ts'>
import { reactive, toRef } from 'vue'
const state = reactive({
    person: {
        name: '丽丽',
        job: {
            name: '讨口子'
        }
    }
})
// 使用一个变量接收响应式对象中的属性
const taokouzi = toRef(state, 'person')
const changeJob = () => {
    taokouzi.value.job.name = '网红'
}
</script>

<style scoped></style>
```

点击按钮后,即使修改的是taokouzi的属性值,但在模板中引用的state.person的属性值也发生了变化,证明`toRef`在`reactive`对象和新变量间制造了映射关系

### toRefs

`toRefs`使用与`toRef`差不多,不同的是它可以将`reactive`对象转化为普通对象,而对象中的每个属性都是对象的`ref`,这样对该普通对象进行**解构赋值**时也不影响内部数据的响应性

## 组件通信

|     方式     |      Vue2      |       Vue3        |
| :----------: | :------------: | :---------------: |
|    父传子    |     props      |       props       |
|    子传父    |     $emit      |       emits       |
|    父传子    |     $attrs     |       attrs       |
|    子传父    |   $listeners   | 无(合并到attrs中) |
|    父传子    | provide/inject |  provide/inject   |
|    父传子    |    $parent     |        无         |
|    子传父    |   $children    |        无         |
|    子传父    |      $ref      |    expose/ref     |
| 兄弟组件传值 |    EventBus    |       mitt        |

### props

vue3中使用props给子组件传递数据的使用方法与vue2并没有太大不同,同样是通过v-bind给子组件传递属性,子组件通过props接收

```typescript
//父组件
<template>
  <div>
    <Child :msg="parentMsg" />
  </div>
</template>
<script setup>
import { ref } from 'vue'
import Child from './Child.vue'
const parentMsg = ref('父组件信息')
</script>

//子组件
<template>
    <div>
        {{ parentMsg }}
    </div>
</template>
<script setup>
import { toRef, defineProps } from "vue";
const props = defineProps(["msg"]);
console.log(props.msg) //父组件信息
let parentMsg = toRef(props, 'msg')
</script>
```

**注**:若子组件想将props赋值给另外一个变量,则需要使用`toRef`函数将该变量变成响应式

### $emit

子组件通过emit发布一个事件并**传递参数**,父组件通过v-on对这个事件进行监听

```vue
// 父组件中
<template>
<Child :count="count" @sendMsg="getMsg" />
</template>
<script setup lang="ts">
const getMsg = (msg: string) => {
  alert('父组件收到了子组件的消息:' + msg)
}
</script>

// 子组件
<script setup lang="ts">
const emit = defineEmits(['sendMsg'])
const sendHello = () => {
    // 通过emit向父组件传递数据
    emit('sendMsg', 'hello')
}
</script>
```

### $attrs和$listeners(Vue3移除listeners)

在vue2中,$attrs可以获取父组件除了props传递的属性和特定的绑定属性(style和class)外的**所有属性**

$listeners可以获取父组件所有v-on(使用了.native修饰符的除外)事件监听器

vue3中,移除了$listeners,但是$attrs可以获得父组件传来的属性,也能获得父组件中的v-on监听器

#### vue2(option API)

```vue

//父组件

<template>
  <div>
    <Child @parentFun="parentFun" :msg1="msg1" :msg2="msg2"  />
  </div>
</template>
<script>
import Child from './Child'
export default {
  components:{
    Child
  },
  data(){
    return {
      msg1:'子组件msg1',
      msg2:'子组件msg2'
    }
  },
  methods: {
    parentFun(val) {
      console.log(`父组件方法被调用,获得子组件传值：${val}`)
    }
  }
}
</script>

//子组件

<template>
  <div>
    <button @click="getParentFun">调用父组件方法</button>
  </div>
</template>
<script>
export default {
  methods:{
    getParentFun(){
      this.$listeners.parentFun('我是子组件数据')
    }
  },
  created(){
    //获取父组件中所有绑定属性
    console.log(this.$attrs)  //{"msg1": "子组件msg1","msg2": "子组件msg2"}
    //获取父组件中所有绑定方法    
    console.log(this.$listeners) //{parentFun:f}
  }
}
</script>
```

#### vue3(composition API)

```typescript
//父组件

<template>
  <div>
    <Child @parentFun="parentFun" :msg1="msg1" :msg2="msg2" />
  </div>
</template>
<script setup>
import Child from './Child'
import { ref } from "vue";
const msg1 = ref('子组件msg1')
const msg2 = ref('子组件msg2')
const parentFun = (val) => {
  console.log(`父组件方法被调用,获得子组件传值：${val}`)
}
</script>

//子组件

<template>
    <div>
        <button @click="getParentFun">调用父组件方法</button>
    </div>
</template>
<script setup>
import { useAttrs } from "vue";

const attrs = useAttrs()
//获取父组件方法和事件
console.log(attrs) //Proxy {"msg1": "子组件msg1","msg2": "子组件msg2"}
const getParentFun = () => {
    //调用父组件方法
    attrs.onParentFun('我是子组件数据')
}
</script>
```

**注**:使用attrs调用父组件传递的事件时,需要在事件名前面加上`on`,比如(parentEvent => onParentEvent)

### provide/inject

provide:是一个对象,或返回对象的函数,里面包含着要传递给子孙后代的属性

inject:一个字符串数组,或一个对象.获取父组件或更高层次组件的provide值,即在任何后代组件中都能通过inject获得.

#### option API

```vue
//父组件
<script>
import Child from './Child'
export default {
  components: {
    Child
  },
  data() {
    return {
      msg1: '子组件msg1',
      msg2: '子组件msg2'
    }
  },
  provide() {
    return {
      msg1: this.msg1,
      msg2: this.msg2
    }
  }
}
</script>

//子组件

<script>
export default {
  inject:['msg1','msg2'],
  created(){
    //获取高层级提供的属性
    console.log(this.msg1) //子组件msg1
    console.log(this.msg2) //子组件msg2
  }
}
</script>
```



#### composition API

```vue

//父组件
<script setup>
import Child from './Child'
import { ref,provide } from "vue";
const msg1 = ref('子组件msg1')
const msg2 = ref('子组件msg2')
provide("msg1",msg1)
provide("msg2",msg2)
</script>

//子组件

<script setup>
import { inject } from "vue";
console.log(inject('msg1').value) //子组件msg1
console.log(inject('msg2').value) //子组件msg2
</script>
```

**说明:provide/inject在深层组件嵌套中使用较合适.一般在组件开发中使用居多**

### parent/children(Vue3已移除)

$parent:子组件获取父组件的vue实例,用于获取父组件的属性,方法等

$children:父组件获取子组件的vue实例,是一个数组,但不保证子组件在数组中的顺序

vue2

```vue
<script>
import Child from './Child'
export default {
  components: {
    Child
  },
  created(){
    console.log(this.$children) //[Child实例]
    console.log(this.$parent)//父组件实例
  }
}
</script>
```

### expose/ref

Vue2中,$ref可以直接获取子组件实例,也可以获取实例上的方法和属性

Vue3中的使用:

```vue

//父组件

<template>
  <div>
    <Child ref="child" />
  </div>
</template>
<script setup>
import Child from './Child'
import { ref, onMounted } from "vue";
const child = ref() //注意命名需要和template中ref对应
onMounted(() => {
  //获取子组件属性
  console.log(child.value.msg) //子组件元素

  //调用子组件方法
  child.value.childFun('父组件信息')
})
</script>

//子组件

<template>
    <div>
    </div>
</template>
<script setup>
import { ref,defineExpose } from "vue";
const msg = ref('子组件元素')
const childFun = (val) => {
    console.log(`子组件方法被调用,值${val}`)
}
//必须暴露出去父组件才会获取到
defineExpose({
    childFun,
    msg
})
</script>
```

**注:子组件必须使用defineExpose()将数据暴露出去,父组件才能获取到!!!**

### EventBus/mitt

Vue2中通过安装全局事件总线,来实现任意组件之间的通信

```js
new Vue({
	......
	beforeCreate() {
		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
	},
    ......
}) 

```

```vue
//组件1
<template>
  <div>
    <button @click="sendMsg">传值</button>
  </div>
</template>
<script>
import Bus from './bus.js'
export default {
  data(){
    return {
      msg:'子组件元素'
    }
  },
  methods:{
    sendMsg(){
      Bus.$emit('sendMsg','兄弟的值')
    }
  }
}
</script>

//组件2

<template>
  <div>
    组件2
  </div>
</template>
<script>
import Bus from './bus.js'
export default {
  created(){
   Bus.$on('sendMsg',(val)=>{
    console.log(val);//兄弟的值
   })
  }
}
</script>

//bus.js

import Vue from "vue"
export default new Vue()
```

mitt.js同理,不再赘述

## v-model和sync

在vue中我们知道props是单向数据流,每次父组件更新时,子组件的props也会更新为最新的值

但若是子组件想要修改props,则需要使用到.sync修饰符.而Vue3中移除了.sync的写法.

```vue
// 父组件中
<template>
<Child v-model:changeCount="count"/>
</template>

// 子组件
<script setup>
const emit = defineEmits(['update:changeCount'])
const changeProps = () => {
    // 通过emit修改父组件传递的数据
    emit('update:changeCount', 100)
}
</script>
```

总结:

vue3中移除了sync的写法，取而代之的式v-model:event的形式

其`v-model:事件名="msg"`或者`:changePval.sync="msg"`(Vue2写法)的完整写法为 `@update:changePval="msg=$event"`。

所以子组件需要发送`update:事件名`事件进行修改父组件的值

## customRef

作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制

```Vue
<template>
    <input type="text" v-model="keyword">
    <h3>{{ keyword }}</h3>
</template>

<script setup lang='ts'>
/* 
需求：在input框输入1s后,下方h3标签显示输入文本
*/
import { customRef } from 'vue';
// const myRef = ref('')
// 自定义ref(参数:value为我们要传入自定义ref的值)
const myRef = (value: string, delay: number) => {
    let timer: number
    return customRef((track, trigger) => {
        // 需要指定getter和setter
        return {
            get() {
                // 收集依赖
                track()
                return value
            },
            set(newVal) {
                clearTimeout(timer)
                // 延迟触发更新
                timer = setTimeout(() => {
                    value = newVal
                    // 数据修改触发依赖更新
                    trigger()
                }, delay)
            }
        }
    })
}
// 使用自定义ref
const keyword = myRef('', 1000)
</script>

<style scoped></style>
```



## 自定义hooks

当我们谈论有状态逻辑和无状态逻辑时，可以将其类比为日常生活中的两种不同类型的任务。

1. 无状态逻辑：

	无状态逻辑类似于我们日常生活中的简单任务。这些任务不需要依赖过去的状态或上下文信息，而是仅根据给定的输入，立即给出预期的输出。例如，我们可能需要一个函数来将一个给定的数字转换成货币格式，或者将日期格式化为特定的格式。这些任务的结果只取决于输入，与之前的状态无关。

	在编程中，无状态逻辑通常是纯函数，它们不修改任何外部状态，并且对于相同的输入始终返回相同的输出。这使得它们易于测试和复用。类似于 lodash 或 date-fns 这样的库，提供了许多无状态逻辑的实用函数，可以帮助我们处理各种简单而重复的任务。

2. 有状态逻辑：

	有状态逻辑则类似于我们日常生活中的复杂任务。这些任务需要考虑过去的状态或上下文信息，并根据不同的情况做出不同的决策。例如，跟踪当前鼠标在页面中的位置，它需要不断更新鼠标的位置信息，从而产生一个随时间变化的状态。

	在编程中，有状态逻辑管理着会随时间变化的状态，并且可能会根据状态的变化做出不同的行为或响应。在 Vue 应用中，有状态逻辑常常用于管理组件的数据、状态和行为。Vue 的组合式 API 提供了一种有效的方式来封装和复用有状态逻辑，称为 "组合式函数"（Composables）。通过组合式函数，我们可以将复杂的有状态逻辑抽象成独立的功能块，并在不同的组件中复用它们。

综上所述，无状态逻辑类似于简单的即时任务，仅取决于给定的输入而产生预期的输出。而有状态逻辑则类似于复杂的持续任务，需要考虑过去的状态或上下文信息，并随时间变化而做出不同的决策。Vue 的组合式 API 可以帮助我们封装和复用有状态逻辑，使得我们的应用代码更加模块化和易于维护。

## 自定义指令

在`<script setup>`中,你可以通过以下方式注册指令

局部自定义指令:

```vue
<script setup>
// 在模板中启用 v-focus
const vFocus = {
  mounted: (el) => el.focus()
}
</script>

<template>
  <input v-focus />
</template>
```



## 路由

## 响应式原理

首先,什么是**响应式**?其实很简单,就是组件的`data`数据一旦被修改,则立即触发视图更新

让我们先来回顾一下Vue2中的响应式原理

### Vue2

Vue2中使用`Object.defineProperty`对数据进行劫持并修改,实现的响应式

缺点:

- 对结构复杂的对象实现深层次监听需要一次性递归多层数据

- 无法直接对数据对象新增/删除属性(可以借助`$set`,`$delete`)
- 对数组的下标直接修改,Vue无法监听到数据的改变

### Vue3

Vue3中通过Proxy代理对象和Reflect反射对象,实现了响应式.并且解决了Vue2中无法监听到新增/删除属性的缺陷;以及直接修改数组下标无法监听到的问题

实现原理:

- 通过Proxy(代理),拦截源对象中任意属性的变化,例如对属性的读写,新增,删除
- 通过Reflect(反射),对源对象中的属性进行操作

```js
new Proxy(data, {
	// 拦截读取属性值
    get (target, prop) {
    	return Reflect.get(target, prop)
    },
    // 拦截设置属性值或添加新属性
    set (target, prop, value) {
    	return Reflect.set(target, prop, value)
    },
    // 拦截删除属性
    deleteProperty (target, prop) {
    	return Reflect.deleteProperty(target, prop)
    }
})

proxy.name = 'tom'   
```

## Teleport组件

## Suspense组件

## 如何获取组件实例

在Vue3的`setup`和其它`composition API`中没有`this`,所以我们该如何访问到组件实例呢

## 完成TodoList

在学习Vue2时,已经实现了一个简易的todoList,如今学习Vue3时也当然少不了使用新语法新特性再实现一次了

样式方面,直接使用Vue2中的

### 分析需求:

1. 添加,删除待做事项
2. 修改待做事项
3. 一键勾选/反选事项
4. 一键删除已做事项
5. 动画效果
6. 本地存储

### 踩坑1:

```js
const todos = ref([] as Array<Todo>)
const todo = reactive({
    // 待做事项名
    title: '',
    // 是否完成
    done: false
})
```

由于todo是响应式变量,若直接将todo作为参数push到数组todos中,那么数组中的所有元素的引用实际上是指向了同一个地址.就拿以下代码为例

```js
const addTodo = () => {
    if (todo.title.trim() === '') {
        alert('输入不能为空!')
        todo.title = ''
        return
    }
    todos.value.push(todo)
```

在这段代码中,我将todo直接push到了todos数组中,但实际上只是把todo的引用添加到了数组之中,也就是说无论在这之后push多少个todo,所指向的其实都是**同一个引用**.因此,我们在用户输入文字获取到todo对象后,应该使用`Object.assign`或ES6的解构赋值对其进行复制,之后将复制的值push到todo数组中.

### 实现input框自动聚焦

ref:一开始的实现思路和Vue2时一样,给input框标记一个ref,之后再监听数据源todos中的isEdit的变化,当值为`true`时,则调用`input.value.focus()`,但在具体的实现过程中,无论是在数据发生变化后调用`input.value.focus()`,抑或是使用`nextTick`,都显示`input.value.focus` not a function,不知道问题出现在哪了,暂且不表

自定义指令:

```typescript
const vFocus = {
    updated(el: any, binding: any) {
        if (binding.value) {
            el.focus()
        }
    }
}
```

在setup语法糖中，任何以v开头的驼峰式命名的变量都可以被用作自定义指令.在以上代码中,我定义了一个v-focus指令,则在模板中可以直接使用

```vue
<input type="text" v-show="item.isEdit" @blur="item.isEdit = false"                v-focus="item.isEdit">
```

没想到困扰我一晚上的功能,只需要一个自定义指令就解决了😅

### 全选/反选

思路:一开始把这个功能的实现思路想的太复杂了,使用了自定义事件将footer中更新的数据传递给根组件,并且通过监听todos数组来实现修改todo时,全选框的动态变化.但后续实践起来发现代码过于冗余,且有些小bug.于是重新转换思路,选择使用计算属性对全选框进行处理.

实现如下:

```typescript
const isAllDone: Ref<boolean> = computed({
    get() {
        return todos.length ? todos.every(todo => todo.done) : false
    },
    set(newValue) {
        return todos.forEach((todo) => todo.done = newValue)
    }
})

/* 删除全部完成事项 */
const clearAll = () => {
    if (isAllDone) {
        // 删除数组中所有元素
        todos.splice(0, todos.length)
    }
}
```

## Vue3父子组件挂载顺序
