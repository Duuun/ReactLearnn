## 五、React表单控制
### 受控绑定
概念：使用React组件的状态（useState）控制表单的状态
```
function App(){
  const [value, setValue] = useState('')
  return (
    <input 
      type="text" 
      value={value} 
      onChange={e => setValue(e.target.value)}
    />
  )
}
```
Tips：
input绑定value后，无法键入：
### 非受控绑定/获取Dom

1. 获取Dom

useRef生成ref对象 绑定到dom元素身上
dom可用时，ref。current获取dom  //渲染完毕后dom才可用

2. 通过获取DOM的方式获取表单的输入数据
```
function App() {
  const inputRef = useRef(null)
  const showDom = () => {
    console.dir(inputRef.current)
  }

  return (
    <div className="App">
      <input
        type='text'
        ref={inputRef}>
      </input>
      <button onClick={showDom}></button>

    </div>
  );
}
```

## 六、React组件通信

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367114591-7dc98e4d-1535-40ba-bbe8-60612dda69b1.png#averageHue=%23eeeeee&clientId=u8b307f73-4637-4&from=paste&height=376&id=u71088986&originHeight=470&originWidth=1304&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=67513&status=done&style=none&taskId=u1f5ddd53-5eac-4d7d-8ee0-1251fb58e91&title=&width=1043.2)

1. A-B 父子通信
2. B-C 兄弟通信
3. A-E 跨层通信

### 父子通信-父传子
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367157374-f01596fd-bc43-4e2c-b462-309ab6f6d82f.png#averageHue=%23f5f3ea&clientId=u8b307f73-4637-4&from=paste&height=421&id=u6cbb3478&originHeight=526&originWidth=1134&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=669476&status=done&style=none&taskId=u306df2ff-255d-47cb-a44d-e67704b8801&title=&width=907.2)
#### 基础实现
嵌套关系实现：

**实现步骤 **

1. 父组件传递数据 - 在子组件标签上绑定属性 
2. 子组件接收数据 - 子组件通过props参数接收数据   

// props传过来的是一个对象，props.xx属性就可以用
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367742838-5073702e-11a4-44b7-9e45-eb0a1ee57a29.png#averageHue=%23fefaf7&clientId=u8b307f73-4637-4&from=paste&height=62&id=ue419225a&originHeight=77&originWidth=282&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=4650&status=done&style=none&taskId=u17748025-a6a9-4175-92bd-d5235323b6b&title=&width=225.6)

```javascript
function Son(props){
  return <div>{ props.name }</div>
}


function App(){
  const name = 'this is app name'
  return (
    <div>
    <Son name={name}/>
    </div>
  )
}
```
#### props说明

1. **props可以传递任意的合法数据（都可以父传子）**，比如数字、字符串、布尔值、数组、对象、函数、**JSX**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367175220-6ebf79f3-7300-4ad0-bff3-b682faf432a4.png#averageHue=%23c8d1c3&clientId=u8b307f73-4637-4&from=paste&height=357&id=ud2a65c6f&originHeight=446&originWidth=1452&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=265572&status=done&style=none&taskId=u94c3b352-b300-44de-a3e6-0c3c73b89e4&title=&width=1161.6)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709368045189-8e085348-851f-478f-a3eb-9d4884aca45b.png#averageHue=%2323282f&clientId=u8b307f73-4637-4&from=paste&height=244&id=u7b4eff4c&originHeight=507&originWidth=667&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27849&status=done&style=none&taskId=u45cceba9-4bcd-4694-b7f3-f0e37dd9205&title=&width=321.60003662109375)

2. **props是只读对象，**子组件只能读取props中的数据，不能直接进行修改, 父组件的数据只能由父组件修改 

在子组件中修改会报错
谁的数据谁才能修改
#### 特殊的prop-chilren
场景：当我们把内容嵌套在子组件的标签内部时，在props对象中会自动创建一个名为children的属性，并接收该内容
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367185693-1480ad82-08df-49c2-8bd4-3eebfcd0db8c.png#averageHue=%23c3ca9e&clientId=u8b307f73-4637-4&from=paste&height=326&id=u174cc60f&originHeight=674&originWidth=938&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117525&status=done&style=none&taskId=uc0e6bf72-0cbf-4507-a1d2-2ffb7bb0b3c&title=&width=453.8000183105469)
### 父子通信-子传父
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367197544-d90155f7-51a7-44a6-b3ba-a53165667528.png#averageHue=%23f6f4ee&clientId=u8b307f73-4637-4&from=paste&height=346&id=u7d790f25&originHeight=432&originWidth=1062&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=474122&status=done&style=none&taskId=ue6c3ae8f-b902-4d25-adcf-0720d7642c0&title=&width=849.6)

