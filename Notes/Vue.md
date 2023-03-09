# Vue生命周期

![](D:/Desktop/%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

# Vue组件

## 组件通信

### 父子组件传参

#### props+$emit

​	props向下传递（接收数据），事件向上传递（发送消息）

```javascript
<template>
  <div class="parent">
    <h1>{{ msg }}</h1>
    <p>爸爸的资产：{{ money }}</p>
    <p>儿子的名字：{{ sonName }}</p>
    <h1>使用$event参数</h1>
    <my-son :name="sonName" @updateDemo="money-=$event" @update:name="sonName=$event"></my-son>
    <h1>使用方法接收参数</h1>
    <my-son :name="sonName" v-on:updateDemo="spendMoney" @update:name="sonName=$event"></my-son>
    <my-son :name="sonName" @updateDemo="spendMoney" @update:name="sonName=$event"></my-son>
    <h1>使用sync双向绑定</h1>
    <my-son :name.sync="sonName" @updateDemo="spendMoney"></my-son>
  </div>
</template>

<script>
import MySon from "@/components/MySon.vue";

export default {
  name: "MyFather",
  components: {
    MySon
  },
  data() {
    return {
      msg: '父组件',
      money: 100,
      sonName: 'JoJo'
    }
  },
  methods: {
    spendMoney(money) {
      this.money -= money
    }
  }
}
</script>

<style scoped>

</style>
```

```javascript
<template>
  <div class="parent">
    <h1>{{ msg }}</h1>
    <p>爸爸给我起的名子为：{{ name }}</p>
    <button @click="getMoney(10)">我花我爸十块钱</button>
    <button @click="changeName">我要改名</button>
  </div>
</template>

<script>
export default {
  name: "MySon",
  props: {
    name: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      msg: '子组件'
    }
  },
  methods: {
    getMoney(money) {
      this.$emit('updateDemo', money)
    },
    changeName() {
      this.$emit('update:name', "柯南")
    }
  }
}
</script>

<style scoped>

</style>
```

#### $parent

```javascript
changeName() {
      this.$parent.sonName = '柯南'
    }
```

#### provide+inject

对于层级比较复杂的组件，父组件通过provide提供数据，子组件使用inject注入数据

provide:一个对象或返回一个对象的函数

inject选项是一个字符串数组或一个对象，对象的key是本地的绑定名

# Vue插件

# Vue脚手架

## ref属性

### 作用

​	原生JavaScript：`document.getElementById`('#xx')

ref:给元素或子组件注册引用信息---获取DOM元素，获取组件实例对象

### 语法

`ref="refName"`

`this.$refs.refname`

### 子组件用法

```javascript
// 子组件Person.vue
<template>
  <div>
    <h2>{{msg}}</h2>
  </div>
</template> 

<script>
export default { 
  data() { 
    return {
      msg: 'Hello world'
    }
  }
}
</script>

```

```js
// 父组件
<template>
  <div>
    <Person ref="childRef" />
    <button @click="getChild">点击获取上方子组件</button> 
  </div>
</template>

<script>
import Person from './Person.vue'
export default { 
  components: {
    Person,
  },
  methods:{
    getChild(){
      console.log(this.$refs.childRef);
    }
  }
}
</script>

```



## props配置项

1. 功能：让组件接收外部传过来的数据

2. 传递数据：`<Demo name="xxx"/>`

3. 接收数据：

   1. 第一种方式（只接收）：`props:['name']`

   2. 第二种方式（限制数据类型）：`props:{name:String}`

   3. 第三种方式（限制类型、限制必要性、指定默认值）：

      ```javascript
      props:{
          name:{
          	type:String, //类型
              required:true, //必要性
              default:'JOJO' //默认值
          }
      }
      ```

      

## mixin混入

## plugin插件

## scoped样式

## TODOList案例

- ### 组件化编码流程：

1. 拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突

2. 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
   1. 一个组件在用：放在组件自身即可
   2. 一些组件在用：放在他们共同的父组件上（状态提升）
3. 实现交互：从绑定事件开始

- ### props适用于：

1. 父组件 ==> 子组件 通信
2. 子组件 ==> 父组件 通信（要求父组件先给子组件一个函数）

- **使用v-model时要切记：**v-model绑定的值不能是props传过来的值，因为props是不可以修改的

- props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做

  

## WebStorage

未登录显示搜索历史：浏览器本地存储存到了硬盘上

存储内容大小一般支持5MB左右（不同浏览器可能还不一样）

浏览器端通过Window.sessionStorage和Window.localStorage属性来实现**本地存储机制**

**相关API：**

​	`xxxStorage.setItem('key', 'value')`：该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新

其对应的值

​	`xxxStorage.getItem('key')`：该方法接受一个键名作为参数，返回键名对应的值

​	`xxxStorage.removeItem('key')`：该方法接受一个键名作为参数，并把该键名从存储中删除

​	`xxxStorage.clear()`：该方法会清空存储中的所有数据

**备注：**

​	SessionStorage存储的内容会随着浏览器窗口关闭而消失

​	LocalStorage存储的内容，需要手动清除才会消失

​	`xxxStorage.getItem(xxx)`如果 xxx 对应的 value 获取不到，那么getItem()的返回值是null

​	`JSON.parse(null)`的结果依然是null




## 自定义事件

### 绑定

```js
<template>
  <div>
    <h1>{{ msg }}</h1>
    <!--通过父组件给子组件传递函数类型的props:实现子给父传递数据-->
    <my-school :getSchoolName="getSchoolName"></my-school>
    <!--通过父组件给子组件绑定一个自定义事件实现：子给父传递数据-->
    <!--<my-student @atguigu="demo"></my-student>-->
    <!--自定义事件：ref,更灵活-->
    <my-student ref="student"></my-student>
  </div>
</template>
<script>
export default {
  name: "App",
  components: {
    MyStudent,
    MySchool,

  },
  data() {
    return {
      msg: '你好呀'
    }
  },
  methods: {
    getSchoolName(name) {
      console.log('App受到了学校名', name)
    },
    //里面的组件实例对象就是App的组件实例对象
    // getStudentName(name, ...params) {
    //   console.log('App受到了学生名', params.name)
    // },
    demo() {
      console.log('demo被调用了')
    }
  },
  mounted() {
    // this.$refs.student.$on('atguigu', this.getStudentName)
    this.$refs.student.$on('atguigu', (name, ...params) => {
      console.log('App收到了学生名：', name, params)
      console.log(this)
      this.studentName = name
    })

    // 只触发一次
    // this.$ref.student.$once('atguigu', this.getStudentName)
  }
}
</script>
```

### 总结

1. 一种组件间通信的方式，适用于：==子组件 > 父组件

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中）

3. 绑定自定义事件：

   ​	第一种方式，在父组件中：<Demo @atguigu="test"/> 或 <Demo v-on:atguigu="test"/>

   ​	第二种方式，在父组件中：

   ```js
   <Demo ref="demo"/>
   ...
   mounted(){
       this.$refs.demo.$on('atguigu',data)
   
   ```

   ​			**若想让自定义事件只能触发一次，可以使用once修饰符，或$once方法**

4. 触发自定义事件：`this.$emit('atguigu',数据)`

5. 解绑自定义事件：`this.$off('atguigu')`

6. 组件上也可以绑定原生DOM事件，需要使用native修饰符


> 注意：通过this.$refs.xxx.$on('atguigu',回调)绑定自定义事件时，回调要么配置在
>
> methods中，要么用箭头函数，否则this指向会出问题！

## 全局事件总线

任意组件间的数据通信

> 必须满足以下条件：
>
> 1. 所有的组件对象都必须能看见他
> 1.  这个对象必须能够使用`$on`、`$emit`和`$off`方法去绑定、触发和解绑事件
