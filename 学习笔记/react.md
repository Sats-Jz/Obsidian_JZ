UI = f(state), 通过状态驱动视图更新

# JSX
```jsx
createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766936927988-af7572dc-4efd-46e5-883c-896a2bc944e7.png)

jxdev函数（开发） | jsx函数（线上）

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766937057664-dac8cded-a781-4cec-9a2a-33d77e23726c.png)

+ 函数式组件： 更简洁
+ 对象式组件

函数式组件+hooks 优雅的开发

# 组件化开发
组件的输入： props， 也叫做属性比如id

内部状态： state

响应交互

## props
### renderprops
```jsx
// import { useState } from 'react'
import './App.css'
import { HelloWorld } from './components/helloworld'

function App() {
  return <HelloWorld title="Hello World" render={() => <p style={{ color: 'blue' }} >This is a custom render function.</p>}  ></HelloWorld>
}

export default App
```

```jsx
interface HelloWorldProps {
    title: string;
    render?: () => React.ReactNode;
}

export const HelloWorld = (props: HelloWorldProps) => {
    const {title, render} = props;
    return (
        <div>
        <h1>Hello World {title} {render?.()}</h1>
        </div>
    );
};
```

render内部的状态也可以影响到外部，比如render传参



<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766939449303-68e4afe0-ba51-44e1-932f-c12eb6288ff2.png)

最好： 就近取值

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766939493472-07a9aa01-0306-4a5e-8b8b-71bbb47dea0f.png)



+ List渲染
+ <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766939609406-d313c0b1-ef4f-4ba9-9f1b-8cfab76b9d87.png)
+ <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766939737992-c66791c5-66a5-4afb-9545-d56022c44d30.png)
+ 条件渲染
+ <!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766939805991-9a13bd7e-6299-48ee-9a43-46c00beda548.png)

### 事件处理+合成事件系统
+ 驼峰民命 

```jsx
interface HelloWorldProps {
    title: string;
    // count: number;
    render?: (count: number) => React.ReactNode;
    onChange?: (count: number) => void;
}
```

### useState Hook
## 副作用
通过Effect

状态变化后的额外作用

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766940050356-bcd540a8-5895-4f57-a282-d881501c5f64.png)



三种形式：

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766940148865-dc66bb30-9c3c-4893-b997-99d1040e6469.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766940321655-af992ba3-2111-4d23-8ae4-108a0d9b0f0e.png)

## Ref
+ 存储值，不会引起状态变更
+ 获取dom元素

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2025/png/51149297/1766940623081-4d7dc267-fd02-4578-8bee-a0e04db53e6f.png)

```jsx
cosnt kRef = useRed()
```