核心思路：

1. 通过父传子将父组件中的函数传给子组件
2. 在子组件中调用父组件中的函数并传递参数

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709368530061-f6971e0e-934d-4019-8584-4f5d75413284.png#averageHue=%23243734&clientId=u8b307f73-4637-4&from=paste&height=114&id=u04d7ffa0&originHeight=143&originWidth=651&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38346&status=done&style=none&taskId=u9f9385d4-d10b-4e10-a59a-cf0b9db92cb&title=&width=520.8)

```javascript
function Son({ onGetMsg }){
  const sonMsg = 'this is son msg'
  return (
    <div>
    {/* 在子组件中执行父组件传递过来的函数 */}
    <button onClick={()=>onGetMsg(sonMsg)}>send</button>
    </div>
  )
}


function App(){
  const getMsg = (msg)=>console.log(msg)

  return (
    <div>
    {/* 传递父组件中的函数到子组件 */}
    <Son onGetMsg={ getMsg }/>
    </div>
  )
}
```

### 兄弟组件通信
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709367204762-713264f7-2dff-4283-8b8e-8deba9ce035b.png#averageHue=%23f4f4f2&clientId=u8b307f73-4637-4&from=paste&height=284&id=u2927e70d&originHeight=564&originWidth=990&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=569366&status=done&style=none&taskId=u133a66bf-424f-490e-8160-c92308ee3d3&title=&width=497.79998779296875)

实现思路: 借助 **状态提升** 机制，通过共同的父组件进行兄弟之间的数据传递

1. A组件先通过**子传父**的方式把数据传递给父组件App
2. App拿到数据之后通过**父传子**的方式再传递给B组件

```javascript
// 1. 通过子传父 A -> App
// 2. 通过父传子 App -> B

import { useState } from "react"

function A ({ onGetAName }) {
  // Son组件中的数据
  const name = 'this is A name'
  return (
    <div>
    this is A compnent,
    <button onClick={() => onGetAName(name)}>send</button>
  </div>
)
}

function B ({ name }) {
  return (
    <div>
    this is B compnent,
    {name}
    </div>
  )
}

function App () {
  const [name, setName] = useState('')
  const getAName = (name) => {
    setName(name)
  }
  return (
    <div>
    this is App
    <A onGetAName={getAName} />
    <B name={name} />
    </div>
  )
}

export default App
```
### 跨层组件通信
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709369944662-e35fc726-c3f1-4367-8548-e92ab70e7d8a.png#averageHue=%23fbf9f3&clientId=u8b307f73-4637-4&from=paste&height=445&id=u05a39caa&originHeight=556&originWidth=2062&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=671363&status=done&style=none&taskId=u4ea3700f-8a52-4fe9-932f-6a4fe9c739c&title=&width=1649.6)
**实现步骤：**

1. 使用 createContext方法创建一个上下文对象Ctx （一般大写开头）
2. 在顶层组件（App）中通过 Ctx.Provider 组件提供数据，并设置value属性（要传递内容）
```javascript
      <MsgContent.Provider value={msg}>
        this is APP
        <A />
      </MsgContent.Provider>
```

3. 在底层组件（B）中通过 useContext 钩子函数获取消费数据

用一个变量来接收
```javascript
const msgGet = useContext(MsgContent)  
```

完整代码
```javascript
// App -> A -> B

import { createContext, useContext } from "react"

// 1. createContext方法创建一个上下文对象
// 一般首字母发泄
const MsgContext = createContext()

function A () {
  return (
    <div>
    this is A component
    <B />
    </div>
  )
}

function B () {
  // 3. 在底层组件 通过useContext钩子函数使用数据
  const msg = useContext(MsgContext)
  return (
    <div>
    this is B compnent,{msg}
    </div>
  )
}

function App () {
  const msg = 'this is app msg'
  return (
    <div>
    {/* 2. 在顶层组件 通过Provider组件提供数据 */}
    <MsgContext.Provider value={msg}>
    this is App
    <A />
    </MsgContext.Provider>
    </div>
  )
}

export default App
```

###  使用Context机制跨层级组件通信
只要有层级就可以用这种方法
  ![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709371103598-6af0aca3-0b0a-4d64-b4d9-fa447c94a4dc.png#averageHue=%23fbf9f6&clientId=u8b307f73-4637-4&from=paste&height=231&id=ua888acce&originHeight=369&originWidth=812&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=167229&status=done&style=none&taskId=u09d325e9-a68f-4d88-83ab-17b6c79b347&title=&width=507.79998779296875)

