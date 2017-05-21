## 配置环境变量
- node可以在命令行中执行也是因为这个原理
- 可自定义将文件配置到环境变量下
> 系统->属性->高级系统设置->环境变量->path->将自定义的文件属性中的起始位置中的路径复制后新增到path中


## 前端的模块化
- 闭包
- 单例(有且只有一份)
- requirejs(Asynchronous Modules Definition=AMD)是依赖前置; seajs(Common Module Definition)是就近依赖
- commonjs规范-是node模块的实现(require + exports + module)

## node 特点
- 异步IO (定时器 + 回调函数)
- 单线程 (其他语言是多线程) 多线程就是切换上下文，速度很快，看起来像干很多事情
- 进程包含线程，但是node一个进程只包含一个线程

## 事件环
- 靠事件驱动

## global (在当前文件夹下可以使用)
- node中没有window属性
- console.log(this);//{}  -->是空数组,不是global
```
console.log(this);//{}  -->是空数组,不是global
```
- 定时器中的this 是定时器自己

## const
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
## var 
- 可以不用初始化，使用时会提示undefined，不会报错
- 可以修改

## let
- 写代码不要丢掉 let

## 作用域 {}
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
