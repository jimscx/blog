---
title: react 入门
date: 2016-10-2 22:21:03
tags: vue react
---

#### 1.react入坑
react的jsx语法着实让人用着挺爽的，不用再去拼接html字符串了。jQuery也几乎可以摒弃了。。。但是当遇到Flux--->redux 的时候感觉真的很是反人类。
react语法看官网就很容易理解，抓住state和props,操作它们就好，可是redux真的不太容易理解。

#### 1.1. 语法
看官网就可，使用vscode编码下载jsx语法插件也是很舒服的
#### virtualDom
* 原生的dom
   当你用document.createElement()创建一个空的Element的时候（比如创建一个空的div），有以下这几页的东西需要实现（当然， 这不是标准，只是个大概的意思）：
  HTMLElement - Web API Interfaces
  Element - Web API Interfaces
  GlobalEventHandlers
  非常非常多，并且还有不少嵌套引用，反正就是慢。
  
* 相对于 DOM 对象，原生的 JavaScript 对象处理起来更快，而且更简单。DOM 树上的结构、属性信息我们都可以很容易地用 JavaScript 对象表示出来：
  ```
    var element = {
    tagName: 'ul', // 节点标签名
    props: { // DOM的属性，用一个对象存储键值对
      id: 'list'
    },
    children: [ // 该节点的子节点
      {tagName: 'li', props: {class: 'item'}, children: ["Item 1"]},
      {tagName: 'li', props: {class: 'item'}, children: ["Item 2"]},
      {tagName: 'li', props: {class: 'item'}, children: ["Item 3"]},
    ]
  	}
  ```
  对应的html 就是这样的
  ```
   <ul id='list'>
      <li class='item'>Item 1</li>
      <li class='item'>Item 2</li>
      <li class='item'>Item 3</li>
	</ul>
  ```
#### diff算法
#### **如何实现一个diff算法**
1. 步骤一：用JS对象模拟DOM树
1. 步骤二：比较两棵虚拟DOM树的差异
1.  步骤三：把差异应用到真正的DOM树上

#### 1.2. react中的注释
同JavaScript注释一样
* 单行注释 // comments
* 多行注释 /* comments */
* React推荐在父组件与子组件间写注释的时候，要加{} 类似于这样{/* comments */}.

#### 1.3. 路由的基本使用 [react-router](https://github.com/reactjs/react-router)

#### 1.3.1 首先使用根路由组件 `import Router from 'react-router';`然后使用{} 的形式把你定义的路由规则加载进来
```
import React from 'react';
import Router from 'react-router';
import ReactDOM from 'react-dom';
import createBrowserHistory from 'history/lib/createBrowserHistory';//h5路由形式去掉 # 
import routes from './routes';
let history = createBrowserHistory();
ReactDOM.render(<Router history={history}>{routes}</Router>, document.getElementById('app'));
```
定义自己的路由规则 routes.js
```
export default (
  <Route component={App}>
      <Route path='/index' component={Home} />
      <Route path='/focus' component={Focus} />
      <Route path='/info' component={Info} />
      <Route path='/supplementInfo/:name/:email/:openId/:labId' component={SupplementInfo} />
  </Route>
  )
```
定义根组件
```
.....App.js组件
import React from 'react';
class App extends React.Component {
	render() {
		return (
			<div className="page">
				{this.props.children}
			</div>
		);
	}
}
```
Link的使用 Link组件用于取代<a>元素，生成一个链接，允许用户点击后跳转到另一个路由。它基本上就是<a>元素的React版本，可以接收Router的状态。

```
import { Router, Route, Link } from 'react-router'

this.state.users.map(user => (
              <li key={user.id}><Link to={`/user/${user.id}`}>{user.name}</Link></li>
            ))
```
#### 1.3.2 react 路由中参数的获取
* 类似于`/home/?id=1`

```
  使用`this.props.location.query.id`
```
* 类似于`/home/:id`

```
使用`this.props.params.id`
```

#### 1.4. react的生命周期

生命周期大致可以描述为：实例化、存在期和销毁时

1. 客户端的实例化：
    * **getDefaultProps**
    * **getInitialState**
    * **componentWillMount**
     该方法在首次渲染之前调用，也是再 render 方法调用之前修改 state 的最后一次机会。
    *  **render**
      render方法返回的结果并不是真正的DOM元素，而是一个虚拟的表         现，类似于一个DOM tree的结构的对象。react之所以效率高，就是          这个原因。
    *  **componentDidMount**
      该方法被调用时，已经渲染出真实的 DOM，可以再该方法中通过            this.getDOMNode() 访问到真实的 DOM(推荐使用               ReactDOM.findDOMNode())。
 1. 存在期
     * **componentWillReceiveProps**
      组件的 props 属性可以通过父组件来更改，这时，componentWillReceiveProps 将来被调用。可以在这个方法里更新 state,以触发 render 方法重新渲染组件。
	 * **shouldComponentUpdate**
      如果你确定组件的 props 或者 state 的改变不需要重新渲染，可以通过在这个方法里通过返回 false 来阻止组件的重新渲染，返回 false 则不会执行 render 以及后面的 componentWillUpdate，componentDidUpdate 方法。
	 * **componentWillUpdate**
	  这个方法和 componentWillMount 类似，在组件接收到了新的 props 或者 state 即将进行重新渲染前
	 * **componentDidUpdate**
      这个方法和 componentDidMount 类似，在组件重新被渲染之后，componentDidUpdate(object prevProps, object prevState) 会被调用。可以在这里访问并修改 DOM。
      
1. 销毁时
     * **componentWillUnmount**
     每当React使用完一个组件，这个组件必须从 DOM 中卸载后被销毁，此时 componentWillUnmout 会被执行，完成所有的清理和销毁工作，在 conponentDidMount 中添加的任务都需要再该方法中撤销，如创建的定时器或事件监听器


#### 1.5. react 的state和props

props 的区别在于 state只存在组件的内部，props 在所有实例中共享。所以说别调用 this.setProps 或者 直接修改 this.props，把它当成只读的数据最好。props就是牵一发动全身的主。

#### 1.6 父子组件的通信
