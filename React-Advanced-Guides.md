# Context
* Context

Context 通过组件树提供了一个传递数据的方法，从而避免了在每一个层级手动的传递 props 属性
* API

*React.createContext*

[举例](https://segmentfault.com/a/1190000019911590)

*Provider*

*Consumer*

```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
const {Provider, Consumer} = React.createContext('green');
class Header extends Component {
    render() {
        return (
            <div>
                <Toolbar />
            </div>
        )
    }
}
class Toolbar extends Component {
    render() {
        return (
            <Button />
        )
    }
}
class Button extends Component {
    render() {
        return (
            <Consumer>
                {(value)=><button style={{color:value}}>按钮</button>}
            </Consumer>
        )
    }
}
ReactDOM.render(
    <Provider value='red'>
        <Header />
    </Provider>,
    document.getElementById('root')
)
```
# HOC(高阶组件)

* 是 React 中的高级技术，用来重用组件逻辑
* 高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件
* 注意事项

不要修改原始组价，使用组合方式

传递不相关 props 属性给被包裹的组件

包装显示名字以便于调试
```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
function withFetch(WrappedComponent,url){   
    class WithFetch extends Component{
        constructor(){
            super();
            this.state = {
                data:''
            }
        }
        componentDidMount = () => {
            fetch(url, {
                method: 'get'
            }).then(res=>res.json()).then((res)=>{
                console.log(res)
                this.setState({
                    data:res
                },()=>{console.log(this.state.data)})
            }).catch(error => console.log('error is', error));
        }
        render(){
            return <WrappedComponent data={this.state.data}/>
        }
        
    }
    WithFetch.displayName = `WithFetch(${getDisplayName(WrappedComponent)})`;
    return WithFetch;
}
function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
class SearchPoetry extends Component{
    render(){
        return (
            this.props.data&&[<h3>{this.props.data.result[0].title}</h3>,
            <p>{this.props.data.result[0].authors}</p>,
            <p>{this.props.data.result[0].content}</p>
            ]
        );
    }
}
const SearchPoetryWithFetch = withFetch(SearchPoetry,'https://api.apiopen.top/searchPoetry?name=静夜思')
ReactDOM.render(<SearchPoetryWithFetch />,document.getElementById('root'));
```
# Portals

Portals 提供了一种将子节点渲染到父组件以外的 DOM 节点的方式
```
ReactDOM.createPortal(child, container)
  ……
  render() 
    return ReactDOM.createPortal( 
    this.props.children, 
    domNode, 
  ); 
}
```
# fetch
```
import React,{Component} from 'react';
import ReactDOM from 'react-dom';
class App extends Component{
    componentDidMount() {
        // mdn：fetch讲解地址：
        // https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

        // fetch 默认是get请求，返回的数据需要通过json()函数转成json数据
        // fetch('/api/courses').then(res=>res.json()).then((res)=>{
        //     console.log(res);
        // })

        // fetch发送post请求
        // fetch('/api', {
        //     method: 'post',
        //     headers: {
        //       'Content-Type': 'application/json'
        //     },
        //     body: JSON.stringify({
        //         "username":"tom",
        //         "passwd":"123456"
        //     })
        // }).then(res=>res.json()).then((res)=>{
        //     console.log(res);
        // }).catch(error => console.log('error is', error));

        // fetch配置第二个参数
        // fetch('/api/courses', {
        //     method: 'get'
        // }).then(res=>res.json()).then((res)=>{
        //     console.log(res);
        // }).catch(error => console.log('error is', error));
    }
    render(){
        return (
            <div>fetch 请求</div>
        )
    }
}
ReactDOM.render(
    <App/>,
    document.getElementById('root')
);
```
