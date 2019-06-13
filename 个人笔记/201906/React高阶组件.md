## 1. 高阶组件是什么？

高阶组件（ higher-order component ，HOC ）是 React 中复用组件逻辑的一种进阶技巧。它本身并不是 React 的 API，而是一种 React 组件的设计理念，

可以类比高阶函数，高阶函数是把函数作为参数传入到函数中并返回一个新的函数。这里我们把函数替换为组件，就是高阶组件了

`const EnhancedComponent = higherOrderComponent(WrappedComponent);`

## 2. 高阶组件常见的实现方式

### 2.1 Props Proxy 属性代理

#### 先了解下代理模式

> Proxy（代理模式）：为其他对象（消费者）提供一个代理（代理商）以控制对这个对象（生成商）的访问。

![代理](https://raw.githubusercontent.com/winfredwyw/notes/master/assets/201902/%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F.jpg)

> 代理模式优势: 重用业务逻辑

#### Props Proxy实现

> 请看具体代码

#### 注意点

Props Proxy 作为一层代理，具有隔离的作用，因此传入 WrappedComponent 的 ref 将无法访问到其本身，需要在 Props Proxy 内完成中转，具体可参考以下代码，react-redux 也是这样实现的。

```
function ppHOC(WrappedComponent) {
  return class PP extends React.Component {
    // 实现 HOC 不同的命名
    static displayName = `HOC(${WrappedComponent.displayName})`;

    getWrappedInstance() {
      return this.wrappedInstance;
    }

    // 实现 ref 的访问
    setWrappedInstance(ref) {
      this.wrappedInstance = ref;
    }

    render() {
      return <WrappedComponent {
        ...this.props,
        ref: this.setWrappedInstance.bind(this),
      } />
    }
  }
}

@ppHOC
class Example extends React.Component {
  static displayName = 'Example';
  handleClick() { ... }
  ...
}

class App extends React.Component {
  handleClick() {
    this.refs.example.getWrappedInstance().handleClick();
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick.bind(this)}>按钮</button>
        <Example ref="example" />
      </div>  
    );
  }
}
```

### 2.2 提取state

请看代码

### 2.3 反向继承

HOC 类继承了 WrappedComponent，意味着可以访问到 WrappedComponent 的 state、props、生命周期和 render 等方法。这种方案依然是继承的思想，对于 WrappedComponent 也有较强的侵入性，因此并不常见。

```
function ppHOC(WrappedComponent) {
  return class ExampleEnhance extends WrappedComponent {
    ...
    componentDidMount() {
      super.componentDidMount();
    }
    componentWillUnmount() {
      super.componentWillUnmount();
    }
    render() {
      ...
      return super.render();
    }
  }
}
```


## 3. HOC 的适用范围

- 与 DOM 相关，建议使用父组件，类似于原生 HTML 编写
- 与 DOM 不相关，如校验、权限、请求发送、数据转换这类，通过数据变化间接控制 DOM，可以使用 HOC 抽象
- 交叉的部分，DOM 相关，但可以做到完全内聚，即这些 DOM 不会和外部有关联，均可


## 4. 参考地址

- [精读 React 高阶组件](https://github.com/dt-fe/weekly/blob/v2/012.%E7%B2%BE%E8%AF%BB%20React%20%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6.md)
- [讨论区](https://github.com/dt-fe/weekly/issues/18)
- [设计模式](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F.md)