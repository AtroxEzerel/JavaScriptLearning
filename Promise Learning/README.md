# Promise从入门到自定义

## 1.前置知识

### 1.1区别实例对象与函数对象

- 实例对象：new函数产生的对象，称为实例对象，简称为对象
- 函数对象：将函数作为对象使用时，简称为函数对象

### 1.2两种类型的回调函数

#### 1.2.1同步回调

- 理解：立即执行，完全执行完了才结束，不会放入回调队列中
- 例子：数组遍历相关的回调函数/Promise的excutor函数

#### 1.2.2异步回调

- 理解：不会立即执行，会放入回调队列中将来执行
- 例子：定时器回调./ajax回调/Promise的成功|失败的回调

### 1.3JS的error处理

#### 1.3.1错误的类型

- Error:所有错误的父类型
- RefrenceError:引用的变量不存在
- TypeError:数据类型不正确的错误
- RangeError:数据值不在其所允许的范围内
- SyntaxError:语法错误

#### 1.3.2错误处理

- 捕获错误：try...catch
- 抛出错误：throw error

#### 1.3.3错误对象

- message属性：错误相关信息
- stack属性：函数调用栈记录信息

## 2.promise的理解与使用

### 2.1Promise是什么

#### 2.1.1理解

- 抽象表达：Promise是JS中进行异步编程的新的解决方案
- 具体表达
  - 从语法上说：Promise是一个构造函数
  - 从功能上说：promsie对象用来封装一个异步操作并可以获取其结果

#### 2.1.2promise的状态改变

- pending 变为 resolved
- pending 变为 rejected

说明 

- 只有这两种 且一个promise对象只能改变一次
- 无论变为成功还是失败都只会有一个结果数据
- 成功的结果数据一般称为value 失败的结果一般称为reason

#### 2.1.3promise的基本流程

![](https://img2022.cnblogs.com/blog/2332774/202204/2332774-20220409163740954-5653776.png)

#### 2.1.4promise的基本使用

```js
//1.创建一个新的promise对象
const p = new Promise((resolve, reject) => {
    //执行器函数
    //2.执行异步任务
    setTimeout(() => {
        const time = Date.now()

        if (time % 2 === 0) {
            //3.1成功调用resolve(value)
            resolve('成功的数据,time：' + time)
        } else {
            //3.2失败调用reject(reason)
            reject('失败的数据,time:' + time)
        }

    }, 1000)
})

p.then(
    //onResolved
    value => {
        //接收得到成功的value数据
        console.log('成功的回调',value);
    },
    //onRejected
    reason => {
        //接收得到失败的reason数据
        console.log('失败的回调',reason);
    }
)
```

### 2.2为什么要使用Promise

#### 2.2.1指定回调函数的方式更加灵活

- 旧的：必须在启动异步任务前指定
- promise：启动异步任务 =>返回promise对象 =>给promise对象绑定回调函数(甚至可以在异步任务结束后指定)

#### 2.2.2支持链式调用 可以解决回调地狱问题

- 什么是回调地狱？：回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调函数执行的条件
- 回调地狱的缺点：不便于阅读/不便于异步处理
- 解决方案：promise链式调用
- 终极解决方案 ：async/await

### 2.3如何使用Promise
#### 2.3.1API

**1.Promise构造函数：Promise(excutor){}**

- excutor函数：执行器(resolve,reject)=>{}
- resolve函数：内部定义成功时我们调用的函数value=>{}
- reject函数：内部定义失败时我们调用的函数reason=>{}

说明：excutor会在Promise内部立即同步回调，异步操作在执行器中执行

**2.Promise.prototype.then方法：(onResolved,onRejected)=>{}**

- onResolved函数：成功的回调函数(value)=>{}
- onRejected函数：失败的回调函数(reason)=>{}

说明：指定用于得到成功value的成功的回调和用于得到失败reason的失败回调

**3.Promise.prototype.catch方法：(onRejected)=>{}**

- onRejected函数：失败的回调函数(reason)=>{}

说明：then()的语法糖，相当于：then(undefined,onRejected)

**4.Promise.resolve方法：(value)=>{}**

- value:成功的数据或promise对象

说明：返回一个成功/失败的promise对象

**5.Promise.reject方法：(reason)=>{}**

- reason:失败的原因

说明：返回一个失败的promise对象

**6.Promise.all方法：(promise)=>{}**

- promises:包含n个promise的数组

说明：返回一个新的promise，只有所有的promise都成功才算成功，只要有一个失败就直接失败

**7.Promise.race方法：(promises)=>{}**

- promises:包含n个promise的数组

说明：返回一个新的promise，第一个完成的promise的结果状态就是最终的状态
