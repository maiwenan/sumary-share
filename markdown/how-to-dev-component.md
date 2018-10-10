## 组件开发总结



### 确定开发什么组件
* 多个页面使用到同一个功能模块
* 为了方便维护，需要把一个页面拆分成多个组件
* 根据组件库设计图，专门开发一套组件


###### 复用组件
<table>
  <tr>
    <td>![复用组件1](/images/how-to-dev-component/1.png)</td>
    <td>![复用组件2](/images/how-to-dev-component/2.png)</td>
  </tr>
</table>


###### 拆分组件
![拆分组件](/images/how-to-dev-component/3.jpg)

推荐阅读：[thinking-in-react](https://reactjs.org/docs/thinking-in-react.html)


###### 组件库组件
清晰划分好需要开发的组件，可直接看设计规范



### 组件的功能
假如要开发 `Select` 组件，第一个思考的是什么？
<table>
  <tr>
    <td>![4](/images/how-to-dev-component/4.jpg)</td>
    <td style="vertical-align: top;">实现逻辑？<br/>架构设计？<br/>输入输出接口？<br/>其他？</td>
  </tr>
</table>


##### 功能场景
- 根据设计图确定功能场景（已知场景）
- 从产品给出的需求确定功能场景（已知场景）
- 参考竞品的功能场景（潜在场景）
- 依据自己的开发经验来评估功能场景是否需要（潜在场景）


###### 根据设计图确定场景
[选择器设计图](https://gitlab.gridsum.com/frontend/gs-ui/component/uploads/a35a98f286e197101005f442f2c31ee1/%E5%9F%BA%E6%9C%AC%E5%8D%95%E5%A4%9A%E9%80%89%E6%8B%A9%E5%99%A8ui.jpg)
* 可以多选
* 可以搜索
* 支持禁用
* 下拉选项支持禁用


###### 从产品给出的需求确定场景（设计图的补充）


###### 参考竞品确定场景
[element 选择器](http://element-cn.eleme.io/#/zh-CN/component/select)
* 支持不同大小
* 可即时输入下拉选项
* 支持下拉选项分组
* 支持远程搜索
* ....


###### 依据经验确定场景
* 是否可清空选项
* 数据量大时，是否需要分页
* 多选时，是否只显示已选个数



### 良好的组件设计



#####  接口设计
- 依据数据交互来确定
- 依据功能场景来确定
- 参考竞品组件的接口设计


###### 数据交互确定接口
* `sources` 表示数据源
* `value` 表示当前选中的值
* `@change` 表示当前选项变化时触发的事件


###### 场景确定接口
* `multiple` 表示是否可多选
* `searchable` 表示是否可搜索
* `size` 表示不同大小的选择器
* `clearable` 表示可清空选项
* ......


###### 参考竞品组件确定接口
* `multiple-limit` 表示多选时最大选项数
* `loading` 远程搜索时是否展示加载动画
* `@clear` 表示清空选项时触发的事件 


我们可以看一下最终的接口设计：[选择器](http://gs-ui.gridsum.com/#/zh-CN/component/select)



##### 易用性&扩展性

* 结构化数据生成的选择器

    ```
    <gs-select :sources="sources" />    
    ```

* 把结构化数据生成的选择器拆分成 `Select` 和 `Option` 两个组件来组合使用
    
    ```
    <gs-select>
        <gs-option v-for="(item, i) in sources" :key="i" :value="item.value">
            <gs-icon name="icon" />{{ item.label }}
        </gs-option>
    </gs-select>
    ```


###### 优缺点

第一种的优点：

* 简单易用

第一种的缺点：

* 不支持自定义渲染
* 不支持自定义key值

项目中封装类似组件使用第一种会相对容易封装以及容易使用，
组件库中封装使用第二种相对能适应更多的场景


###### 提高易用性的一些做法
- api调用（临时提示类型的组件）

    ```
    message.info('提示信息');
    ```

- 指令调用（临时提示类型的组件，但依赖于页面中其他元素或组件的）

    ```
    <data-list v-loading="loading" />
    ```



##### 异步接口

组件提供的异步钩子函数最好支持回调或者promise，比如 `searchable-method` 方法

```
// 回调
if (this.searchableMethod) {
    this.searchableMethod(searchVal, callback);
}
// promise
if (this.searchableMethod) {
    const promise = this.searchableMethod(searchVal);

    if (promise.then) {
       // ... 
    }
}
```

建议支持 promise，这样使用的时候就不用显式调用 `callback(data)`



##### 接口的数据校验以及校验提示

```
// 使用 vue 提供的组件属性校验
{
    props: {
        value: {
            type: [Number, String, Array],
            default: undefined,
            required: true
        }
    }
}
```

```
// 代码逻辑里的自定义校验
if (process.env === 'development') {
    if (!data.hasProps('name label')) {
        console.warning('data should include name and label props');
    }
}
```



##### 支持多语言

- 提取组件的文案，以 `key-value` 的形式保存到一个对象
- 以每个国家作为一个 key 对应一个 文案对象
- 切换国家时，则修改组件使用的文案对象为当前国家对应的文案对象



##### 主题设计

- 确定主题色，提取颜色变量（字体颜色变量、背景颜色变量、边框颜色变量等）到一个单独的变量文件进行声明
- hover, focus, active等状态的颜色最好是通过 normal 状态的颜色计算出来的
- 组件涉及到数值类的数据是否也以变量的形式表示？如：高度、字体大小、内外边距等



### 可访问性

* 语义化标签
    - 比如导航组件可以使用 `nav` 标签，每一项可以使用 `li` 标签

* 支持键盘操作
    - 如：modal使用 `esc` 键可以关闭
    - 多条目的组件可以使用方向键进行控制

推荐阅读：[可访问性](https://reactjs.org/docs/accessibility.html)



### 文档
- 示例（ui和源码）
- 组件的输入输出（props， slot，api）

[Modal文档示例](http://gs-ui.gridsum.com/#/zh-CN/component/modal)



### 项目构建


###### 代码规范
* 代码规范指南和组件约定 [详情](http://gs-ui.gridsum.com/#/zh-CN/component/component-guide)
* 使用 [`husky`](https://github.com/typicode/husky) 和 [`lint-staged`](https://github.com/okonet/lint-staged) 对修改过的代码进行校验


###### 测试

推荐使用vue官方提供的测试套件 [`@vue/test-utils`](https://vue-test-utils.vuejs.org/zh)

可参考demo：[Filters](https://gitlab.gridsum.com/frontend/components/filters)


###### 抽离第三方依赖类库
- 本身依赖的类库
- 构建时新加的helper类库

```
const dependencies = Object.keys(Object.assign(pkg.peerDependencies, pkg.dependencies));

export default {
  // ...
  externals: function (id, parent) {
    const matchs = dependencies.filter(dep => {
      const re = new RegExp(`^${dep}`);
      
      return re.test(id);
    });
      
    return matchs.length > 0;
  }
}
```


###### 支持按需加载
- 直接引入需要的文件 `import 'xxx/lib/alert'`
- 使用插件进行转译成第一种


###### 构建模式
- es 模块
- cj 模块
- umd 模块

推荐阅读：[国双组件库打包优化总结](https://gitlab.gridsum.com/szfrontend/team/wikis/%E5%9B%BD%E5%8F%8C%E7%BB%84%E4%BB%B6%E5%BA%93%E6%89%93%E5%8C%85%E4%BC%98%E5%8C%96%E6%80%BB%E7%BB%93)



### vue 开发组件技巧

* api调用的自定义渲染实现

    ```
    // 使用方式
    {
        render(h) {
            return (<div>自定义内容</div>);
        }
    }
    ```

* bem 或者 scoped ，开发组件推荐使用 bem 样式命名风格


* npm link 方式测试构建好的组件库包，参考文档：[npm link](https://docs.npmjs.com/cli/link)
    ```
    cd gs-ui
    npm link

    cd my-project
    npm link @gs-ui/gs-ui
    ```



### 后续
* 组件库工作流构建
* 组件的架构设计以及代码实现
* 组件/项目维护、迭代以及发布流程