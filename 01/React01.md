

# React01

day1

## 一、React简介

React由Meta公司开发，是一个用于 构建Web和原生交互界面的库
![image.png](E:\typo\BPP\React.assets\01.png)



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
>    3.  react-basic React项目的名称（可以自定义）
> 3. 清除不需要的内容

创建React项目的更多方式
[https://zh-hans.react.dev/learn/start-a-new-react-project](https://zh-hans.react.dev/learn/start-a-new-react-project)

![image-20240229184333042](E:\typo\BPP\React.assets\image-20240229184333042.png)

1. 核心包

"react": "^18.2.0",

  "react-dom": "^18.2.0",



2. index.js

![image-20240229185258770](E:\typo\BPP\React.assets\image-20240229185258770.png)



3. App.js

![image-20240229185245393](E:\typo\BPP\React.assets\image-20240229185245393.png)



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

![image.png](E:\typo\BPP\React.assets\03.png)

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

![image-20240229195944063](E:\typo\BPP\React.assets\image-20240229195944063.png)



#### 4. JSX高频场景-列表渲染

<img src="E:\typo\BPP\React.assets\04.png" alt="image.png" style="zoom:67%;" />



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

Tips：

1. 要循环哪个结构，就在map中的return哪个结构
2. 要给dom加独一无二的key（字符串、number），一般用id，React内部框架使用，提升性能



#### 5. JSX高频场景-条件渲染

![image.png](E:\typo\BPP\React.assets\05.png)

> 在React中，可以通过逻辑与运算符&&、三元表达式(?:) 实现基础的条件渲染



逻辑与运算符&&：只要显示一个内容，flag为true，执行后面

![image-20240229201149123](E:\typo\BPP\React.assets\image-20240229201149123.png)

三元表达式(?:)：要显示两个内容，true显示前面，false显示后面

![image-20240229201234209](E:\typo\BPP\React.assets\image-20240229201234209.png)

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

<img src="E:\typo\BPP\React.assets\06.png" alt="image.png" style="zoom: 33%;" />



> 需求：列表中需要根据文章状态适配三种情况，单图，三图，和无图三种模式
> 解决方案：自定义函数 + 判断语句



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



## 三、React的事件绑定

[js中的匿名函数的作用以及用法讲解_js 匿名函数 作用域-CSDN博客](https://blog.csdn.net/Lynn_yu/article/details/111933773)



#### 1. 基础实现

> React中的事件绑定，通过语法 `on + 事件名称 = { 事件处理程序 }`，整体上遵循驼峰命名法

```jsx
function App(){
  const clickHandler = ()=>{
    console.log('button按钮点击了')
  }
  
  return (
    <button onClick={clickHandler}>click me</button>
  )
}
```



#### 2. 使用事件参数

> 在事件回调函数中设置形参e即可

```jsx
function App(){
  const clickHandler = (e)=>{
    console.log('button按钮点击了', e)
  }
  return (
    <button onClick={clickHandler}>click me</button>
  )
}
```



#### 3. 传递自定义参数

> 语法：事件绑定的位置改造成箭头函数的写法，在执行clickHandler实际处理业务函数的时候传递实参

```jsx
function App(){
  const clickHandler = (name)=>{
    console.log('button按钮点击了', name)
  }
  return (
    <button onClick={()=>clickHandler('jack')}>click me</button>
  )
}
```


**注意：不能直接写函数调用，这里事件绑定需要一个函数引用**  
**onClick={()=>clickHandler('jack')}**



#### 4. 同时传递事件对象和自定义参数

> 语法：在事件绑定的位置传递事件实参e和自定义参数，clickHandler中声明形参，注意顺序对应

```jsx
function App(){
  const clickHandler = (name,e)=>{
    console.log('button按钮点击了', name,e)
  }
  return (
    <button onClick={(e)=>clickHandler('jack',e)}>click me</button>
  )
}
```



## 四、React组件基础使用



概念：一个组件就是一个用户界面的一部分，它可以有自己的逻辑和外观，组件之间可以互相嵌套，也可以服用多次

可以把一个庞大的应用分成一个个小部分

<img src="E:\typo\BPP\React.assets\07.png" alt="image.png" style="zoom: 33%;" />

#### 1. 组件基础使用

> 在React中，一个组件就是**首字母大写的函数**，内部存放了组件的逻辑和视图UI, 渲染组件只需要把组件当成**标签**书写即可

```jsx
// 1. 定义组件
function Button(){
  return <button>click me</button>
}
// 或者箭头函数也可以，只要是首字母大写的函数就可以
const Button = () => {
  return <button>click me</button>
}

// 2. 使用组件
function App(){
  return (
    <div>
      {/* 自闭和 */}
      <Button/>
      {/* 成对标签 */}
      <Button></Button>
    </div>
  )
}
```



## 五、组件状态管理-useState

#### 1. 基础使用

> useState 是一个 React Hook（函数），它允许我们向组件添加一个`状态变量`, 从而控制影响组件的渲染结果
> 和普通JS变量不同的是，**状态变量一旦发生变化组件的视图UI也会跟着变化（数据驱动视图）**

<img src="E:\typo\BPP\React.assets\08.png" alt="image.png" style="zoom:33%;" />

```javascript
导包
import { useState } from 'react'

function App() {
    // const状态变量  setCount 修改状态变量的方法
  const [count, setCount] = useState(0)  //useState返回一个数组，然后进行解构
  const handleClick = () => {
    setCount(count + 1)
  }

  return (
    <div className="App">
      <button onClick={handleClick}>{count}</button>
    </div>
  );
}
```

简写

```jsx
function App(){
  const [ count, setCount ] = React.useState(0)
  return (
    <div>
      <button onClick={()=>setCount(count+1)}>{ count }</button>
    </div>
  )
}
```



#### 2. 状态的修改规则

> 在React中状态被认为是只读的，我们应该始终`替换它而不是修改它`, 直接修改状态不能引发视图更新

<img src="E:\typo\BPP\React.assets\09.png" alt="image.png" style="zoom: 33%;" />

#### 3. 修改对象状态

> 对于对象类型的状态变量，应该始终给set方法一个`全新的对象` 来进行修改
>
> 修改set方法就相当于是替换状态变量

<img src="E:\typo\BPP\React.assets\10.png" alt="image.png" style="zoom: 50%;" />

**对于复杂类型**

给set方法传一个全新的对象，展开运算符先把原来的状态变量搞过来，在修改要替换的，新值覆盖掉原来的

```javascript
import { useState } from 'react'
function App() {
  const [form, setForm] = useState({ name: 'jack' })
  const click = () => {
    setForm({
      ...form,
      name: 'joy'
    })
  }

  return (
    <div className="App">
      <button onClick={click}>{form.name}</button>

    </div>
  );
}
```



## 六、组件的基础样式处理

> React组件基础的样式控制有俩种方式，行内样式和class类名控制

行内：多单词驼峰写法

```jsx
<div style={{ color:'red'}}>this is div</div>
```

```css
.foo{
  color: red;
}
```

```jsx
import './index.css'

function App(){
  return (
    <div>
      <span className="foo">this is span</span>
    </div>
  )
}
```





# B站评论案例

类名

























