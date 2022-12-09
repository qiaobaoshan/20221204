136 尚硅谷
[toc]
# `vue` 基础
## 初识 `Vue`
- 初识`Vue`：
	- 1.想让`Vue`工作，就必须创建一个`Vue`实例，且要传入一个配置对象；
	- 2.`root`容器里的代码依然符合`html`规范，只不过混入了一些特殊的`Vue`语法；
	- 3.`root` 容器里的代码被称为【`Vue`模板】；
	- 4.`Vue`实例和容器是一一对应的；
	- 5.真实开发中只有一个`Vue`实例，并且会配合着组件一起使用；
	- 6.`{{xxx}}`中的`xxx`要写`js`表达式，且`xxx`可以自动读取到`data`中的所有属性；
	- 7.一旦`data`中的数据发生改变，那么页面中用到该数据的地方也会自动更新 ；

	- 注意区分：`js`表达式 和 `js`代码(语句)
		- 1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方：
			- (1). `a`
			- (2). `a+b`
			- (3). `demo(1)`
			- (4). `x === y ? 'a' : 'b'`

		- 2.`js`代码(语句)
			- (1). `if(){}`
			- (2). `for(){}`

## `Vue`模板语法有2大类：

- 1.插值语法：

	- **功能**：用于解析标签体内容。
	- **写法**：`{{xxx}}`，`xxx`是`js`表达式，且可以直接读取到`data`中的所有属性。

- 2.指令语法：

	- **功能**：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）。
	- **举例**：`v-bind:href="xxx"` 或  简写为 `:href="xxx"`，`xxx`同样要写`js`表达式，且可以直接读取到data中的所有属性。
	- **备注**：`Vue`中有很多的指令，且形式都是：`v-????`，此处我们只是拿`v-bind`举个例子。

## `Vue`中有2种数据绑定的方式：

- 1.**单向绑定(`v-bind`)**：数据只能从`data`流向页面。

- 2.**双向绑定(`v-model`)**：数据不仅能从`data`流向页面，还可以从页面流向`data`。

- **备注**：
	- 1.双向绑定一般都应用在表单类元素上（如：`input`、`select`等）
	- 2.`v-model:value` 可以简写为 `v-model`，因为`v-model`默认收集的就是`value`值。

## 136 `Vue3` 带来了什么？
    （2600+次提交，30+RFC（请求意见稿），600+次PR）
    **Vue3github**:https://github.com/vuejs/vue-next/releases/tag/v3.0.0

- 性能提升
- 源码的升级:
    使用 `Proxy` 代替 `defineProperty` 实现响应式
    重写虚拟 `Dom` 和使用`Tree-shaking`去除没有使用的代码
- 拥抱 `TypeScript`
- 新的特性：
    组合式`API`
    - `setup`配置
    - `ref`和`reactive`
    - `watch`与`watchEffect`

    新的内置组件：
    - 
    其他的改变：

# vuex 
## 5.1 理解 `vuex`
### 5.1.1 `vuex` 是什么
- 1. 概念：专门在 `Vue` 中实现集中式状态（数据）管理的一个 `Vue` 插件，对 `vue` 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。
- 2. `Github` 地址: https://github.com/vuejs/vuex

### 5.1.2 什么时候使用 `Vuex`
- 1. 多个组件依赖于同一状态
- 2. 来自不同组件的行为需要变更同一状态

### 5.1.3 小案例
- `vue` 实现计数器（未使用`vuex`）
```vue
<!-- components/count.vue -->
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
				sum:0 //当前的和
			}
		},
		methods: {
			increment(){
				this.sum += this.n
			},
			decrement(){
				this.sum -= this.n
			},
			incrementOdd(){
				if(this.sum % 2){
					this.sum += this.n
				}
			},
			incrementWait(){
				setTimeout(()=>{
					this.sum += this.n
				},500)
			},
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>

```
- `vuex` 实现 计数器

```vue
<!-- components/count.vue -->
<template>
	<div>
		<h1>当前求和为：{{$store.state.sum}}</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			increment(){
				//actions中没有逻辑，直接commit，跳过了dispatch到actions
				this.$store.commit('JIA',this.n)
			},
			decrement(){
				//actions中没有逻辑，直接commit，跳过了dispatch到actions
				this.$store.commit('JIAN',this.n)
			},
			incrementOdd(){
				this.$store.dispatch('jiaOdd',this.n)
			},
			incrementWait(){
				this.$store.dispatch('jiaWait',this.n)
			},
		},
		mounted() {
			console.log('Count',this)
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>

```

