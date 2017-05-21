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