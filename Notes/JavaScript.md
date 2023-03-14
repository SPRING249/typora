

## 变量

## 数据类型

变量的数据类型只有在程序运行时，根据等号右边的值来确定。

**动态语言，变量的数据类型是可以变化的。**

分类：

1. 简单数据类型（Number,String,Boolean,Undefined,Null）
2. 复杂数据类型（object）

### 基本数据类型

| 类型名         | typeof 检测结果            | 值举例                       |
| -------------- | -------------------------- | ---------------------------- |
| 数字类型       | number                     | `5`、`2.5`、`-0.5`           |
| 字符串类型     | string                     | `'前端'`、`"后端"`、`'3.14'` |
| 布尔类型       | boolean                    | `true`、`false`              |
| undefined 类型 | undefined                  | `undefined`                  |
| null 类型      | object（可以理解为空对象） | `null`                       |

# 函数

## reduce（）方法

### 语法介绍

**语法**

```javascript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```

**参数值**

| 参数                                             | 描述                                                         |
| :----------------------------------------------- | ------------------------------------------------------------ |
| function(total, currentValue, currentIndex, arr) | 必需。为数组中的每个元素运行的函数。<br />total：必需，initialValue，或函数先前返回的值。<br />currentValue:必需，当前元素的值<br />index：可选，当前数组索引<br />arr:可选，当前数组对象 |
| initialValue                                     | 可选，作为初始值传递给函数的值                               |

### 用法

#### 求和，求乘积

```
var  arr = [1, 2, 3, 4];
var sum = arr.reduce((x,y)=>x+y)
var mul = arr.reduce((x,y)=>x*y)
console.log( sum ); //求和，10
console.log( mul ); //求乘积，24
```

#### 计算数组中每个元素出现的次数



# 对象、原型、原型链和继承

## 工厂函数

> ##### 	抽象工厂模式（英语：Abstract factory pattern）是一种软件开发设计模式。抽象工厂模式提供了一种方式，可以
>
> ##### 将一组具有同一主题的单独的工厂封装起来。在正常使用中，客户端程序需要创建抽象工厂的具体实现，然后使用抽
>
> ##### 象工厂作为接口来创建这一主题的具体对象。客户端程序不需要知道（或关心）它从这些内部的工厂方法中获得对象
>
> ##### 的具体类型，因为客户端程序仅使用这些对象的通用接口。抽象工厂模式将一组对象的实现细节与他们的一般使用分
>
> ##### 离开来。

​	抽象工厂==模具，只能根据模具生产商品，却不可以修改模具

缺点：无法区分对象类型

```js
 function createObject(name, age, info) {
            const obj = {}
            obj.name = name
            obj.age = age
            obj.running = function () {
                console.log(info)
            }
            return obj
        }

        const person = createObject('monent', 18, '跑步')
        const friend = createObject('JoJo', 20, '喜欢看电视')

        person.running()
        friend.running()
```



## 构造函数模式

- 没有显示地创建对象

- 属性和方法直接赋值给了this

- 没有return

  

```js
        function Person(name, age, info) {
            this.name = name
            this.age = age

            this.running = function () {
                console.log(info)
            }

        }

        const person = new Person("mon", 18, "我想睡觉")
        const student = new Person("pan", 18, "我要学习")

        person.running()
        student.running()
```

- 要创建Person实例，应使用new操作符

  1. 在内存创建一个新对象

  2. 这个新对象内部的[[prototype]](proto)特性被赋值为构造函数的prototype属性；

     ​	`person.proto=Person.prototype`

  3. 构造函数内部的this被赋值为这个新对象[[this指向新对象]]()

  4. 执行构造函数内部的代码。`person.proto.name='mon'`

  5. 如果构造函数返回非空对象，则返回该对象；否则，返回刚创建的新对象

```js
// instanceof 运算符用来检测 constructor.prototype 是否存在于参数 object 的原型链上
// person和student的隐式原型都指向Person的显式原型;
console.log(person.__proto__ === student.__proto__); //true
console.log(person instanceof Object); //true
console.log(person instanceof Person); //true
console.log(student instanceof Object); //true
console.log(student instanceof Person); //true

// 以上代码的代码实际上执行的该例子
console.log(student.__proto__.__proto__.constructor.prototype === Object.prototype);
console.log(student.__proto__.constructor.prototype === Person.prototype);
```

**问题：**虽然person和student都有一个方法running，但是这两个方法不是同一个Function实例，指向的内存也各不相同

```js
console.log(student.running === person.running); //false
```

**由于没必要的内存损耗，于是出现了原型模式**

## 原型模式

- 每个函数都会创建一个prototype属性，这个属性是一个对象，该对象可以给每个实例共享属性和方法

  ```js
  		function Person(name, age) {
              this.name = name
              this.age = age
          }
  
          Person.prototype.running = function (info) {
              console.log(info)
          }
          // 在这里,把方法running定义在了Person的prototype属性上
          // person和student共享同一个原型方法running,指向的是同一快内存空间
          const person = new Person("mon", 18)
          const student = new Person("pan", 18)
  
          console.log(person.running === student.running) //false
  
          person.running('我要睡觉')
          student.running('我想学习')
  ```

  

## 原型层级

![Js Object LayOut](../../assets/Js%20Object%20LayOut.jpg)!](../typora/assets/Js%20Object%20LayOut.jpg)

### ![6531713-dcca7e7c3f63594f.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15e1efb250be47d49d5ec81a5ce6f1ed~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

### Functon和Object 的关系

1. 在js中，每个JavaScript函数实际上都是一个Function对象

2. 在JavaScript中，几乎所有对象都是**Object**类型的实例，它们都会从**Object.prototype**继承属性和方法

3. 从原型链上讲，Function 继承了 Object

4. 从构造器上讲，Function 构造了 Object

   ```js
    function Person(name, age) {
               this.name = name
               this.age = age
   
           }
   
           Person.prototype.running = function (info) {
               console.log(info)
   
           }
           const obj = {}
           const person = new Person("潘博文", 18)
           const student = new Person("dog", 17)
   
           console.log(obj.__proto__ === Object.prototype)
           //Object的构造函数
           console.log(Object.constructor === Function)
           console.log(Function.prototype.__proto__ === Object.prototype)
           console.log(Function.constructor === Function)
           console.log(Person.__proto__.constructor.__proto__ === Function.prototype)
           console.log(student.__proto__.__proto__.constructor.__proto__ === Function.prototype)
   
           console.log(Function.constructor.__proto__ === Function.prototype)
           console.log(Object.constructor === Function.constructor)
           console.log(Object instanceof Function)
           console.log(Function instanceof Object)
           console.log(Function.prototype.__proto__===Object.prototype)
   
   ```

- ###### 原型查找机制,首先在实例上查找,有则返回,没有则往上查找,这时候查找到了原型上了,如果能找到有该属性或方法,则返回,如果

  ###### 仍然没找打,就在**Object**的原型上查找,找到了则返回,没有就返回**undefined**或者报错。

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayName = function () {
    console.log("我是在实例身上的");
  };
  this.memory = "我是属于实例的";
}
// 在这里,把方法running定义在了Person的prototype属性上了
Person.prototype.running = function () {
  console.log("我是原型上的方法");
};

Object.prototype.牛逼 = "这是真的";

Person.prototype.memory = "我是属于原型的";

const person = new Person("moment", 18);

console.log(person.name); // 来自实例
console.log(person.memory); // 来自实例
person.sayName(); // 来自实例
person.running(); // 来自原型
console.log(person.牛逼); // 这是真的
console.log(person.六六六); // undefined
```