```js
//store/index.js

//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)

//准备actions——用于响应组件中的动作
const actions = {
	/* jia(context,value){
		console.log('actions中的jia被调用了')
		context.commit('JIA',value)
	},
	jian(context,value){
		console.log('actions中的jian被调用了')
		context.commit('JIAN',value)
	}, */
	jiaOdd(context,value){
		console.log('actions中的jiaOdd被调用了')
		if(context.state.sum % 2){
			context.commit('JIA',value)
		}
	},
	jiaWait(context,value){
		console.log('actions中的jiaWait被调用了')
		setTimeout(()=>{
			context.commit('JIA',value)
		},500)
	}
}
//准备mutations——用于操作数据（state）
const mutations = {
	JIA(state,value){
		console.log('mutations中的JIA被调用了')
		state.sum += value
	},
	JIAN(state,value){
		console.log('mutations中的JIAN被调用了')
		state.sum -= value
	}
}
//准备state——用于存储数据
const state = {
	sum:0 //当前的和
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```
### 5.1.4  `Vuex` 原理图
<img src="./images/vuex.png" width="500px" />

### 搭建 `vuex` 环境
- 1 创建文件 `src/store/index.js`

```js

//该文件用于创建Vuex中最为核心的store
import Vue from 'vue'

//引入Vuex
import Vuex from 'vuex'

//应用Vuex插件
Vue.use(Vuex)

//准备actions——用于响应组件中的动作
const actions = {}

//准备mutations——用于操作数据（state）
const mutations = {}

//准备state——用于存储数据
const state = {
	sum:0 //当前的和
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
})
```

- 2 在`main.js` 中创建`vm` 时传入 `store` 配置项

```js
//...
//引入store
import store from './store'
//...
//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store
})
```

## 5.2 `vuex` 核心概念和 API

### 5.2.1 `state`
- 1. `vuex` 管理的状态对象
- 2. 它应该是唯一的
- 3. 示例代码：

### 5.2.2 `actions`
- 1. 值为一个**对象**，包含多个响应用户动作的回调函数
- 2. 通过 `commit( )`来触发 `mutation` 中函数的调用, 间接更新 `state`
- 3. 如何触发 `actions` 中的回调？  
	在组件中使用: `$store.dispatch('对应的 action 回调名') 触发`
- 4. 可以包含异步代码（定时器, `ajax` 等等）
- 5. 示例代码：

### 5.2.3 `mutations`
- 1. 值是一个**对象**，包含多个直接更新 `state` 的方法
- 2. 谁能调用 `mutations` 中的方法？如何调用？   
	在 `action` 中使用：`commit('对应的 mutations 方法名') 触发`
- 3. `mutations` 中方法的特点：不能写异步代码、只能单纯的操作 state
- 4. 示例代码：

### 5.2.4 `getters`
- 1. 值为一个**对象**，包含多个用于返回数据的函数
- 2. 如何使用？—— `$store.getters.xxx`
- 3. 示例代码：
### 5.2.5 `modules`
- 1. 包含多个 `module`
- 2. 一个 `module` 是一个 `store` 的配置对象
- 3. 与一个组件（包含有共享数据）对应
# 


## 创建一个`vue3`的项目工程
### 137、使用  `vue/cli`  创建

官网：

可配置淘宝镜像或者采用科学上网工具来提升速度

```shell
## 查看@vue/cli 版本，确保在4.5.0以上
vue --version
## 安装或者升级你的vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve

```
### 138、使用 `vite` 创建

vite官网：https://vitejs.cn
- 什么事vite?  新一代前段构建工具
- 优势如下：
    - 开发环境中，无需打包操作，可快速地冷启动
    - 轻量快速地热重载（HMR）
    - 真正的按需编译，不用在等待整个应用编译完成。
    - 丰富的功能
    - 优化的构建

- 传统构建webpack 和 vite 的构建对比图
    - wepack：是一个入口，根据路由，分析，都准备好。
    - Vite：是根据你进来的网页 查看入口，再准备好了。

```shell
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm i
## 运行
npm run dev

```
关闭语法检查：配置`vue.config.js`,配置 `lintOnSave:false` 即可

## 139 看看`vue3`的项目目录

先看一下`src`下`main.js`文件

```js
// 引入的不是 Vue 构造函数了，是一个名为createApp 工厂函数
// 这里不支持vue2的写法
import { createApp, VueElement } from 'vue'
import App from './App.vue'
import './index.css'

createApp(App).mount('#app')
// 上面可以写成：
// 实际是创建一个实力对象--app （类似于之前的vue2 中的vm，但是app比VM更轻）
// const app = createApp(App)
// console.log("app",app);
//挂载
// app.mount("#app")

// 去除挂载
// setTimeout( ()=>{
//     app.unmount("#app")
// },2000)


// vue2 原来是这么写的
// const vm = new Vue({
//     render:h => h(App)
// })
// vm.$mount('#app')

```
再看看`App.vue`文件
```vue
<template>
    <!-- vue 3 可以没有根标签结构 -->
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>
```
## 140 安装`vue3 devtools`
 在 `chrome` 浏览器应用商店搜索进行安装
 或者在 `github` 上下载进行安装

