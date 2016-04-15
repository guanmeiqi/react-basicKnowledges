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
---------------------------------
React 提供一个工具方法 React.Children 来处理 this.props.children 。用
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
虚拟事件对象:事件处理器将会传入虚拟事件对象的实例,一个对浏览器本地事件的跨浏览器封装。
它有和浏览器本地事件相同的属性和方法,包括stopPropagation()和preventDefault(),
但是没有浏览器兼容问题。
```javascript
 //props 事件处理器
    var HelloWorld = React.createClass({
        propTypes: {
            onClick: React.PropTypes.func.isRequired
        },
        clickHandle: function () {
            this.props.onClick.apply(this);
        },
        render: function () {
            return (
                    < section
            onClick = {this.clickHandle} >
                      ...
                    </section >
                   );
         }
    })

    //传入事件处理器
    var TouristSpots=React.createClass({
        clickHandle:function(word){
            report.reportClick("word",word.id);
        },
        render:function(){
            var word=this.props.word;
            return(
                    <div><HelloWorld word={word} onClick={this.clickHandle}/>
                    </div>
            );
        }
    })
```

[slide]

支持的事件  

剪贴板事件：
onCopy onCut onPaste  

键盘事件：
onKeyDown onKeyPress onKeyUp  

焦点事件：
onFocus onBlur 

表单事件：
onChange onInput onSubmit  

鼠标事件：
onClick onDoubleClick onDrag onDragEnd onDragEnter onDragExit onDragLeave
onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp 

触摸事件：
为了使触摸事件生效，在渲染所有组件之前调用 React.initializeTouchEvents(true)。
onTouchCancel onTouchEnd onTouchMove onTouchStart   

UI 事件：
onScroll   

鼠标滚轮滚动事件：
onWheel   

{:&.flexbox.vleft}
[slide]
表单事件处理

```
// 该表单组件里面用到了RadioButtons和Checkboxes
var FormApp = React.createClass({
    getInitialState:function(){
        return {
            inputValue: '请输入...',
            selectValue: 'A',
            radioValue:'B',
            checkValues:[],
            textareaValue:'请输入...'
        }
    },
    handleSubmit:function(e){
        e.preventDefault();
        var formData = {
            input: this.refs.goodInput.getDOMNode().value,
            select: this.refs.goodSelect.getDOMNode().value,
            textarea: this.refs.goodTextarea.getDOMNode().value,
            radio: this.state.radioValue,
            check: this.state.checkValues,
        }

        console.log('the form result is:')
        console.log(formData);

        this.refs.goodRadio.saySomething();

    },
    handleRadio:function(e){
        this.setState({
            radioValue: e.target.value,
        })
    },
    handleCheck:function(e){
        var checkValues = this.state.checkValues.slice();
        var newVal = e.target.value;
        var index = checkValues.indexOf(newVal);
        if( index == -1 ){
            checkValues.push( newVal )
        }else{
            checkValues.splice(index,1);
        }

        this.setState({
            checkValues: checkValues,
        })
    },
    render: function(){
        return (
            <form onSubmit={this.handleSubmit}>
                <input ref="goodInput" type="text" defaultValue={this.state.inputValue }/>
                <br/>
                选项：
                <select defaultValue={ this.state.selectValue } ref="goodSelect">
                    <option value="A">A</option>
                    <option value="B">B</option>
                    <option value="C">C</option>
                    <option value="D">D</option>
                    <option value="E">E</option>
                </select>
                <br/>
                <p>radio button!</p>
                <RadioButtons ref="goodRadio" handleRadio={this.handleRadio} />
                <br/>

                <Checkboxes handleCheck={this.handleCheck} />
                <br/>
                <textarea defaultValue={this.state.textareaValue} ref="goodTextarea"></textarea>
                <button type="submit">提交</button>

            </form>
        )
    }
});

// 定义单选框按钮组
var RadioButtons = React.createClass({
    saySomething:function(){
        alert("yo what's up man!");
    },
    render:function(){
        return (
            <span>
                A
                <input onChange={this.props.handleRadio} name="goodRadio" type="radio" value="A"/>
                B
                <input onChange={this.props.handleRadio} name="goodRadio" type="radio" defaultChecked value="B"/>
                C
                <input onChange={this.props.handleRadio} name="goodRadio" type="radio" value="C"/>
            </span>
        )
    }
});

var Checkboxes = React.createClass({
    render: function(){
        return (
            <span>
                A
                <input onChange={this.props.handleCheck}  name="goodCheckbox" type="checkbox" value="A"/>
                B
                <input onChange={this.props.handleCheck} name="goodCheckbox" type="checkbox" value="B" />
                C
                <input onChange={this.props.handleCheck} name="goodCheckbox" type="checkbox" value="C" />
            </span>
        )
    }
})


ReactDOM.render(<FormApp />, document.getElementById('app'));
```

[slide]
多个简单的组件嵌套，可构成一个复杂的复合组件，从而完成复杂的交互逻辑，实现页面功能。

```javascript
// 定义一个头像avatar的组件
var Avatar = React.createClass({
  render: function() {
    return (
      <div>
        <ProfilePic link={this.props.link} />
        <ProfileLink name={this.props.name} />
      </div>
    );
  }
});

// 定义一个人物图片ProfilePic组件
var ProfilePic = React.createClass({
  render: function() {
    return (
      <img src={this.props.link} />
    );
  }
});

// 定义一个人物链接ProfileLink组件
var ProfileLink = React.createClass({
  render: function() {
    return (
      <a href={'https://github.com/' + this.props.name}>
        {this.props.name}
      </a>
    );
  }
});

// 渲染到容器
ReactDOM.render(
  <Avatar
    name="GuoYongfeng"
    link="https://avatars2.githubusercontent.com/u/8686869?v=3&s=460"
  />,
  document.getElementById('example')
);

```

