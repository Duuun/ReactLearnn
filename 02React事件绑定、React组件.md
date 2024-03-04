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
## 四、React组件
概念：一个组件就是一个用户界面的一部分，它可以有自己的逻辑和外观，组件之间可以互相嵌套，也可以服用多次
可以把一个庞大的应用分成一个个小部分
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384297622-7824697f-d83f-4a7a-8d85-1f40ecb675c1.png#averageHue=%23f1f1f1&clientId=u8b307f73-4637-4&from=paste&height=370&id=c04yb&originHeight=462&originWidth=1376&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=40248&status=done&style=none&taskId=u858f75e9-b345-4251-967a-fb5d829e115&title=&width=1100.8)
### 1. 组件基础使用
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

### 2.组件状态管理-useState
#### 1. 基础使用
> useState 是一个 React Hook（函数），它允许我们向组件添加一个`状态变量`, 从而控制影响组件的渲染结果
和普通JS变量不同的是，**状态变量一旦发生变化组件的视图UI也会跟着变化（数据驱动视图）**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384329287-459aa957-3327-456d-a2ee-fe9cbd8828d9.png#averageHue=%23f3f2ea&clientId=u8b307f73-4637-4&from=paste&height=270&id=u65d408ef&originHeight=338&originWidth=804&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14675&status=done&style=none&taskId=ud60b865f-d527-495a-b590-8a521e5944a&title=&width=643.2)

```javascript
// 导包
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

**简写**
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

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384338088-34a68174-4c74-4878-9238-1045e9f38a89.png#averageHue=%231c4746&clientId=u8b307f73-4637-4&from=paste&height=378&id=u6511c722&originHeight=472&originWidth=1658&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=254862&status=done&style=none&taskId=u21f11031-82f7-48c1-b6ab-caf54a22355&title=&width=1326.4)
#### 3. 修改对象状态
> 对于对象类型的状态变量，应该始终给set方法一个`全新的对象` 来进行修改
> 修改set方法就相当于是替换状态变量

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709384349066-a228a114-79f0-45f2-8c61-da861f2c23de.png#averageHue=%234c7b4d&clientId=u8b307f73-4637-4&from=paste&height=506&id=u965d0dd6&originHeight=632&originWidth=1612&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=223050&status=done&style=none&taskId=u6f05f07e-0af4-4c24-a6c7-fdb42e62773&title=&width=1289.6)
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

### 3.组件的基础样式处理
> React组件基础的样式控制有俩种方式，**行内样式和class类名控制**

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



