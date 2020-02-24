# React-grammar
```
import React from 'react';
import ReactDOM from 'react-dom';
```
## JSX 语法
* 实例
```
const element = <h1>hello world</h1>;
ReactDOM.render(element,document.getElementById('root'));
```
* 在 JSX 中使用表达式
```
const num = 100;
const element = <h1>{num}</h1>;
// 元素渲染：首页中添加一个 id="root" 的 <div> ，节点所有的内容都将由 React DOM 来管理
ReactDOM.render(element,document.getElementById('root'));
```
* React 元素 *实际上就是一个普通的对象*
```
// 元素包含：元素类型、元素属性、子节点
let eleobj = {
  type: 'div',
  props: {
    children: ['hello','world'],
    className: 'box',
    id: 'box'
  }
};

// 当遇到 JSX 表达式的时候，通过 React.createElement 方法返回类似上述对象
const element = React.createElement('h1',{className:'box'},'hello',getElement('p',{className:'box'},'world'));
ReactDOM.render(element,document.getElementById('root'));
```
* 更新渲染元素
```
// 通过 setInterval() 方法，每秒钟调用一次 ReactDOM.render()
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>现在是 {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('example')
  );
}
setInterval(tick, 1000);

// 用函数表示计时器例子
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>现在是 {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}
setInterval(tick, 1000);

// 创建一个 React.Component 的 ES6 类，该类封装了要展示的元素，需要注意的是在 render() 方法中，需要使用 this.props 替换 props
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
} 
function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}
setInterval(tick, 1000);
```
## 组件
* 函数定义组件

接收单一的 props 对象，返回一个 React 元素

props 是组件的输入内容，从父组件传递给子组件的数据（属性）
```
function Hello(props){
  return <h1>Hello{props.name}</h1>;
}
ReactDOM.render(<Hello name='React' />,getElementById('root'));
```
* 类定义组件

React 提供了 component 抽象基础类，通常继承 React.Component

至少定义一个 render() 方法
```
class Hello extends React.Component {
  render(){
    return <h1>Hello {this.props.name}</h1>;
  }
}
ReactDOM.render(<Hello name='React' />,getElementById('root'));
```
* state (状态)

私有的、完全受控于当前组件，组件外部是无法修改的

类定义的组件特有的属性

状态声明（构造函数是唯一能够初始化 this.state 的地方）
```
class Hello extends React.Component{
  // ES6 对类的默认方法：将父类中的 this 对象继承给子类
  constructor(){
    super();
    this.state = {
      name:'React'
    }
  }
  render(){
    return <h1>Hello{this.state.name}</h1>;
  }
}
```
* 生命周期函数

初始化*constructor()*

挂载*componentDidMount()*

更新*componentUpdate()*

卸载*componentWillUnmount()*

错误*componentDidCatch()*
```
class Hello extends React.Component {
  constructor(props) {
      super(props);
      this.state = {opacity: 1.0};
  }
  componentDidMount() {
    this.timer = setInterval(function () {
      var opacity = this.state.opacity;
      opacity -= 0.5;
      if (opacity < 0.1) {
        opacity = 1.0;
      }
      this.setState({
        opacity: opacity
      });
    }.bind(this), 100);
  }
  render () {
    return (
      <div style={{opacity: this.state.opacity}}>
        Hello {this.props.name}
      </div>
    );
  }
}
ReactDOM.render(
  <Hello name="world"/>,
  document.body
);
```
## 事件绑定