# 141 常用的`Composition API`
拉开序幕的`setup`

- 1 理解：`Vue3.0` 重点额一个新的配置项，值是一个**函数**。

- 2 `setup`是所有的`Composition API` “**表演的舞台**”

- 3 组件中用到的：数据、方法等等 均要配置在`setup`中

- 4 `setup` 函数的两种返回值：

    - 若返回一个对象，则对象中的属性、方法，在模板中均可以直接使用！（重点关注）
    - 若返回一个渲染函数，则可以自定义渲染内容。

- 5 注意点：

    - 尽量不要于`vue2` 配置混用

    > `vue2.x` 配置（data,methods,computed...）中，均<font color="red">可以访问</font>到`setup`中的属性 方法

    > 但在`setup`中<font color="red">不能访问</font>`vue2.x` 配置（data,methods,...）
    
    > 如果有重名，`setup`优先
    
    - `setup`不能是一个`async` 函数， 因为返回值不再是`return` 的对象，而是`promise` ，模板看不到 `return` 对象的属性 （后期也可以返回一个 `promise` 实例，但需要 `Suspense` 和异步组件的配合 ）

```vue
<!-- App.vue -->
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3.0 + Vite" />
  <div> 姓名：{{name}}</div>
  <div> 年龄：{{age}}</div>
  <div> a：{{a}}</div>
  <button @click="sayHello"> vue3 说话</button>
  <br/>
  <br/>
  <button @click="sayWelcome"> vue2 说话</button>
  <br/>
  <br/>
  <button @click="test1"> vue2 读取 vue 3 的 数据</button>
  <br/>
  <br/>
  <button @click="test2"> vue3 读取 vue 2 的 数据</button>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import { h }  from 'vue'
export default {
  name: 'App',
  //vue 2 的写法-------
  data(){
    return {
      sex:"男",
      a:"100",
    }

  },
  methods:{
    sayWelcome() {
      alert(`我是${ this.sex }`)
    },
    test1() {
      alert(`我是${ this.name }`)
    },
  },

  // ----------------------------

  //此处只是测试一下setuP，不考虑响应式
  setup(){
    //数据
    let name = "张三";
    let age = "18";
    let a = "18";

    //方法
    function sayHello(){
      alert(`我叫${ name }，今年${ age }岁了`)
    }

    //方法
    function test2(){
      alert(`我vue3 叫${ this.sex }`)
    }

    // 返回一个对象，则对象中的属性、方法，在模板中均可以直接使用！
    return {
      name,
      age,
      sayHello,
      test2,
      a
    }

    // 还可以返回一个函数，渲染函数，则可以自定义渲染内容。
    // return ()=>h('h1','哈哈')


  },
  components: {
    HelloWorld
  }
}
</script>

```

## 142 `ref`数据响应式

- 定义：ref是一个函数：

- 作用：定义一个响应式数据

- 语法：`const xx = ref(initValue)`
    - 创建一个包含响应式数据的<font color="red">引用对象（reference对象，简称ref对象）</font>
    - JS 中操作数据： `xxx.value = newValue`
    - 模板中读取数据：不需要`.value` 直接`<div>{{ xxx}}</div>`
    `reference`  引用
    `implement`  实现

- 备注：
    - 接收的数据可以是：基本数据类型，也可以是对象类型
    - 基本类型的数据：响应式依然是依靠 `Object.defineProperty()` 的 `get`  和 `set` 完成的
    - 对象类型的数据： 内部“求助”了`vue3.0` 中的一个新函数--`reactive` 函数（`reactive` 函数还是使用`Object.Proxy()`实现的）
```js
/* APP.vue */

<template>
  <div> 姓名：{{name}}</div>
  <div> 年龄：{{age}}</div>
  <button @click="changeInfo">修改</button>
</template>

<script>
import {ref} from  "vue"
export default {
  name: 'App',
  
  //此处只是测试一下setuP，不考虑响应式
  setup(){

    //数据  这种数据没有响应式
    // let name = "张三";
    // let age = "18";

    // 使用ref函数实现数据响应式
    let name = ref("张三");
    let age = ref(18);


    function changeInfo(){
      // name = "李四";
      // age = "11";
 
      name.value = "李四";
      age.value = "11";

      //模板里不用.value,检查出了是ref对象，解析出来

      //修改了数据，但是没有数据相应
      console.log("name","age",name,age,job);

    }

    // 返回一个对象，则对象中的属性、方法，在模板中均可以直接使用！
    return {
      name,
      age,
      changeInfo

    }

  },
}
</script>

```
## 143 通过ref函数  处理对象类型

