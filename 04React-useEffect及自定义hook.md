## 七、React副作用管理-useEffect
### 概念理解
useEffect是一个React Hook函数，用于在React组件中**创建不是由事件引起而是由渲染本身引起的操作（副作用）**, 比 如发送AJAX请求，更改DOM等等
![](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709371222725-dddc110d-6cf0-4667-97b2-dba7299422ef.png?x-oss-process=image%2Fresize%2Cw_642%2Climit_0#averageHue=%23fcfcfb&from=url&id=wtI9r&originHeight=442&originWidth=642&originalType=binary&ratio=1.25&rotation=0&showTitle=false&status=done&style=none&title=)
![](assets/10.png#id=TjMrd&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
:::warning
说明：上面的组件中没有发生任何的用户事件，组件渲染完毕之后就需要和服务器要数据，整个过程属于“只由渲染引起的操作”
:::
### 基础使用
> 需求：在组件渲染完毕之后，立刻从服务端获取平道列表数据并显示到页面中

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709371236233-1cad8e5d-b766-4eee-8fbc-12165aabe88c.png#averageHue=%232d3946&clientId=u8b307f73-4637-4&from=paste&height=177&id=uadb35d95&originHeight=288&originWidth=838&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=62961&status=done&style=none&taskId=u16f3b24a-d335-4de4-beaa-c2b375d3dff&title=&width=516)
![](assets/11.png#id=fJ0QY&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

1. 参数1是一个函数，可以把它叫做**副作用函数**，在函数内部可以放置要执行的操作
2. 参数2是一个数组（可选参），**在数组里放置依赖项**，不同依赖项会影响第一个参数函数的执行，当是一个空数组的时候，副作用函数只会在组件渲染完毕之后执行一次 
:::warning
接口地址：[http://geek.itheima.net/v1_0/channels](http://geek.itheima.net/v1_0/channels)
:::

### useEffect依赖说明
useEffect副作用函数的执行时机存在多种情况，根据传入依赖项的不同，会有不同的执行表现

| **依赖项** | **副作用功函数的执行时机** |
| --- | --- |
| 没有依赖项 | 组件初始渲染 + 组件更新时执行 |
| 空数组依赖 | 只在初始渲染时执行一次 |
| 添加特定依赖项 | 组件初始渲染 + 依赖项变化时执行 |

13集再看看三种情况
1和3区别，3执行只看依赖的变化，而1只要有组件更新就会执行
### 清除副作用
> 概念：在useEffect中编写的由渲染本身引起的对接组件外部的操作，社区也经常把它叫做副作用操作，比如在useEffect中开启了一个定时器，我们**想在组件卸载时把这个定时器再清理掉，这个过程就是清理副作用**

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709371264071-a4fd90cc-a0c8-44c1-994c-9fe51a2b22e9.png#averageHue=%23262c36&clientId=u8b307f73-4637-4&from=paste&height=253&id=uafdffd70&originHeight=528&originWidth=644&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=120759&status=done&style=none&taskId=udf6af245-d025-4945-8658-7255eae65cb&title=&width=308.20001220703125)
return 后再写一个函数
:::warning
说明：清除副作用的函数**最常见的**执行时机是在**组件卸载时自动执行**
:::

 需求：在Son组件渲染时开启一个定制器，卸载时清除这个定时器  
```javascript
import { useEffect, useState } from "react"

function Son () {
  // 1. 渲染时开启一个定时器
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('定时器执行中...')
    }, 1000)

    return () => {
      // 清除副作用(组件卸载时)
      clearInterval(timer)
    }
  }, [])
  return <div>this is son</div>
}

function App () {
  // 通过条件渲染模拟组件卸载
  const [show, setShow] = useState(true)
  return (
    <div>
    {show && <Son />}
    <button onClick={() => setShow(false)}>卸载Son组件</button>
    </div>
  )
}

export default App
```

## 八、自定义Hook
### 实现方法
> 概念：自定义Hook是以 `use打头的函数`，通过自定义**Hook函数可以用来**`**实现逻辑的封装和复用**`

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709373646065-7f77d992-da12-4a8c-923c-91f171ddf989.png#averageHue=%23f9f8f3&clientId=u8b307f73-4637-4&from=paste&height=494&id=ue3905537&originHeight=618&originWidth=2006&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=434251&status=done&style=none&taskId=ue9d67dc4-f29a-4877-bdea-d6b03729b1b&title=&width=1604.8)
![](assets/13.png#id=JuEpa&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
 **封装自定义hook通用思路**
1. 声明一个以use打头的函数
2. 在函数体内封装可复用的逻辑（只要是可复用的逻辑）
3. 把组件中用到的状态或者回调return出去（以对象或者数组）
4. 在哪个组件中要用到这个逻辑，就执行这个函数，解构出来状态和回调进行使用

```javascript
// 封装自定义Hook
// 问题: 布尔切换的逻辑 当前组件耦合在一起的 不方便复用
// 解决思路: 自定义hook

import { useState } from "react"

function useToggle () {
  // 可复用的逻辑代码
  const [value, setValue] = useState(true)
  const toggle = () => setValue(!value)

  // 哪些状态和回调函数需要在其他组件中使用 return
  return {
    value,
    toggle
  }
}


function App () {
  const { value, toggle } = useToggle()
  return (
    <div>
    {value && <div>this is div</div>}
    <button onClick={toggle}>toggle</button>
    </div>
  )
}

export default App
```

### React Hooks使用规则

1. 只能在组件中或者其他自定义Hook函数中调用
2. 只能在组件的顶层调用，不能嵌套在if、for、其它的函数中

![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1709373667770-4d3e82e5-fee2-453e-91c9-17cd43616a35.png#averageHue=%23233b20&clientId=u8b307f73-4637-4&from=paste&height=152&id=u1c9657f6&originHeight=458&originWidth=1922&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=219128&status=done&style=none&taskId=uee00a6a7-8e18-42f9-9145-ecf7764beb0&title=&width=638.7999877929688)
![](assets/14.png#id=r0VZj&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

