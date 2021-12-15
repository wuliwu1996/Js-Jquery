### 首先上结论

  promise比setTimeout执行更先；

### 原因

1. <u>异步任务分为宏任务（macrotask）和微任务</u>，微任务执行完主线程才会执行宏任务；

2. **Promise定义之后便会立即执行，其后的.then()才是属于异步操作的微任务**，而setTimeout()是异步的**宏任务**。而微任务执行顺序在宏任务前。
3. 常见宏任务执行顺序：主代码块 > **setImmediate** > MessageChannel > **setTimeout / setInterval**
4. 常见微任务执行顺序：rocess.nextTick > **Promise** > MutationObserver

### 拓展

#### js是单线程，浏览器 or Node是多线程 

​    js是单线程语言，宿主环境（浏览器ORNode等是多线程的）正是通过**开辟多线程事件驱动**使得js具备了异步的属性。
​    只分配给js一个线程执行任务，常见的耗时操作（网络请求，事件监听，定时器，轮询器等），都是使用其他线程实现，要不然效率太低。
​    异步任务完成后，通过整个程序都具备的事件驱动，主线程执行回调函数（**每个事件都会有相应的回调函数**）。比如：

```javascript
setTimeout(function(){
    console.log(time is out);
}，1000）;
```

![1038932-20180831111726925-1117521653](https://user-images.githubusercontent.com/61279134/146214157-93e75a3c-4f78-4b78-9d77-1bdbdcd21f3e.png)

- 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入**Event Table并注册函数**。
- 当指定的事情完成时，Event Table会将这个函数**移入Event Queue**。
- 主线程内的任务执行完毕为空，会去**Event Queue读取对应的函数，进入主线程执行**。
- 上述过程会不断重复，也就是常说的**Event Loop(事件循环)**。

###参考文章
https://www.cnblogs.com/sunmarvell/p/9564815.html