基本数据类型 使用的是`Object.defineProperty()`实现数据相应，
对象数据类型 使用的是`Object.Proxy()`代理对象，实现数据相应，

```vue
<template>
  <div> 姓名：{{name}}</div>
  <div> 年龄：{{age}}</div>
  <div> job：{{job.type}}</div>
  <button @click="changeInfo">修改</button>
</template>

<script>
import {ref} from  "vue"
export default {
  name: 'App',
  
  //此处只是测试一下setuP，不考虑响应式
  setup(){

    //数据  这种数据没有响应式
    // let name = "张三";
    // let age = "18";

    // 使用ref函数实现数据响应式
    let name = ref("张三");
    let age = ref(18);
    let job = ref({
      type:"11",
      salary:"30k",
    });


    function changeInfo(){ 
      name.value = "李四";
      age.value = "11";
      console.log("job.value",job.value);

      job.value.type = "22";



      //模板里不用.value,检查出了是ref对象，解析出来

      //修改了数据，但是没有数据相应
      console.log("name","age",name,age,job);

    }

    // 返回一个对象，则对象中的属性、方法，在模板中均可以直接使用！
    return {
      name,
      age,
      job,
      changeInfo

    }

  },
}
</script>

```
## 144 `reactive` 函数 
- **作用**：定义一个<font color="red">对象类型</font>的响应式数据，（基本类型不要用它，要用 `ref` 函数）
- **语法**：`const obj2代理对象 = reactive(obj1源对象)` 接收一个对象（或数组），返回一个<font color="red">代理对象（ `proxy` 对象）</font>
- `reactive` 定义的响应式数据是"深层次的"
- 内部基于 `ES6` 的 `Proxy` 实现的，通过代理对象操作源对象内部数据进行操作

```vue
<template>
  <div> 姓名：{{name}}</div>
  <div> 年龄：{{age}}</div>
  <div> job:{{job.type}}</div>
  <div> 爱好 :
  <ul>
    <li v-for="item in hobby" key="item">{{item}}</li>
  </ul> </div>
  <button @click="changeInfo">修改</button>
</template>

<script>
import {ref ,reactive} from  "vue"
export default {
  name: 'App',
  
  //此处只是测试一下setuP，不考虑响应式
  setup(){

    //数据  这种数据没有响应式
    // let name = "张三";
    // let age = "18";

    // 使用ref函数实现数据响应式
    let name = ref("张三");
    let age = ref(18);

    //使用reactive实现对象数据类型
    let job = reactive({
      type:"11",
      salary:"30k",
    });

    let hobby = reactive(["11",2,3,4]);


    function changeInfo(){ 
      console.log("job",job);

      job.type = "22";
      hobby[0] = "9999";
    }

    // 返回一个对象，则对象中的属性、方法，在模板中均可以直接使用！
    return {
      name,
      age,
      job,
      hobby,
      changeInfo

    }

  },
}
</script>


```

## 145 `Vue 3.0` 中的响应式原理

**`vue 2.0` 中的响应式原理**

- 实现原理：
  - 对象类型：通过 `Object.defineProperty()` 对属性的读取，修改，进行拦截（数据劫持）
  - 数组类型：通过 重写更新数组的一系列方法累实现拦截。  （对数组的变更方法进行了包裹）

  ```js

  // 原数据
  let person = {
      name:"张三",
      age:18
  }

  // 模拟vue2中实现数据响应式
  // 通过Object.keys()拿到person的每一个属性去循环一下，去监听数据的读取及修改
  let p = {}  ;

  let personkeys = Object.keys(person);

  // console.log(personkeys);

  personkeys.forEach( item =>{
      // console.log(item);

      Object.defineProperty(p,item,{
        configurable:true,

      get(){
          // 有人读取 item 时，调用
          return item
      },
      set(value){
          console.log("有人修改了 属性，被我发现了，我要去更新界面")
          //有人修改  item 时 调用
          item = value
      },
  })
  })
  ```
看看修改一个数据能否数据相应：
在浏览器中运行`p.name = "lisi"`,进行修改数据，即弹出日志

 <img src="./images/001.jpg"/>
我们发现修改一个数据属性，是响应式的。

看看添加一个数据能否数据相应：
在浏览器中运行`p.sex = "男"`,进行修改数据，即弹出日志

 <img src="./images/002.jpg"/>

有`get` `set` 的才是响应式，我们现在的`sex`只是死数据

看看删除一个存在的数据能否数据相应：
在浏览器中运行`delete p.sex`,进行修改数据，即弹出日志
<img src="./images/003.jpg"/>