[slide]
React的组件拥有一套清晰完整而且非常容易理解的生命周期机制，大体可以分为三个过程：
初始化、更新和销毁，在组件生命周期中，随着组件的props或者state发生改变，它的虚拟DOM
和DOM表现也将有相应的变化。
[slide]
组件的生命周期分成三个状态：
---
- Mounting：已插入真实 DOM  
- Updating：正在被重新渲染   
- Unmounting：已移出真实 DOM  

---------------------
React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。
componentWillMount()
componentDidMount()
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
componentWillUnmount()
此外，React 还提供两种特殊状态的处理函数。
componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用

[slide]
不同生命周期内可以自定义的函数

----
1. 初始化  
getDefaultProps  只调用一次，实例之间共享引用
getInitialState  初始化每个实例特有的状态
conponentWillMount render之前最后一次修改状态的机会
render    只能访问this。props和this。state,只有一个顶层组件，不润许修改状态和DOM输出
componentDidMount  成功render并渲染完成真实DOM之后触发，可以修改DOM
2. 运行中  
componentWillReceiveProps 父组件修改属性触发，可以修改新属性，修改状态
shouldComponentUpdate  返回false会阻止render调用（让开发者决定是否更新）
componentWillUpdate（日志打印） 不能修改属性和状态
render    只能访问this。props和this。state,只有一个顶层组件，不润许修改状态和DOM输出
componentDidUpdate  可以修改DOM
3. 销毁  
componentWillUnmount  在删除组件之前进行清理操作，比如计时器和事件监听器


[slide]
mixin 是一个普通对象，通过 mixin 可以在不同组件间共享代码

```
var mixin = {
    propTypes: {
        title: React.PropTypes.string,
    },

    getDefaultProps(){
        return {
            title:'default'
        };
    },
};

var A = React.createClass({
    mixins: [mixin],
    render(){
        return <i>{this.props.title}</i>
    }
});

var B = React.createClass({
    mixins: [mixin],
    render(){
        return <b>{this.props.title}</b>
    }
});

React.render(<div>
<B/> <A title={2}/>
<A/>
</div>,document.getElementById('container'));

```

[slide]

```
// 定义一个按钮组件
var BootstrapButton = React.createClass({
  render: function() {
    return (
      <a {...this.props}
        href="javascript:;"
        role="button"
        className={(this.props.className || '') + ' btn'} />
    );
  }
});
// 定义一个弹框组件
var BootstrapModal = React.createClass({
  // 节点插入到真实的DOM，使用jquery
  componentDidMount: function() {
    // 调用bootstrap插件
    $(this.refs.root).modal({backdrop: 'static', keyboard: false, show: false});
  },
  // 在组件销毁的时候，记得把之前绑定的方法给干掉
  componentWillUnmount: function() {
    $(this.refs.root).off('hidden', this.handleHidden);
  },
  close: function() {
    $(this.refs.root).modal('hide');
  },
  open: function() {
    $(this.refs.root).modal('show');
  },
  render: function() {
    var confirmButton = null;
    var cancelButton = null;

    if (this.props.confirm) {
      confirmButton = (
        <BootstrapButton
          onClick={this.handleConfirm}
          className="btn-primary">
          {this.props.confirm}
        </BootstrapButton>
      );
    }
    if (this.props.cancel) {
      cancelButton = (
        <BootstrapButton onClick={this.handleCancel} className="btn-default">
          {this.props.cancel}
        </BootstrapButton>
      );
    }

    return (
      <div className="modal fade" ref="root">
        <div className="modal-dialog">
          <div className="modal-content">
            <div className="modal-header">
              <button
                type="button"
                className="close"
                onClick={this.handleCancel}>
                &times;
              </button>
              <h3>{this.props.title}</h3>
            </div>
            <div className="modal-body">
              {this.props.children}
            </div>
            <div className="modal-footer">
              {cancelButton}
              {confirmButton}
            </div>
          </div>
        </div>
      </div>
    );
  },
  handleCancel: function() {
    if (this.props.onCancel) {
      this.props.onCancel();
    }
  },
  handleConfirm: function() {
    if (this.props.onConfirm) {
      this.props.onConfirm();
    }
  }
});

// 调用刚才咱们定义的两个组件，写咱们的业务组件
var Example = React.createClass({
  handleCancel: function() {
    if (confirm('亲，确定要取消么')) {
      this.refs.modal.close();
    }
  },
  render: function() {
    var modal = null;
    modal = (
      <BootstrapModal
        ref="modal"
        confirm="OK"
        cancel="Cancel"
        onCancel={this.handleCancel}
        onConfirm={this.closeModal}
        title="Hello, Bootstrap!">
          这是一个结合jQuery和Bootstrap而写的组件
      </BootstrapModal>
    );
    return (
      <div className="example">
        {modal}
        <BootstrapButton onClick={this.openModal} className="btn-default">
          Open modal
        </BootstrapButton>
      </div>
    );
  },
  openModal: function() {
    this.refs.modal.open();
  },
  closeModal: function() {
    this.refs.modal.close();
  }
});

ReactDOM.render(<Example />, document.getElementById('example'));
```


