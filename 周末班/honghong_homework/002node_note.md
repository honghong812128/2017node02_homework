## 1 配置环境变量
- node可以在命令行中执行也是因为这个原理
- 可自定义将文件配置到环境变量下
> 系统->属性->高级系统设置->环境变量->path->将自定义的文件属性中的起始位置中的路径复制后新增到path中


## 2 前端的模块化
- 闭包
- 单例(有且只有一份)
- requirejs(Asynchronous Modules Definition=AMD)是依赖前置; seajs(Common Module Definition)是就近依赖
- commonjs规范-是node模块的实现(require + exports + module)

## 3 node 特点
- 异步IO (定时器 + 回调函数)
- 单线程 (其他语言是多线程) 多线程就是切换上下文，速度很快，看起来像干很多事情
- 进程包含线程，但是node一个进程只包含一个线程

## 4 事件环
- 靠事件驱动

## 5 global (在当前文件夹下可以使用)
- node中没有window属性（前端的全局变量是window 服务端是global）
```
console.log(this);//{}  -->是空数组,不是global
```
- 定时器中的this 是定时器自己

- 闭包中的this是global
```
~function () {
    console.log(this); //闭包中的this才是global
}();
```

- 全局对象-在当前文件夹下可以直接使用的都是全局变量

### console.time() ; console.timeEnd() 可以计算时差
```
console.time();
for (var i = 0; i < 100000000; i++) {
}
console.timeEnd();
//undefined: 151.056ms
```


### setTimeout setInterval (this 代表的是定时器自己)

## 6 变量声明
### const
- 必须初始化,否则会报错
```
const a;
console.log(a);//SyntaxError: Missing initializer in const declaration
```
- 不可以修改
```
const b = 2;
console.log(b);
const b = 3;//Identifier 'b' has already been declared
```

- 只要内存不变可以更改内容
```
const a = [];
console.log(a);//[]
a.push('1');
console.log(a);['1']
```
### var 
- 可以不用初始化，使用时会提示undefined，不会报错
- 可以修改

### let
- 写代码不要丢掉 let

### 作用域 {}
- 同一个“作用域 {}”下 变量不能声明两次
```
function  sum() {
    let a = 1;
    let b = 2;
    let a = 3;//SyntaxError: Identifier 'a' has already been declared
    return a+ b;
}
sum();
console.log(sum());
```

- 如果使用了{} 必须要写return

```
// function a(a) {
//     return function (b) {
//         return a + b;
//     }
// }
let sum = a => b => a + b;
console.log(sum(1)(2));
```



#### 7 改变this的方法
- 改变函数中的this指向 (call,apply)会导致函数执行

```
let obj = {
    color: 'red'
};

function add() {
    console.log(this.color);
}

add.bind(obj)();//red
add();//undefined
```

- bind只能绑定一次

```
function a() {
    console.log(this);
}
var fn = a.bind(1).bind(2);
fn();//[Number: 1]
```

- vat that = this保存this指向

- 使用箭头函数处理this，因为自己家没有this指向 从而解决了this问题
```
let obj = {
    a: function () {
        setTimeout(()=> {
            console.log(this === obj);
        }, 1000);
    }
};
obj.a();//true
```


#### 8 传递参数 可以从第二个参数开始传递实参
```
// 获取参数列表(将arguments转化成数组)
// 1.[].slice.call(arguments,1);
// 2.Array.from(arguments).slice(1);
// 3.剩余运算 ...args 将其他参数转换成数组类型 （拓展 展开运算符） [...arr,...arr1]
function eat(what, ...args) { //arguments(实参的集合)
    //获得除了第一个参数 后面的所有参数

    console.log(arguments); // 类数组 { '0': '香蕉', '1': '苹果', '2': '橘子' }

    console.log([].slice.call(arguments, 1)); //[ '苹果', '橘子' ]

    console.log(Array.from(arguments).slice(1)); //[ '苹果', '橘子' ]

    console.log(args);//[ '苹果', '橘子' ]

    console.log(what);// '香蕉'

}
setTimeout(eat, 1000, '香蕉', '苹果', '橘子');
```



## 9 this的值到底是什么
- this 就是你call一个函数时，传入的context
- 如果你调用的函数不是call形式,请按照自动转化方式转化
```
func(p1, p2);//等价于 func.call(undefined, p1, p2);
   
obj.child.method(p1, p2);//等价于 obj.child.method.call(obj.child, p1, p2)

func.apply(context, p1, p2);

```


```
//e.g. 1
function func() {
   console.log(this)
}
func();//等价于func.call(undefined) this 就是window


//e.g. 2
var obj = {
   foo: function () {
       console.log(this)
   }
};
var bar = obj.foo;
obj.foo();// 转换为 obj.foo.call(obj)，this 就是 obj
bar();//转换为 bar.call(undefined) this 就是 window

// e.g. 3
function fn() {
   console.log(this)
}
function fn2() {
   console.log(this)
}
var arr = [fn, fn2];
arr[0]();// 相当于 arr.0.call(arr)
```

## 10 setImmediate VS setTimeout  异步
- 传时间 =
- 不传时间 setImmediate <= setTimeout

## 11 process 进程
###
- process.pid
- process.kill(process.pid)//结束进程

### 区分开发环境和线上环境
- 在运行文件所在的路径下打开命令窗口 运行 node 文件名（e.g. node callback.js 线上环境）
```
if(process.env.NODE_EVN === 'development'){
    console.log('开发环境');
}else{
    console.log('线上环境');
}
```

- set NODE_EVN=development（命令行输入后）
```
node 文件名（开发环境）
```

### process.nextTick 下一队列（当前队列底部）
```
process.nextTick(function () {
    console.log('nextTick');
});
setImmediate(function () {
    console.log('setImmediate');
});
//重要的事情放在nextTick中，稍微不重要的setImmediate
```

## 12 module  commonjs 规范（模块）
 
### A) 使用模块(require 使用模块)

#### 1) 文件模块 -(引用需要相对路径 ./   ../ )
    
#### 2) 第三方模块 直接写模块名字 不需要 ./ ../

#####  a) 全局的第三方模块 (加了-g 只能在命令行使用)
- nrm 
```
npm install nrm -g // 安装nrm
nrm ls // 列出所有可用源
nrm ls // 用哪个源
```



- nodeppt 模块 --网页PPT

- http-server -- 启动服务的，可以用来调试

##### b) 本地第三方模块 (在当前文件夹下使用)
- 开发依赖  (只是开发时使用 比如 less babel)
```
--save-dev // 或者-D
```
- 发布依赖  (上线开发全部都需要 比如mime)
```
--save // 或者 -S
```

- 安装之前 需要初始化package.json
```
npm init -y// 初始化时不能有汉字、不能用特殊符号、不能命名为下载的模块（不能安自己）
// 如果外层有node_modules 会向上安装
// 不能包含注释内容
```

- 安装文件
    会默认安装到node_modules 下, require 的时候会自动去 node_modules 下查找

- 卸载模块
```
npm uninstall jquery --save //怎样安装的就怎样卸载
```


#### 3) 核心模块 -(内置模块)


### B) 定义模块
- 在node中，一个JS文件就是一个模块
### C) 导出模块
- exports 
- module.exports