配置一下`configurable:true,`,然后就可以删除

在浏览器中运行`delete p.sex`,进行修改数据，即弹出日志

<img src="./images/004.jpg"/>



- <font color="red">存在问题</font>：
  - <font color="red">新增属性、删除属性、界面不会更新</font>。

      - `vue 2.0` 中的 新增及修改数据后不数据相应 解决办法1 ：`this.$set(this.persion,'name',"wang")`
      - `vue 2.0` 中的 新增及修改数据后不数据相应 解决办法2 ：先导入`vue` `import vue from 'vue'`,然后使用`vue.set`方法`vue.set(this.persion,'name',"wang")`

      - `vue 2.0` 中的 删除后数据相应 解决办法1 ：`this.$delete(this.persion,'name')`
      - `vue 2.0` 中的 删除后数据相应 解决办法2 ：先导入`vue` `import vue from 'vue'`,然后使用`vue.set`方法`vue.delete(this.persion,'name')`

  - <font color="red">直接通过下标修改数组，界面不会自动更新</font>
      - `vue 2.0` 中的 修改数组数据实时相应 解决办法1 ：`this.arr.splice(0,1,"新值")`

## 146 **vue 3.0 的响应式**
- 实现原理：
  - 通过 `Proxy` （代理） ：拦截对象中的任意属性的变化，包括：属性值得读写、属性的添加、属性的删除等
  - 通过 `Reflect` （反射）： 对源代理对象的属性进行操作。

`MDN` 文档中描述的 `Proxy` 和 `Reflect` 
    - `Proxy`： https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy
    - `Reflect` ： https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

```js
const p = new Proxy(person, {
    // 读取的时候调用
    get(target, propName) {
        return Reflect.get(target,propName)
    },

    // 新增，修改的时候调用
    set(target, propName, value) {
        Reflect.set(target, propName, value)
    },

    //删除时候,调用
    deleteProperty(target, propName) {
        Reflect.deleteProperty(target, propName)
    }
})
```
```js

// 原数据
let person = {
    name:"张三",
    age:18
}
// 模拟vue3中实现数据响应式
const p = new Proxy(person,{})

```
在浏览器中，运行 `person` 和 `p` 看看
<img src="./images/005.jpg"/>

我们继续完善一下代码，然后再浏览器运行一下：

```js
// 原数据
let person = {
    name:"张三",
    age:18
}
// 模拟vue3中实现数据响应式
const p = new Proxy(person,{

  // 读取时获取到
    get(target,propName){
      console.log(`有人读取p身上的${propName}属性了`,target,propName);
      return target[propName]
    },

    // 修改时获取到  增加的时候
    set(target,propName,value){
      console.log("有人修改了p身上的${propName}属性了，我要去更新界面了！");
      console.log(target,propName,value);
      target[propName] = value
    }

    //删除时候呢？
    deleteProperty(target,propName){
       console.log("有人删除了p身上的${propName}属性了，我要去更新界面了！");
       return delete target[propName]
        
    }
})
```
结果如下：
<img src="./images/006.jpg"/>

实现删除呢？

代码如下：

```js
// 原数据
let person = {
    name: "张三",
    age: 18
}
// 模拟vue3中实现数据响应式
const p = new Proxy(person, {

    // 读取的时候调用
    get(target, propName) {
        console.log(`有人读取p身上的${propName}属性了`, target, propName);
        return target[propName]
    },

    // 新增，修改的时候调用
    set(target, propName, value) {
        console.log("有人修改了p身上的${propName}属性了，我要去更新界面了！");
        console.log(target, propName, value);
        target[propName] = value
    },

    //删除时候,调用
    deleteProperty(target, propName) {
        console.log("有人删除了p身上的${propName}属性了，我要去更新界面了！");
        return delete target[propName]
    }
})
```

## 147 `vue 3.0` 响应式原理`reflect` （反射对象）
上面是实现了数据的响应式，但是`vue 3.0` 不是这么实现的
```js
let obj = { a: 1, b: 2 };

// Object.defineProperty(obj,'c',{
//     get(){
//         return 3
//     }
// })
// Object.defineProperty(obj,'c',{
//     get(){
//         return 4
//     }
// })

const  x1 = Reflect.defineProperty(obj, 'c', {
    get() {
        return 3
    }
})
const  x2 = Reflect.defineProperty(obj, 'c', {
    get() {
        return 4
    }
})
console.log("111",x1,x2);
```
## 148  `reactive` 对比 `ref` 
**从定义数据角度对比**

  - `ref` 用来定义：<font color="red">基本数据类型</font>
  - `reactive` 用来定义：<font color="red">对象或数组 数据类型</font>
  - 备注：ref也可以用来定义<font color="red">对象或数组 类型数据</font>，它内部会通过`reactive`转为<font color="red">代理对象。</font>

