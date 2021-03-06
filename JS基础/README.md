# JS基础

## [变量](变量类型和类型转换.md)

## [this](this.md)

## [函数](函数.md)

## [对象](对象.md)

## [原型链与继承](原型链与继承.md)

## [正则](正则.md)

## [事件队列](事件队列.md)

## [DOM](DOM.md)

## [BOM](BOM.md)


## 常见问题

### eval 是做什么的？

eval 的功能是把对应的字符串解析成 JS 代码并运行

 - eval不安全，若有用户输入会有被攻击风险
 - 非常耗性能（先解析成 js 语句，再执行）



### ['1', '2', '3'].map(parseInt) 答案是多少？

答案 [1, NaN, NaN]

map会给函数传递3个参数： (elem, index, arry)

parseInt接收两个参数(sting, radix)，其中radix代表进制。省略 radix 或 radix = 0，则数字将以十进制解析

因此，map 遍历 ["1", "2", "3"]，相应 parseInt 接收参数如下

```js
parseInt('1', 0);  // 1
parseInt('2', 1);  // NaN
parseInt('3', 2);  // NaN
```


### [严格模式的限制](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode/Transitioning_to_strict_mode)
 
 - 变量必须声明后再使用
 - 函数的参数不能有同名属性，否则报错
 - 不能使用 with 语句
 - 不能对只读属性赋值，否则报错
 - 不能使用前缀 0 表示八进制数，否则报错
 - 不能删除不可删除的属性，否则报错
 - 不能删除变量 delete prop，会报错，只能删除属性 delete global[prop]
 - eval 不会在它的外层作用域引入变量
 - eval 和 arguments 不能被重新赋值
 - arguments 不会自动反映函数参数的变化
 - 不能使用 arguments.callee
 - 不能使用 arguments.caller
 - 禁止 this 指向全局对象
 - 不能使用 fn.caller 和 fn.arguments 获取函数调用的堆栈
 - 增加了保留字（比如 protected、static 和 interface）


### Ajax
Asynchronous Javascript And XML 异步传输+js+xml

 - 创建 XMLHttpRequest 对象,也就是创建一个异步调用对象
 - 建一个新的 HTTP 请求,并指定该 HTTP 请求的方法、URL 及验证信息
 - 设置响应 HTTP 请求状态变化的函数
 - 发送 HTTP 请求
 - 获取异步调用返回的数据

```js
function ajax(url, handler){
  var xhr;
  xhr = new XMLHttpRequest();

  xhr.onreadystatechange = function() {
    if(xhr.readyState == 4 && xhr.status == 200) {
      handler(xhr.responseXML);
    }
  }
  xhr.open('GET', url, true);
  xhr.send();
}
```


### Javascript 垃圾回收方法

标记清除（mark and sweep）

 - 这是 JavaScript 最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”
 - 垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

引用计数(reference counting)

 - 在低版本 IE 中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个 变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加 1，如果该变量的值变成了另外一个，则这个值得引用次数减 1，当这个值的引用次数变为 0 的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为 0 的值占用的空间

参考链接 [内存管理-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Memory_Management)



### 哪些操作会造成内存泄漏？

 - JavaScript 内存泄露指对象在不需要使用它时仍然存在，导致占用的内存不能使用或回收
 - 未使用 var 声明的全局变量
 - 闭包函数(Closures)
 - 循环引用(两个对象相互引用)
 - 控制台日志(console.log)
 - 移除存在绑定事件的 DOM 元素(IE)