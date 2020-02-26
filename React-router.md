# React 路由
* React 路由

react-router 是浏览器和原生应用的通用部分

react-router-native 是用于原生应用的

react-router-dom 是用于浏览器的
* 安装

npm install --save react-router-dom
* 引入

import { BrowserRouter as Router,Route,Link} from 'react-router-dom';
# 路由配置
* 结构
```
import React,{Component} from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter as Router,Route,Link} from 'react-router-dom';
import Home from './components/router/home';
import About from './components/router/about';
import News from './components/router/news';
import './index.css';
class App extends Component{
    render(){
        return (
            <Router>
                <div>
                    <ul>
                        <li><Link to="/">首页</Link></li>
                        <li><Link to="/about">关于我们</Link></li>
                        <li><Link to="/news">新闻</Link></li>
                    </ul>
                    <hr/>
                    {/* exact 是严格匹配 */}
                    <Route exact  path='/' component={Home}/>
                    <Route path='/about' component={About}/>
                    <Route path='/news' component={News}/>
                </div>
            </Router>
        )
    }
}
ReactDOM.render(
    <App/>,
    document.getElementById('root')
);
```
* BrowserRouter VS HashRouter

都是路由的基本组件，需将其放在最外层，选其一使用

内部只能含有一个子节点

BrowserRouter 的 URL 是挃向真实 URL 的资源路径，页面和浏览器的 history 保持一致

HashRouter 使用 URL 中的 hash（#）部分去创建路由
*  Switch

只会挂载与 <Route> 路径匹配成功的第一个
```
<Switch> 
  <Route exact path="/" component={ Home }/> 
  <Route path="/about" component={ About }/> 
  <Route path="/:user" component={ User }/> 
  <Route component={ NoMatch }/> 
</Switch>
```
* Link
```
//to : string
<Link to="/home">Home</Link> 

//to : object
<Link to={ { 
  pathname: '/courses', 
  search: '?sort=name', 
  hash: '#the-hash',
  state:{id : 1} 
  // 传到组件的数据，通过 this.props.location.state 获取
} } > Courses </Link>
```
* Redirect
```
<Switch>
  // 当被挂载时，将路由重定向为 to 属性指定的地址
  // from 和 exact 两个属性只能用在 <Switch> 组件下的 < Redirect />
  <Redirect from = '/old' to = '/new' />
  <Route path='/new' component={ New }/>
</Switch>
```
# 动态路由
* 路由配置
```
<Route path='/content/:id' component={ Content }/>
<Link to={ '/content/1' }>{ item.title }</Link>

//match：包含了具体的 url 信息，在 params 字段中可以获取到各个路由参数的值
<div>{this.props.match.params}</div>
```
* 路由get传值
```
<Route path='/content' component={ Content }/>
<Link to={ `/content?id=1` }>{item.title}</Link>

// 数据存放在 this.props.location.search
```