**从原理角度对比**：

  - `ref` 通过`Object.defineProperty()` 的 `get` 和 `set` 来实现响应式（数据劫持）。
  - `reactive` 通过 `Proxy` 来实现响应式（数据劫持） ，并通过<font color="red">`Reflect`</font> 操作<font color="red">源对象</font> 内部的数据

**从使用角度对比**：
  - `ref` 定义的数据： 操作数据 `Reflect`<font color="red">需要</font> `.value` 读取数据是模板直接读取`Reflect`<font color="red">不需要</font>`.value`
  - `reactive` 定义的数据： 操作数据于读取数据：<font color="red">均不需要</font>`.value`

## 149 `setup` 的两个注意点：

`setup` 执行的时机
  - 在 `beforeCreate` 之前执行一次， `this` 是 `undefined`

`setup` 的参数
  - `props` ：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - `context` : 上下文对象
    - `attrs` : 值为对象，包含：组件外部传递过来。但是没有在 `props` 配置中声明的属性，相当于 `this.$attrs`
    - `slots` : 收到的插槽内容，相当于 `this.$slots`
    - `emit` :  分发自定义事件的函数，相当于 `this.$emit`

## 150 `computed` 计算属性
`computed` 函数
  与 `vue2 .x` 中的 `computed` 配置功能一致
  写法：

```js
import { computed } from "vue"
setup(){
  //计算属性简写
  let fullName = computed( () =>{
    return person.firstName + '-' + person.lastName
  })
  //计算属性的完整写法
  let fullName = computed( {
    get(){
      return person.firstName + '-' + person.lastName
    },
    set(value){
      const nameArr = value.split('-')
      person.firstName = nameArr[0]
      person.lastName = nameArr[1]
    }
  } )
```

## 151 `watch` 属性
`watch` 函数
  - 与 `vue 2 .x` 中的 `watch` 配置功能一致
  - 两个小坑：
    - 监视 `reactive` 定义的响应式数据时， `oldValue` 无法正确获取，强制开启深度监视（ `deep` 配置失效）
    - 监视 `reactive` 定义的响应式数据中某个属性时， `deep` 配置失效

情况一： 监视 `ref` 定义的响应式数据
```js
//情况一： 监视ref 定义的响应式数据
watch(sum,(newValue,oldValue)=>{
  console.log('sum变化了'，newValue,oldValue)
},{immediate:true})  
```
情况二： 监视多个 `ref` 定义的响应式数据
```js
//情况二： 监视多个ref定义的响应式数据
watch([sum,msg],(newValue,oldValue)=>{
  console.log('sum 或者 msg 变化了'，newValue,oldValue)
},{immediate:true})

```
情况三： 监视 `reactive` 定义的响应式数据
若 `watch` 监视的是 `reactive` 定义的响应式数据，则无法正确获得 `oldValue`
若 `watch` 监视的是 `reactive` 定义的响应式数据，则强制开启了深度监视
```js
//情况三： 监视 reactive 定义的响应式数据
//若 watch 监视的 reactive 定义的响应式数据，则无需正确获得 oldValue
//若 watch 监视的是 reactive 定义的响应式数据，则强制开启了深度监视

watch(person,(newValue,oldValue)=>{
  console.log('person变化了'，newValue,oldValue)
},{immediate:true,deep:true}) //此处的deep 配置不在奏效

```
情况四： 监视的 `reactive` 定义的响应式数据中的某个属性
```js
// 情况四： 监视的reactive 定义的响应式数据中的某个属性
watch( ()=>person.job,(newValue,oldValue)=>{
  console.log('person 的 job 变化了'，newValue,oldValue) 
},{immediate:true,deep:true})

```

## 154 `watchEffect`  监视函数

- `watch` 的套路是： 既要指明监视的属性，也要指明监视的回调
- `watchEffect` 的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性
- `watchEffect` 优点像`computed`：
    - 但是 `computed` 注重的计算出来的回调的值（回调函数的返回值），所以必须要写返回值
    - 而 `watchEffect` 更注重的是过程（回调函数的函数体），所以不用写返回值

```js
//watchEffect 所指定的回调中用到的数据只要发生变化，则直接重新执行回调
watchEffect( ()=>{
	const x1 = sum.value
	const x2 = person.age
  console.log('watchEffect 说指定的回调执行了！')
})
```

## 155 生命周期函数
- `Vue 3.0 `中可以继续使用`vue 2.0` 中的生命周期钩子，但是有两个被更名
	- `beforedestory` 改名为 `beforeUnmount`
	- `destory` 改名为 `unmounted`

