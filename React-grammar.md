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

注意：事件名称是驼峰形式
* 事件处理函数绑定 this--箭头函数
```
class App extends React.Compontrnt{
  contructor(){
    super();
    this.state={
      content:'hello'
    }
  }
  handleClick=()=>{
    this.setState({
      content:'world'
    });
  }
  render(){
    return(
      <div>
        <p>{this.state.content}</p>
        <button onClick={this.handleClick}>click</button>
      </div>
    )
  }
}
ReactDOM.render(<App />, document.getElementById('root'));
```
* 事件处理函数绑定 this--通过 bind 绑定 this
```
class App extends React.Compontrnt{
  contructor(){
    super();
    this.state={
      content:'hello'
    }
    this.handleClick=this.handleClick.bind(this);
  }
  handleClick(){
    this.setState({
      content:'world'
    });
  }
  render(){
    return(
      <div>
        <p>{this.state.content}</p>
        <button onClick={this.handleClick.bind(this)}>click</button>
      </div>
    )
  }
}
ReactDOM.render(<App />, document.getElementById('root'));
```
## 事件处理函数传参
* 函数声明时事件对象作为最后一个参数传入
```
deleteRow = ( id, e ) => { }
```
* 箭头函数的事件对象显示传入
```
// 箭头函数没有this指向，默认是继承外部上下的this，所以箭头函数中的this指向的就是组件
<button onClick={(e) => this.deleteRow(id, e)}>
  Delete Row
</button>
```
* bind 会隐式传入
```
// bind() 方法可以把组件的this代替函数的this
<button onClick={this.deleteRow.bind(this, id)}>
  Delete Row
</button>
```
## PropTypes
* PropTypes 类型检查
```
import PropTypes from 'prop-types';
……
class Hello extends React.Component { 
  render() { 
    return <h1>Hello, {this.props.name}</h1>; 
  } 
}
Hello.propTypes = {
  name : PropTypes.string
}
```
* defaultProps 设置默认值
```
import PropTypes from 'prop-types';
class Hello extends React.Component { 
  render() { 
    return <h1>Hello, {this.props.name}</h1>; 
  } 
}
Hello.defaultProps = {
  name : 'Tom'
}
```
* 受控组件

输入的值由 React 控制的表单元素称为“受控组件”*

在 HTML 当中，像 <input>,<textarea>,<select> 这类表单元素会维持自身状态，并根据用户输入进行更新
  
在 React 中，可变的状态通常保存在组件的状态属性中，并且只能用 setState() 方法进行更新
```
import React,{Component} from 'react';
import ReactDOM from 'react-dom';
class TodoList extends Component{
    constructor(){
        super();
        this.state = {
            inputValue:'hello'
        }
    }
    // 不设置onChange事件，input无法修改里面的内容，重新设置state，render被调用，重新渲染
    handleChange = (e)=>{
        this.setState({
            inputValue:e.target.value
        })
    }
    render(){
        return (
            <div>
                <input type="text" onChange={this.handleChange} value={this.state.inputValue}/>
                <button>提交</button>
            </div>
        )
    }
}
```
* 非受控组件

非受控组件将真实数据保存在 DOM 中

容易同时集成 React 和非 React 代码，减少代码量

使用 defaultValue 为表单元素指定初始值

使用 refs 获取表单的值
* refs

用于访问在 render 方法中创建的 DOM 节点或 React 元素
```
//创建 refs
import React,{Component} from 'react';
import ReactDOM from 'react-dom';
class TodoList extends Component{
    constructor(){
        super();
        this.state = {
            inputValue:'hello'
        }
    }
    handleClick = ()=>{
        console.log(this.input.value)
    }
    componentDidMount(){
        this.input.focus();
    }
    render(){
        return (
            <div>
                <input type="text" defaultValue='hello' ref={input=>this.input = input}/>
                <button onClick={this.handleClick}>提交</button>
            </div>
        )
    }
}
```
* TodoList

[示例代码](https://wangmeng456.github.io/React/task-one/index.html)
## DOM Elements
* className

使用 classname 属性指定一个 css 类
* onChange

实时处理用户输入
* htmlFor

因为 for 是在 JavaScript 中的一个保留字， React 元素使用 htmlFor 代替
* dangerouslySetInnerHTML

React 提供的替换浏览器 DOM 中的 innerHTML 接口的一个函数
```
let str = '<h1>hello</h1>'
let ele = <div dangerouslySetInnerHTML={{__html:str}}></div>
ReactDOM.render(ele,document.getElementById('root'));
```
* style

style 属性接受一个JavaScript 对象，其属性用小驼峰命名法命名，而不是接受 CSS 字符串
```
<div style={{width:100,height:'100px',background:'red'}}>
  Hello World!
</div>
```
