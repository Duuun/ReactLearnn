### 介绍
> Redux 是React最常用的集中状态管理工具，类似于Vue中的Pinia（Vuex），可以独立于框架运行
作用：通过集中管理的方式管理应用的状态


![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709383336827-2254e523-29ec-4920-b7e9-48f746d3c4cc.png#averageHue=%23ececec&clientId=u8b307f73-4637-4&from=paste&height=273&id=ue32e0b04&originHeight=805&originWidth=1428&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=313596&status=done&style=none&taskId=uc870614a-67d3-4c21-bb74-eca58611e20&title=&width=483.79998779296875)![](assets/1.png#id=X2Rs9&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

1. 独立于组件，无视组件之间的层级关系，简化通信问题
2. 单项数据流清晰，易于定位bug
3. 调试工具配套良好，方便调试
### Redux快速体验
#### 实现计数器
> 需求：不和任何框架绑定，不使用任何构建工具，使用纯Redux实现计数器

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709383451615-22ab862a-09e0-426b-bdf7-4a38e0874842.png#averageHue=%23ededed&clientId=u8b307f73-4637-4&from=paste&height=237&id=u8ccb0954&originHeight=576&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27477&status=done&style=none&taskId=u33c7bb7c-e6d4-4621-ad87-2437e0b053f&title=&width=526.7999877929688)
**使用步骤：**

1. 定义一个 reducer 函数 （根据当前想要做的修改返回一个新的状态），使得根据不同的action 返回不同的state
2. 使用**createStore**方法传入 **reducer函数 **生成一个store实例对象
3. 使用store实例的 **subscribe**方法订阅数据的变化（数据一旦变化，可以得到通知）
4. 使用store实例的 **dispatch**方法提交action对象 触发数据变化（告诉reducer你想怎么改数据）
5. 使用store实例的 **getState**方法 获取最新的状态数据更新到视图中

**代码实现：**
```javascript
<button id="decrement">-</button>
  <span id="count">0</span>
  <button id="increment">+</button>

  <script src="https://unpkg.com/redux@latest/dist/redux.min.js"></script>

  <script>
  // 定义reducer函数 
  // 内部主要的工作是根据不同的action 返回不同的state
  function counterReducer (state = { count: 0 }, action) {
    switch (action.type) {
      case 'INCREMENT':
        return { count: state.count + 1 }
      case 'DECREMENT':
        return { count: state.count - 1 }
      default:
        return state
    }
  }
// 使用reducer函数生成store实例
const store = Redux.createStore(counterReducer)

// 订阅数据变化
store.subscribe(() => {
  console.log(store.getState())
  document.getElementById('count').innerText = store.getState().count

})
// 增
const inBtn = document.getElementById('increment')
inBtn.addEventListener('click', () => {
  store.dispatch({
    type: 'INCREMENT'
  })
})
// 减
const dBtn = document.getElementById('decrement')
dBtn.addEventListener('click', () => {
  store.dispatch({
    type: 'DECREMENT'
  })
})
  </script>
```

#### 2. Redux数据流架构
> Redux的难点是理解它对于数据修改的规则, 下图动态展示了在整个数据的修改中，数据的流向

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709383462588-6b9e13cc-f5ac-4683-840b-0737a4d226cf.png#averageHue=%23f9f8f7&clientId=u8b307f73-4637-4&from=paste&height=279&id=u5a8acaf0&originHeight=892&originWidth=1594&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=803768&status=done&style=none&taskId=u4741a252-e2c9-4728-b00a-d061c6f39b4&title=&width=498.79998779296875)
为了职责清晰，Redux代码被分为三个核心的概念，我们学redux，其实就是学这三个核心概念之间的配合，三个概念分别是:

1. state:  一个对象 存放着我们管理的数据
2. action:  一个对象 用来描述你想怎么改数据
3. reducer:  一个函数 根据action的描述更新state
### Redux与React - 环境准备
> Redux虽然是一个框架无关可以独立运行的插件，但是社区通常还是把它与React绑定在一起使用，以一个计数器案例体验一下Redux + React 的基础使用


#### 配套工具
> 在React中使用redux，官方要求安装俩个其他插件 - **Redux Toolkit **和 **react-redux**


1.  Redux Toolkit（RTK）- 官方推荐编写Redux逻辑的方式，是一套工具的集合集，简化书写方式 

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709433325585-648242d7-19bd-41a5-a984-bc4626f82325.png#averageHue=%23f3f5eb&clientId=u13c9601a-abc7-4&from=paste&height=64&id=i9zvp&originHeight=80&originWidth=731&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13689&status=done&style=none&taskId=uf9503245-a3c4-40ad-a86b-3e94d552e8e&title=&width=584.8)

2.  react-redux - 用来 链接 Redux 和 React组件 的中间件 

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709433314188-5878deb8-1b51-4e26-a413-330f59469ddf.png#averageHue=%23f9faf7&clientId=u13c9601a-abc7-4&from=paste&height=267&id=u7b26e13e&originHeight=334&originWidth=1270&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26565&status=done&style=none&taskId=u62e43345-5f5c-49f0-a00a-3253a1693f0&title=&width=1016)
#### 配置基础环境

1. 使用 CRA 快速创建 React 项目
```javascript
npx create-react-app react-redux
```

2. 安装配套工具
```javascript
npm i @reduxjs/toolkit  react-redux
```

3. 启动项目
```javascript
npm run start
```
#### store目录结构设计
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709433368778-fc7596ab-19b3-4fa4-be1f-59920e4959bf.png#averageHue=%23262930&clientId=u13c9601a-abc7-4&from=paste&height=215&id=u56cbb07d&originHeight=521&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=76369&status=done&style=none&taskId=ucfbb1950-cd40-4702-b254-963fcca4203&title=&width=527.2000122070312)

1.  通常集中状态管理的部分都会单独创建一个单独的 `store` 目录 
2.  应用通常会有很多个子store模块，所以创建一个 `modules` 目录，在内部编写业务分类的子store 
> 编写同步异步函数

3.  store中的入口文件 index.js 的作用是组合modules中所有的子模块，并导出store 
> 通过configureStore({ })来组合并绑定


### Redux与React - 实现counter
#### 整体路径熟悉
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709433895887-a9dceb7d-724b-43d5-bb3b-497be58bb036.png#averageHue=%23eaf1df&clientId=u13c9601a-abc7-4&from=paste&height=260&id=uad3175ad&originHeight=820&originWidth=1904&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=203124&status=done&style=none&taskId=u2bd0c44e-60eb-4829-843b-28abe81bcfe&title=&width=603)

#### 使用React Toolkit 创建 counterStore
> 使用传统的 Redux，创建一个 reducer 需要定义 action 类型(action types)，编写 action 创建函数(action creators)，并手动处理每个 action 类型对应的状态更新逻辑。这导致了大量的样板代码，并且容易出现错误。
> `createSlice` 函数的目的就是简化这个过程，它可以根据提供的初始状态和一组 reducer 函数自动生成相应的 action 类型、action 创建函数和状态更新逻辑。
> Slice 是一个**用来定义 reducer 函数、action creators 和 action types 的工具**。当你创建一个 Slice 时，**它会生成一个包含 reducer 函数和相关 action creators 的对象。**

```javascript
import { createSlice } from '@reduxjs/toolkit'

const counterStore = createSlice({
  // 模块名称独一无二
  name: 'counter',
  // 初始数据
  initialState: {
    count: 0
  },
  // 修改数据的同步方法，支持直接修改
  reducers: {
    increment (state) {
      state.count++
    },
    decrement(state){
      state.count--
    }
  }
})

// 解构出actionCreater
const { increment,decrement } = counter.actions

// 获取reducer函数
const counterReducer = counterStore.reducer

// 以按需导出的方式导出actioCreater
export { increment, decrement }
// 以默认导出的方式导出reducer
export default counterReducer
```
**补充：**

1. 同步更新action

**reducers** 是 Redux 中的一个概念，用于指定如何更新 Redux store 中的状态。**reducers** 是纯函数，接收先前的状态和一个动作 (action)，然后返回一个新的状态。在 Redux 中，通常通过 Redux store 的 dispatch 方法来分发动作。
**setChannelList** 是一个 reducer 函数，它接收两个参数：先前的状态 **state** 和一个包含 **payload** 属性的动作对象 **action**。根据 Redux 的惯例，**payload** 通常是一个包含需要更新的数据的对象。
在这个特定的 **setChannelList** reducer 中，它的作用是将传递给它的 **action.payload** 中的数据赋值给 Redux store 中的 **channelList** 属性。这样做的效果是更新了 Redux store 中的 **channelList** 数据。

2. 解构setChannelList.actions

**setChannelList** 是一个 action creator，它是一个函数，用于创建一个 action 对象，将 channel 列表设置到 Redux store 中。这个 action 对象包含一个 **type** 属性，用于指定要执行的操作类型，以及一个 **payload** 属性，用于携带需要更新到 store 中的数据。
即解构出Slice 中定义的所有 action creators
#### 合并Store
```javascript
import { configureStore } from '@reduxjs/toolkit'

import counterReducer from './modules/counterStore'

export default configureStore({
  reducer: {
    // 注册子模块
    counter: counterReducer
  }
})
```
#### 为React注入store
> react-redux负责把Redux和React 链接 起来，内置 **Provider 组件**通过 store 参数把创建好的store实例注入到应用中，链接正式建立

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'

// 导入store
import store from './store'
// 导入store提供组件Provider
import { Provider } from 'react-redux'

ReactDOM.createRoot(document.getElementById('root')).render(
  // 提供store数据
  <Provider store={store}>
  <App />
  </Provider>
)
```
#### React组件使用store中的数据
> 在React组件中使用store中的数据，需要用到一个钩子函数 - **useSelector**，它的作用是把store中的数据映射到组件中，使用样例如下：

#### React组件修改store中的数据
> React组件中修改store中的数据需要借助另外一个hook函数 - useDispatch，它的作用是生成提交action对象的dispatch函数，使用样例如下：

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709433944781-22612689-74a4-449b-9460-68043df1b9e0.png#averageHue=%23292e36&clientId=u13c9601a-abc7-4&from=paste&height=322&id=u16185002&originHeight=720&originWidth=1151&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=191058&status=done&style=none&taskId=ufa8abbe6-5ee9-43d6-a30b-75addd4a160&title=&width=514.2000122070312)
![](assets/8.png#id=pgqJF&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
### Redux与React - 提交action传参
> 需求：组件中有俩个按钮 `add to 10` 和 `add to 20` 可以直接把count值修改到对应的数字，目标count值是在组件中传递过去的，需要在提交action的时候传递参数

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709434957067-41d5a5fe-0bad-4670-8427-278c375c9e29.png#averageHue=%23e7e8e7&clientId=u13c9601a-abc7-4&from=paste&height=262&id=ub4a2541a&originHeight=328&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=127071&status=done&style=none&taskId=u569ac752-aed6-4ff3-bfbb-8113ef41b16&title=&width=1024)
实现方式：在reducers的同步修改方法中添加action对象参数，在调用actionCreater的时候传递参数，参数会被传递到action对象![](assets/9.png#id=fDaqS&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
### Redux与React - 异步action处理
**需求理解**
![](assets/11.png#id=pgtfR&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)![](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709434976556-81c9cc52-d580-4ae1-8f62-c3fa7e11eaed.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_937%2Climit_0#averageHue=%23edf2e4&from=url&id=pHET4&originHeight=379&originWidth=937&originalType=binary&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&title=)
**实现步骤**

1. 创建store的写法保持不变，配置好同步修改状态的方法
2. 单独封装一个函数，在函数内部return一个新函数，在新函数中
2.1 封装异步请求获取数据，导入axios插件
2.2 调用同步actionCreater传入异步数据生成一个action对象，并使用dispatch提交
3. 组件中dispatch的写法保持不变
> dispatch 用来派发 action 的事件描述 ,dispatch 将action的事件描述传递给 Reducers
> dispatch(【解构出来的】(res.data.data.channels))


**样板代码**
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709435482270-0f166c16-6d0c-45de-8d34-82a3cb55136e.png#averageHue=%2332503c&clientId=u13c9601a-abc7-4&from=paste&height=211&id=u378c263c&originHeight=160&originWidth=787&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70162&status=done&style=none&taskId=ubb11b3e3-b7e2-49b5-b0ac-1b553a96ff1&title=&width=1037)
异步代码
```javascript
// 创建异步请求
 //解构channelStore得到actionCreater即setChannelList
const { setChannelList } = channelStore.actions 
const url = 'http://geek.itheima.net/v1_0/channels'
// 封装一个函数 在函数中return一个新函数 在新函数中封装异步
// 得到数据之后通过dispatch函数 触发修改
const fetchChannelList = () => {
  return async (dispatch) => {
    const res = await axios.get(url)
    dispatch(setChannelList(res.data.data.channels))
  }
}
```

**完整代码实现**
```javascript
import { createSlice } from '@reduxjs/toolkit'
import axios from 'axios'

const channelStore = createSlice({
  name: 'channel',
  initialState: {
    channelList: []
  },
  reducers: {
    setChannelList (state, action) {
      state.channelList = action.payload   //固定属性 payload
    }
  }
})


// 创建异步请求
 //解构channelStore得到actionCreater即setChannelList
const { setChannelList } = channelStore.actions 
const url = 'http://geek.itheima.net/v1_0/channels'
// 封装一个函数 在函数中return一个新函数 在新函数中封装异步
// 得到数据之后通过dispatch函数 触发修改

const fetchChannelList = () => {
  return async (dispatch) => {
    const res = await axios.get(url)
    dispatch(setChannelList(res.data.data.channels))
  }
}

// 按需导出
export { fetchChannelList }

const channelReducer = channelStore.reducer
export default channelReducer
```
```javascript
import { useEffect } from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { fetchChannelList } from './store/channelStore'

function App () {
  // 使用数据
  const { channelList } = useSelector(state => state.channel)

  // 用useEffect触发异步请求执行
  useEffect(() => {
    dispatch(fetchChannelList())
  }, [dispatch])

  return (
    <div className="App">
    <ul>
    {channelList.map(task => <li key={task.id}>{task.name}</li>)}
                     </ul>
                     </div>
                    )
}

export default App
```
### Redux调试 - devtools
> Redux官方提供了针对于Redux的调试工具，支持实时state信息展示，action提交信息查看等

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709435000293-ab7c3156-1243-404d-bae5-1d47dd22f634.png#averageHue=%23383e49&clientId=u13c9601a-abc7-4&from=paste&height=326&id=u4e3a5dc7&originHeight=408&originWidth=1280&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=62178&status=done&style=none&taskId=ud93c9e22-feff-482b-8e36-5438f831794&title=&width=1024)
![](assets/12.png#id=EX7H5&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
## 

