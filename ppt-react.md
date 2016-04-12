title: react基础
speaker: tammy_girl
url: https://github.com/guanmeiqi/react-basicKnowledges
transition: cards
files: /js/demo.js,/css/demo.css

[slide]

# React基础学习
## a javascript library for building user interfaces

[slide]

# 1.jsx
# 2.component
# 3.Data Flow 
# 4.VirtualDOM  {:&.flexbox.vleft}
# 5.event

[slide]

## 使用 React 的网页源码，结构大致如下
----

```HTML
<!DOCTYPE html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
          <h1>Hello, world!</h1>,
                  document.getElementById('example')
          );
    </script>
  </body>
</html>

```

[slide]

# 1.jsx

[slide]

## 语法
----

```javascript
类似 xml 的语法，用来描述组件树
var ExampleComponent = React.createClass({
         render: function () {
             return (
                     <div className="navigation">
                     Hello World!
             </div>
             );
         }
     });
     
     <div classname="x">
          <a href="#">#</a>
             <component x="y">1</component>
      </div>
```

```javascript
不用JSX，用React提供的API写的话
var ExampleComponent = React.createClass({
        render: function () {
            return (
                    React.createElement('div', {className: 'navigation'}, 'Hello World!')
            );
        }
    });
    
     React.createElement('div',{className:'x'},[
         React.createElement('a',{href:'#'},'#'),
         React.createElement(Component,{x:'y'},1);
    ]);
```

[slide]

## 命名、注释、根元素个数、JSX 嵌入变量
----

```javascript
    // 1. 组件命名遵循驼峰命名，首字母大写
    var  ComponentDemo = React.createClass ({
         render:function(){
            {
                /* 2. 在一个组件的子元素位置使用注释要用 {} 包起来
                 这是代码注释
                 也可以是多行
                 */
            }
            var name = this.props.name;
            // 3. render 方法 return 回来的根元素只能是一个，超过会报错
            //React只有一个限制， 组件只能渲染单个根节点。如果你想要返回多个节点，
            它们必须被包含在同一个节点里
            // 4. { } 里面可以写JS代码
            return (
            <div>
            hello, {name ? name : "我是默认的"}
            </div>
        );
        }
    })
    ReactDOM.render(<ComponentDemo/>, document.getElementById('example'));
    //5.JSX 嵌入变量
    //可以通过 {变量名} 来将变量的值作为属性值
    var x = 'http://www.alipay.com';
    var y = <a href= {x} target="_blank">alipay.com</a>;
    ReactDOM.render(y, document.getElementById('container'));
```

[slide]

##  styles
----

```javascript
var StyleDemo = React.createClass({
        render:function(){
            // 6. 在JS文件里面给组件定义样式
            var MyComponentStyles = {
                color: 'blue',
                fontSize: '28px'
            };
            return (
                    <div style={MyComponentStyles}>
                    可以直接这样写行内样式
                    </div>
        )
        }
    })
   ReactDOM.render(<StyleDemo/>, document.getElementById('example'));
```

[slide]

## HTML 转义

----
React会将所有显示到DOM的字符转义，防止XSS。所以如果JSX中含有转义后的实体字符比如
(&copy);©最后显示到DOM中不会正确显示，因为React自动把(&copy);中的特殊字符转义了。
有几种解决办法：

---- 
- 直接使用 UTF-8 字符 ©
- 使用对应字符的 Unicode 编码，查询编码
- 使用数组组装
```html
<div>{['cc ', <span>&copy;</span>, ' 2015']}</div>
```
- 直接插入原始的 HTML
```html
<div dangerouslySetInnerHTML = {{__html: 'cc &copy; 2015'}} />
```

[slide]

## JSX SPREAD 
----
可通过{...obj}来批量设置一个对象的键值对到组件的属性(注意顺序)

```javascript
 var props = {};
     props.foo = x;
     props.bar = y;
     var component = <Component {...props} />;
     //props 对象的属性会被设置成 Component 的属性。

```
- 属性也可以被覆盖：
```javascript
     var props = { foo: 'default' };
     var component = <Component {...props} foo={'override'} />;
     console.log(component.props.foo); // 'override'
     //写在后面的属性值会覆盖前面的属性。
```
- 属性名不能和 js 关键字冲突

例如：class-className, readOnly, for-htmlfor

