
## 组件开发经验总结

> 不会涉及到技术细节，更多的是经验总结




### 开发什么组件

通常我们是根据项目的实际需要来确定是否要把某个功能封装成一个独立的组件，比如：一个功能多个页面使用到，
那么可以考虑是否把该功能封装成组件



### 确定组件的功能

我们先来看一个图，假如现在我们需要开发一个标签组件，如下图，你们第一个想到的是什么或者会做什么？

> 图片...

实现逻辑？
架构设计？
功能场景？
其他？

###### 功能场景

* 已知场景
* 潜在场景




### 组件的场景

确定开发一个什么组件，根据设计稿或者产品原型确定组件有哪些使用场景





### 良好的组件设计

* 枚举组件的使用场景

* 参考竞品组件
    - 参考接口设计，对应使用场景
    - 参考代码设计架构以及实现方法
    - 参考组件交互方式
    - 参考组件的使用方式
* 组件拆分粒度，提高可扩展性
    - 以menu为例，从传入结构化的数据生成菜单到拆分成 `Menu` 和 `MenuItem`来灵活组合 
* 异步接口支持
    - 组件提供的钩子函数最好支持异步，使用回调或者promise
* 接口的数据校验以及校验提示
* 错误处理，组件需自行处理内部可能出现的错误



### 可访问性

* 语义化标签
* 支持键盘操作
* 支持多语言
* 焦点的定位以及恢复



### 状态机



### 构建

* 文档（哪些内容需要文档化）
    - 示例（ui和源码）
    - 组件的输入输出（props， slot，api）

* test

* 抽离第三方依赖类库
    - 本身依赖的类库
    - 构建时新加的helper类库

* 支持按需加载（两种方式）
    - 直接引入需要的文件 `import 'xxx/lib/alert'`
    - 使用插件进行转译成第一种

* 构建模式
    - es 模块
    - cj 模块
    - umd 模块

* 发布npm包



### vue 开发组件技巧

* 组件易用性
    - api调用（临时提示类型的组件）
    - 指令调用（临时提示类型的组件，但依赖于页面中其他元素或组件的）

* api调用的自定义渲染实现

* 有条件的阻止事件冒泡（待定）

* bem 或者 scoped ？