- `Vue 3.0` 也提供了 `Composition API` 形式的生命周期钩子，与 `Vue 2.x` 中钩子对应关系如下：
	- `beforeCreate` ====> `setup()`
	- `created` =========> `setup()`
	- `beforeMount` =====> `onBeforeMount`
	- `mounted` =========> `onMounted`
	- `beforeUpdate` ====> `onBeforeUpdate`
	- `updated` =========> `onUpdated`
	- `beforeUnmount` ===> `onBeforeUnmount`
	- `unmounted` =======> `onUnmounted`

## 156 自定义 `hook` 函数

- 什么事 `hook` 函数？ ----本质事一个函数，把 `setup` 函数中使用的 `Composition API` 进行了封装

- 类似于 `Vue 2.x` 中的 `mixin`

- 自定义 `hook` 的优势： 复用代码，让 `setup` 中的逻辑更清楚易懂


## 156 `ToRef` 

作用： 创建一个 `ref` 对象， 其 `value` 值 指向另一个对象中的某个属性值

语法：` const name = toRef(person,'name')`

应用：要将响应式对象中的某个属性单独提供给外部使用时。

扩展： `toRefs` 与 `toRef` 功能一致，但可以批量创建多个 `ref` 对象，语法：`toRefs(person)`

# 158 其他 `Componsition API` 

## 1 `shallowReactive` 与 `shallowRef` 
- **`shallowReactive`** : 只处理对象最外层属性的响应式（浅响应式）
- **`shallowRef`** : 只处理基本数据类型的响应式，不进行对象的响应式处理
- **什么时候用？**
	如果有一个对象数据，结构比较深，但变化时只是外层属性变化 ===》 `shallowReactive`
	如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ====》 shallowRef

## 159 

## 2 `readonly` 与 `shallowReadonly`
 
- **`readonly`** : 让一个响应式数据变为只读的（ 深只读 ）

- **`shallowReadonly`** ： 让一个响应式数据变为只读的。（ 浅只读 ）

- **应用场景**： 不希望数据被修改时，比如别人的数据，你只能用，但是不能修改。

## 160 `toRaw` 与 `markRaw`
- **`toRaw`** : 
	- **作用**： 将一个由 reactive 生成的 <font color="red">响应式对象</font> 转为 <font color="red">普通对象</font>。
	- **使用场景**： 用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。

- **`markRaw`** : 
	- **作用**：标记一个对象，使其永远不会再成为响应式对象。
	- **应用场景**：
		- 1 有些值不应该设置为响应式的，例如复杂的第三方类库等。
		- 2 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。

## 161 `customRef`	

**作用**：创建一个自定义的 `ref` ，并对其依赖项跟踪和更新触发进行显式控住

实现防抖效果：
```vue
<template>
	<input type="text" v-model="keyWord">
	<h3>{{keyWord}}</h3>
</template>

<script>
	import {ref,customRef} from 'vue'
	export default {
		name: 'App',
		setup() {
			//自定义一个ref——名为：myRef
			function myRef(value,delay){
				let timer
				return customRef((track,trigger)=>{
					return {
						get(){
							console.log(`有人从myRef这个容器中读取数据了，我把${value}给他了`)
							track() //通知Vue追踪value的变化（提前和get商量一下，让他认为这个value是有用的）
							return value
						},
						set(newValue){
							console.log(`有人把myRef这个容器中数据改为了：${newValue}`)
							clearTimeout(timer)
							timer = setTimeout(()=>{
								value = newValue
								trigger() //通知Vue去重新解析模板
							},delay)
						},
					}
				})
			}

			// let keyWord = ref('hello') //使用Vue提供的ref
			let keyWord = myRef('hello',500) //使用程序员自定义的ref
			
			return {keyWord}
		}
	}
</script>
```

## 162 `privide` 与 `inject`
- **作用**： 实现 <font color="red">祖与后代组件间 </font>通信
- **套路**： 父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据
- **具体写法**：
	- 1 祖组件中：

	```js
	setup(){
		//...
		let car = reactive({name:'奔驰',price:"40w"})
		provide('car',car)
		//...
	}
	```
	- 2 孙组件中：
	```js
	setup(props,context){
		//...
		const car = inject('car')
		return { car }
		//...
	}
	```
## 163 6 响应式数据的判断

- `isRef` : 检查一个值是否为一个 `ref` 对象

- `isReactive` : 检查一个对象是否由 `reactive` 创建的相应是代理

- `isReadonly` : 检查一个对象是否由 `isReadonly` 创建的只读代理

- `isProxy` : 检查一个对象是否由 `reactive` 或者 `readonly` 方法创建的代理

