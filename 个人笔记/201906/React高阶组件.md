## 1. 高阶组件是什么？

高阶组件（ higher-order component ，HOC ）是 React 中复用组件逻辑的一种进阶技巧。它本身并不是 React 的 API，而是一种 React 组件的设计理念，

可以类比高阶函数，高阶函数是把函数作为参数传入到函数中并返回一个新的函数。这里我们把函数替换为组件，就是高阶组件了

`const EnhancedComponent = higherOrderComponent(WrappedComponent);`

## 2. 高阶组件常见的实现方式

### 2.1 Props Proxy 属性代理

> Proxy（代理模式）：为其他对象（消费者）提供一个代理（代理商）以控制对这个对象（生成商）的访问。