## 一、React简介
React由Meta公司开发，是一个用于 构建Web和原生交互界面的库
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384160374-896df364-f098-4f9c-822c-87de277913c9.png#averageHue=%23d6efe0&clientId=u8b307f73-4637-4&from=paste&height=758&id=u4b1dc56e&originHeight=948&originWidth=2500&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=961049&status=done&style=none&taskId=uef101cfa-5cfd-4336-b2c1-81a700d11a1&title=&width=2000)
#### 1. React的优势
相较于传统基于DOM开发的优势

1. 组件化的开发方式
2. 不错的性能

相较于其它前端框架的优势

1. 丰富的生态
2. 跨平台支持

#### 2. 开发环境创建
**create-react-app**是一个快速 创建React开发环境的工具，底层由Webpack构建，封装了配置细节
> 1. 安装react的包：npm install -g create-react-app
> 2. 执行命令： npx create-react-app react-basic 
>    1. npx Node.js工具命令，查找并执行后续的包命令
>    2. create-react-app 核心包（固定写法），用于创建React项目
>    3. react-basic React项目的名称（可以自定义）
> 
 
> 3. 清除不需要的内容

创建React项目的更多方式
[https://zh-hans.react.dev/learn/start-a-new-react-project](https://zh-hans.react.dev/learn/start-a-new-react-project)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384179122-4fd8d39b-6431-4fb5-9445-4b1406ad14fc.png#averageHue=%2323282f&clientId=u8b307f73-4637-4&from=paste&height=235&id=u72078ae4&originHeight=294&originWidth=827&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36469&status=done&style=none&taskId=u3efaf2de-4904-44b3-b5ea-4dddd167bca&title=&width=661.6)

1. 核心包

"react": "^18.2.0",
"react-dom": "^18.2.0",

2. index.js

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384196726-d29bd628-2a1f-49b7-97f5-da97c84a4979.png#averageHue=%23242830&clientId=u8b307f73-4637-4&from=paste&height=241&id=ud3655a4c&originHeight=374&originWidth=774&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37484&status=done&style=none&taskId=uadf859fd-c7ee-4a05-adc4-bd60fb95326&title=&width=498.20001220703125)

3. App.js

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384206863-6b6c5485-6059-4b13-99c1-275dae218ba3.png#averageHue=%2323272f&clientId=u8b307f73-4637-4&from=paste&height=235&id=uc56f8885&originHeight=353&originWidth=749&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=23389&status=done&style=none&taskId=u2c7397ea-e70d-4a74-b9b3-e8e8cfc5f6d&title=&width=499.20001220703125)


![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709362315728-a938fe74-73af-4975-a48d-7143af78fb8a.png#averageHue=%23282d35&clientId=u8b307f73-4637-4&from=paste&height=240&id=ub3f93bd5&originHeight=300&originWidth=612&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31525&status=done&style=none&taskId=u32113b09-38c5-4b46-8ab8-ac553f51427&title=&width=489.6)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709362349167-0789227b-779e-4543-84a9-8f9a36cb00b8.png#averageHue=%23252a32&clientId=u8b307f73-4637-4&from=paste&height=318&id=u97a51ec8&originHeight=397&originWidth=501&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27381&status=done&style=none&taskId=u95dce7c1-9dc7-4351-940b-11b832f7106&title=&width=400.8)

## 二、JSX基础
#### 1. 什么是JSX
> 概念：JSX是**JavaScript和XMl(HTML)的缩写**，表示在JS代码中编写HTML模版结构，它是React中构建UI的方式

```jsx
const message = 'this is message'

function App(){
  return (
    <div>
      <h1>this is title</h1>
      {message}
    </div>
  )
}
```

优势：

1. HTML的声明式模版写法
2. JavaScript的可编程能力

#### 2. JSX的本质
> JSX并不是标准的JS语法，它是 JS的语法扩展，浏览器本身不能识别，需要通过**解析工具做解析之后**才能在浏览器中使用

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384231288-85eeeef0-6c9a-4c77-8a97-882c39f141fe.png#averageHue=%23f5f4f2&clientId=u8b307f73-4637-4&from=paste&height=323&id=u61ad70f8&originHeight=404&originWidth=2036&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=217424&status=done&style=none&taskId=u0f08bad7-bf6b-450a-bff8-19d829e19c1&title=&width=1628.8)
#### 3. JSX高频场景-JS表达式
> 在JSX中可以通过 `大括号语法{}` 识别JavaScript中的表达式，比如常见的变量、函数调用、方法调用等等

1. 使用引号传递字符串
2. 使用JS变量
3. 函数调用和方法调用
4. 使用JavaScript对象
5. **注意：if语句、switch语句、变量声明不属于表达式，不能出现在{}中**
```jsx
const count = 100

function getName() {
  return 'jack'
}

function App(){
  return (
    <div>
      <h1>this is title</h1>
      {/* 字符串识别 */}
      {'this is str'}
          
      {/* 变量识别 */}
      {count}
          
      {/* 函数调用 渲染为函数的返回值 */}
      {getName()}
          
      {/* 方法识别 */}
      {new Date().getDate()}
          
      {/* js对象识别 */}
      {/* 外层{}识别样式，内层表示样式对象 */}
      <div style={{ color: 'red' }}> this is div</div>     
    </div>
  )
}
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384247662-f480211b-baac-47a4-af77-f9d70d558391.png#averageHue=%23f6f2f0&clientId=u8b307f73-4637-4&from=paste&height=58&id=u3c0b8039&originHeight=72&originWidth=395&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=3400&status=done&style=none&taskId=u0a06ee2c-83a8-4c5e-a01b-7e3a813cbce&title=&width=316)

#### 4. JSX高频场景-列表渲染
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384259664-38df481a-6096-439b-a37d-69149b78b2b1.png#averageHue=%23fbfbfb&clientId=u8b307f73-4637-4&from=paste&height=292&id=u55d762f7&originHeight=470&originWidth=934&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=160302&status=done&style=none&taskId=uf08fa704-0ecd-44ba-992a-0baa5dd9505&title=&width=579.4000244140625)
> 在JSX中可以使用原生js中的`map方法` 实现列表渲染


```jsx
const list = [
  {id:1001, name:'Vue'},
  {id:1002, name: 'React'},
  {id:1003, name: 'Angular'}
]

function App(){
  return (
    <ul>
      {list.map(item=><li key={item.id}>{item}</li>)}
    </ul>
  )
}
```

**Tips：**

1. 要循环哪个结构，就在map中的return哪个结构
2. 要给dom加独一无二的key（字符串、number），一般用id，React内部框架使用，提升性能

#### 5. JSX高频场景-条件渲染
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384265133-b540fcf8-77c1-4c94-9dc7-9beaa718dbe9.png#averageHue=%23fbfbfa&clientId=u8b307f73-4637-4&from=paste&height=256&id=u7b5b227c&originHeight=350&originWidth=792&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17255&status=done&style=none&taskId=u9227fd8a-1f76-4f55-88dc-6c95ee5bbdf&title=&width=579.6000366210938)
> 在React中，可以通过逻辑与运算符&&、三元表达式(?:) 实现基础的条件渲染


**逻辑与运算符&&：**只要显示一个内容，flag为true，执行后面
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384272500-dce95443-0903-40dc-a60a-0d40475d0202.png#averageHue=%23202a24&clientId=u8b307f73-4637-4&from=paste&height=69&id=ubafe867e&originHeight=86&originWidth=740&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34280&status=done&style=none&taskId=uae0814bb-2985-4fed-961f-6f9d0657ecf&title=&width=592)
**三元表达式(?:)：**要显示两个内容，true显示前面，false显示后面
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384278646-160cc2c3-fe69-4c32-aa9c-cfd819f7932d.png#averageHue=%2325393b&clientId=u8b307f73-4637-4&from=paste&height=59&id=u3a55fdd4&originHeight=74&originWidth=1226&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53988&status=done&style=none&taskId=u9645e76d-5413-406f-a0bd-114a7fea463&title=&width=980.8)
```jsx
const flag = true
const loading = false

function App(){
  return (
    <>
      {flag && <span>this is span</span>}
      {loading ? <span>loading...</span>:<span>this is span</span>}
    </>
  )
}
```

#### 6. JSX高频场景-复杂条件渲染
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384285596-20ded9cf-0dd8-468f-90cf-4fef145f340e.png#averageHue=%23e1dcd9&clientId=u8b307f73-4637-4&from=paste&height=547&id=u5b0dc0ba&originHeight=890&originWidth=756&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=497625&status=done&style=none&taskId=ued8c2d79-b022-4fe8-b509-80a189978fa&title=&width=464.79998779296875)

> 需求：列表中需要根据文章状态适配三种情况，单图，三图，和无图三种模式
解决方案：自定义函数 + 判断语句

```jsx
const type = 1  // 0|1|3

function getArticleJSX(){
  if(type === 0){
    return <div>无图模式模版</div>
  }else if(type === 1){
    return <div>单图模式模版</div>
  }else(type === 3){
    return <div>三图模式模版</div>
  }
}

function App(){
  return (
    <div>
      { getArticleJSX() }
    <div/>
  )
}
```