```js
let car = reactive({name:'奔驰',price:'40W'})
let sum = ref(0)
let car2 = readonly(car)

console.log(isRef(sum))
console.log(isReactive(car))
console.log(isReadonly(car2))
console.log(isProxy(car))
console.log(isProxy(sum))
```

# `Componsition API`  的优势

## `Options API`（ 配置式 API ） 存在的问题
使用传统`Options API`中，新增或者修改一个需求，就需要分别在`data`，`methods`，`computed`里修改。

## `Componsition API`(组合式 API)  的优势
我们可以更优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

# 新的组件
## `Eragment`（片段、碎片的意思）
- **在vue2 中**，：组件必须由一个根标签
- **在vue3 中**： 组件可以没有根标签，内部会将多个根标签包含在一个`Fragment` 虚拟元素中
- **好处**：减少标签层级，减小内存占用

## `Teleport` 传送
什么是 `Teleport` ？ ----`Teleport` 是一种能够将我们的<font color="red">组件html结构</font> 移动到指定位置的技术
应用： 弹窗组件
```html
<teleport to="移动位置">
	<!-- 遮罩层 -->
	<div v-if="isshow" class="mask">
		<!-- 弹窗内容区 -->
		<div class="dialog">
			<h3 >我是一个弹窗</h3>
			<button @click="ishow = false">关闭弹窗</button>
		</div>
	</div>
</teleport>
```
```html
<style>
	.mask{
		position: absolute;
		top: 0;bottom: 0;left: 0;right: 0;
		background-color: rgba(0, 0, 0, 0.5);
	}
	.dialog{
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		text-align: center;
		width: 300px;
		height: 300px;
		background-color: green;
	}
</style>
```
## `suspense` 悬念
- 等待异步组件时，渲染一些额外内容，让应用有更好的用户体验 
- 使用步骤：
	- 异步引入组件
	```js
	//静态引入
	//import Child from './components/Child.vue'

	import {defineAsyncComponent} from 'vue'

	//动态引入  或者  异步引入  网速慢的时候区别出来了
	const Child = defineAsyncComponent( () => import('./components/Child.vue'))
	```
	- 使用 `suspense` 包裹组件，并配置好 `default` 和 `fallback`

	```html
	<Suspense>
		<template v-slot:default>
			<Child/>
		</template>
		<template v-slot:fallback>
			<h3>稍等，加载中...</h3>
		</template>
	</Suspense>
	```
# 其他
## 1 全局API 的配置
- vue 2.x 有许多全局API 和配置
	- 例如： 注册全局组件、注册全局指令等

```js
//注册全局组件
Vue.component('MyButton',{
	data:()=>( {
		count: 0
	}),
	templete:`<button @click="count++"> Clicked {{ count}} teimes. </button>`
})
//注册全局指令
Vue.directive('focus',{
	insert:el=>el.focus()
})
```
- `Vue 3.0` 中有这些`API` 做出了调整：

	- 将全局的`API`，即： `Vue.xxx` 调整到应用实例 （`app`）上

	| `2.x `全局 `API` (`vue`) | `3.x` 实例 `API`（`app`)|
	| :-----: |  :-----: |
	| `Vue.config.xxx` | `app.config.xxxx` |
	| `Vue.config.productionTip` | <font color="red"> 移除 </font> |
	| `Vue.component` | `app.component` |
	| `Vue.directive` | `app.directive` |
	| `Vue.mixin` | `app.mixin` |
	| `Vue.use` | `app.use` |
	| `Vue.prototype` | `app.config.globalProperties` |
## 2 其他改变
- `data` 选项应始终被声明为一个函数
	- 过度类名的更改：
		- `Vue 2.x` 写法
		```css
		.v-enter,v-leave-to {
			opacity:0;
		}
		.v-leave,.v-enter-to {
			opacity:1;
		}

		```
		- `Vue 3.x` 写法
		```js
		.v-enter-from,.v-leave-to{
			opacity:0;
		}
		.v-enter-from,.v-leave-to{
			opacity:1;
		}

		```
	- <font color="red"> 移除 </font> `keyCode` 作为 `v-on` 的修饰符，同时也不在支持 `config.keyCodes`
	- <font color="red"> 移除 </font> `v-on.native` 修饰符
		父组件中绑定事件

		```html
		<my-component
			v-on:close="handleComponentEvent"
			v-on:click="handleNativeClickEvent"
		/>
		```

		子组件中声明自定义事件

		```js
		<script>
			export default {
				emits:['close']
			}
		<script/>
		```
	- <font color="red"> 移除 </font> 过滤感（`filter`)
		过滤器虽然看起来很方便，但它需要一个自定义语法，打破大括号内表达式是“只是JavaScript”的假设，这不仅有学习成本，而且有实现的成本！建议用方法调用或计算属性去替换过滤器。
	- ...