# node基础知识
## node可以在命令行下执行的原因：
将可执行文件配置到了环境变量下

## node是服务端运行js的环境
可以让js跑在服务端里，操作文件系统，模块，包，操作系统API
## 前端模块化
- 闭包
- 单例模式（不能解决命名冲突，调用时代码过长）
- requirejs(AMD)依赖前置  seajs(CMD)就近依赖，需要的时候再引进来这个模块
- commonjs规范，node模块的实现
## node特点
- 异步IO（定时器，回调函数）
- 单线程
（多线程实现：切换执行上下文，因为执行速度很快，所以感觉再做很多件事情）
- 进程（包含线程），node一个进程只能开一个线程
## 事件环
- 靠事件驱动（当前的事件和之后的事件）

## 全局对象
前端的全局变量是window，服务端的是全局对象，是global
在当前文件夹下可以直接使用的，都是全局对象
例如：`console`
### 全局对象的组成
global上的属性，和五个特殊的形参。
**node中没有window属性**
node中的this是{}。
- var 不能直接将内容挂载在global上，
- 没有var声明的，会挂载在global上。