[slide]

# 2.component

[slide]

## 最简单的React组件及其渲染

----

```javascript
var HelloMessage = React.createClass({
  render: function() {
    return <div>Hello World!</div>;
  }
});
// 加载组件到 DOM 元素 mountNode
//React.render(<HelloMessage />, mountNode);
// 我们可以看看React给我们提供了哪些顶层组件
console.log( React );

ReactDOM.render(<HelloMessage />, mountNode);
```

变量 HelloMessage 就是一个组件类。模板插入 <HelloMessage /> 时，会自动生成 
HelloMessage 的一个实例。所有组件类都必须有自己的 render 方法，用于输出组件。

[slide]

## React相关文件顶层api介绍

### react.js
* React.Component   使用ES6的class创建组件用的API
- React.createClass 使用ES5的class创建组件用的API
- React.PropTypes   
- React.Children    操作 map/forEach children 工具类
### 还有几个不是很常用的
- React.createElement
- React.cloneElement
- React.createFactory
- React.DOM

[slide]

### react-dom.js
- ReactDOM.render 渲染组件到 dom
                  用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
- ReactDOM.unmountComponentAtNode
- ReactDOM.findDOMNode

### react-dom-server.js
- ReactDOMServer.renderToString
- ReactDOMServer.renderToStaticMarkup

[slide]

### 一个最基本的React组件由数据和JSX两个主要部分构成 
----

这是一个简单完整的React组件【类】   
 
```javascript
var Hello = React.createClass({
//组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。
   propTypes:{
      name:React.PropTypes.String.isRequired
      },
   getInitialState:function(){
      return{
         name:this.ptops.name};
      },
   reader :function(){
      return{
        <ul>
           <li>hello{this.state.name}!</li>
        </ul>};
      }
})
```

- props 主要作用是提供数据来源，可以简单的理解为 props 就是构造函数的参数。
- state 唯一的作用是控制组件的表现，用来存放会随着交互变化状态，比如开关状态等。
- JSX 做的事情就是根据 state 和 props 中的值，结合一些视图层面的逻辑，
输出对应的 DOM 结构。

[slide]

# 3.Data Flow

[slide]

## “单向数据绑定”是 React 推崇的一种应用架构的方式。
----
在React中，数据流是自上而下单向的从父节点传递到子节点，所以组件是简单且容易把握的，
他们只需要从父节点提供的props中获取数据并渲染即可。如果顶层组件的某个prop改变了，
React会递归地向下遍历整棵组件数，重新渲染所有使用这个属性的组件。

<div>
    <img src="/imgs/flow.png" height="250">
</div>

[slide]

## this.props
----

```javascript
var HelloMessage = React.createClass({
         render: function() {
               return <div>Hello {this.props.name}</div>;
          }
});
// 加载组件到 DOM 元素 mountNode
ReactDOM.render(<HelloMessage name="John"/>, mountNode);
```
props 是组件包含的两个核心概念之一，另一个是 state（这个组件没用到）。
可以把 props 看作是组件的配置属性，在组件内部是不变的，只是在调用这个组件的时候传入
不同的属性（比如这里的 name）来定制显示这个组件。组件的属性可以在组件类的 
this.props 对象上获取，比如 name 属性就可以通过 this.props.name 读取。

[slide]

## propTypes
----

组件的属性可以接受任意值，字符串、对象、函数等等都可以。
有时，我们需要一种机制，验证别人使用组件时，提供的参数是否符合要求。

- 组件类的PropTypes属性，就是用来验证组件实例的属性是否符合要求。

```javascript
var MyTitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },

  render: function() {
     return <h1> {this.props.title} </h1>;
   }});
//上面的Mytitle组件有一个title属性。PropTypes 告诉 React，这个 title 属性是
必须的，而且它的值必须是字符串。现在，我们设置 title 属性的值是一个数值。
var data = 123;

ReactDOM.render(
  <MyTitle title={data} />,
  document.body
);
//这样一来，title属性就通不过验证了。控制台会显示一行错误信息。
```

[slide]

- getDefaultProps 方法可以用来设置组件属性的默认值
（这是ES5的写法，ES6定义组件的默认props可以直接写props）
----

```javascript
var MyTitle = React.createClass({
  getDefaultProps : function () {
    return {
      title : 'Hello World'
    };
  },

  render: function() {
     return <h1> {this.props.title} </h1>;
   }});

ReactDOM.render(
  <MyTitle />,
  document.body
);
//上面代码会输出"Hello World"。
```

[slide]

## this.props.children
----

this.props对象的属性与组件的属性一一对应，但是有一个例外，
就是this.props.children属性。它表示组件的所有子节点
```javascript
var NotesList = React.createClass({
  render: function() {
    return (                                                                                                                  
      <ol>{
        React.Children.map(this.props.children, function (child) {
          return <li>{child}</li>;})
      }</ol>
    );
  }});
  
ReactDOM.render(<NotesList>
    <span>hello</span><span>world</span>
                </NotesList>，document.body);
```
注意:this.props.children的值有三种可能:

- 如果当前组件没有子节点,它就是 undefined;
- 如果有一个子节点，数据类型是 object ;
- 如果有多个子节点，数据类型就是 array 。
所以，处理 this.props.children 的时候要小心。

[slide]

React 提供一个工具方法 React.Children 来处理 this.props.children 。我们可以用
 React.Children.map 来遍历子节点，而不用担心 this.props.children 的数据类型是
  undefined 还是 object。

[slide]

## this.state
----

组件免不了要与用户互动，React 的一大创新，就是将组件看成是一个状态机，一开始有一个
初始状态，然后用户互动，导致状态变化，从而触发重新渲染 UI  
```javascript
var LikeButton = React.createClass({
  getInitialState: function() {
    return {liked: false};
  },
  handleClick: function(event) {
    this.setState({liked: !this.state.liked});
  },
  render: function() {
    var text = this.state.liked ? 'like' : 'haven\'t liked';
    return (
      <p onClick={this.handleClick}>
        You {text} this. Click to toggle.
      </p>
    );
  }});
```
上面代码是一个 LikeButton 组件，它的 getInitialState 方法用于定义初始状态，也就
是一个对象，这个对象可以通过 this.state 属性读取。当用户点击组件，导致状态变化，
this.setState 方法就修改状态值，每次修改以后，自动调用 this.render 方法，
再次渲染组件。

[slide]

this.props 和 this.state 都用于描述组件的特性，可能会产生混淆

----
## 简单的区分方法:

- this.props 表示那些一旦定义，就不再改变的特性，
- this.state 是会随着用户互动而产生变化的特性。

[slide]

# 4.Virtual DOM

[slide]

- 当组件状态 state 有更改的时候，React 会自动调用组件的 render 方法重新渲染整个组件的 UI。

----
如有大面积的操作 DOM，性能会是一个很大的问题，所以 React 实现了一个虚拟 DOM，组件 
DOM 结构就是映射到这个虚拟 DOM 上，React 在这个虚拟 DOM 上实现了一个 diff 算法，

<div>
    <img src="/imgs/dom.png" height="250">
</div>

当要更新组件的时候，会通过 diff 寻找到要变更的 DOM 节点，再把这个修改更新到浏览器实
际的 DOM 节点上，所以实际上不是真的渲染整个 DOM 树。这个虚拟 DOM 是一个纯粹的JS数
据结构，所以性能会比原生 DOM 快很多


[slide]

----

- 缺陷——首次渲染大量DOM时因为多了一层虚拟DOM的计算，会比innerHTML插
入方式慢。

[slide]

- 从组件获取真实 DOM 的节点，用 ref 属性

----

```javascript
var MyComponent = React.createClass({
  handleClick: function() {
    this.refs.myTextInput.focus();
  },
  render: function() {
    return (
      <div>
        <input type="text" ref="myTextInput" />
        <input type="button" value="Focus the text input" onClick={this.handleClick} />
      </div>
    );
  }});

ReactDOM.render(<MyComponent />,document.getElementById('example'));
```

上面代码中，组件 MyComponent 的子节点有一个文本输入框，用于获取用户的输入。这时就必
须获取真实的 DOM 节点，虚拟 DOM 是拿不到用户输入的。为了做到这一点，文本输入框必须有
一个 ref 属性，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。
注意：由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入
文档以后，才能使用这个属性，否则会报错。上面代码中，通过为组件指定 Click 事件的回调
函数，确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName] 
属性

[slide]

# 5.event

[slide]

[slide]
[slide]

