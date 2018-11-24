## React

###### 相关的三个库

- react.min.js-React的核心库
- react-dom.min.js-提供与DOM相关的功能
- babel.min.js-可以将ES6代码转换为ES5代码

### 元素渲染
--

###### 元素

元素是构成React应用的最小单位，用于描述屏幕上输出的内容
`const element = <h1>Hello, world!</h1>;`

###### ReactDOM.render()方法

ReactDOM.render()方法-将element元素渲染到id=example中

```
const element = <h1>Hello, world!</h1>;
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

###### 更新元素渲染

React元素都是不可变的。当元素被创建之后，你是无法改变其内容或属性的。

更新界面的唯一方法：创建一个新的元素，然后将它传入ReactDOM.render()方法

### React JSX
--

###### 表达式

在 JSX 中使用 JavaScript 表达式。表达式写在花括号 {} 中。

`<h1>{1+1}</h1>`

在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。

`<h1>{i == 1 ? 'True!' : 'False'}</h1>`

###### 样式

React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。

```
var myStyle = {
    fontSize: 100,
    color: '#FF0000'
};
ReactDOM.render(
    <h1 style = {myStyle}>菜鸟教程</h1>,
    document.getElementById('example')
);
```

###### 注释

注释需要写在花括号中

```
<div>
	<h1>菜鸟教程</h1>
	{/*注释...*/}
</div>
```

###### 数组

JSX 允许在模板中插入数组，数组会自动展开所有成员

```
var arr = [
  <h1>菜鸟教程</h1>,
  <h2>学的不仅是技术，更是梦想！</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

###React组件
--
- 使用函数定义一个组件

```
function HelloMessage(props) {
    return <h1>Hello World!</h1>;
}
```
- 用户自定义组件

```
const element = <HelloMessage />
```
>注意，原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。

- 向组件传递参数，可以使用this.props对象

```
function HelloMessage(props) {
    return <h1>Hello {props.name}!</h1>;
}
 
const element = <HelloMessage name="Runoob"/>;
 
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

###React State（状态）
--
######组件、状态思想
所谓组件，就是状态机器
<br>
React将用户界面看做简单的状态机器。当组件处于某个状态时，那么就输出这个状态对应的界面。通过这种方式，就很容易去保证界面的一致性。
<br>
在React中，你简单的去更新某个组件的状态，然后输出基于新状态的整个界面
<br>
这种组件模型简化了我们思考的方式：对组件的管理就是对状态的管理。不同于其它框架模型，React组件很少需要暴露组件方法和外部交互。例如，某个组件有只读和编辑两个状态。一般的思路可能是提供beginEditing()和endEditing()这样的方法来实现切换；而在React中，需要做的是setState({editing: true/false})。在组件的输出逻辑中负责正确展现当前状态。这种方式，你不需要考虑beginEditing和endEditing中应该怎样更新UI，而只需要考虑在某个状态下，UI是怎样的。显然后者更加自然和直观。

######介绍

React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。

```
<div id="example"></div>
    <script type="text/babel">
        class Clock extends React.Component {
            constructor(props) {
                super(props);
                this.state = {date: new Date(),  //通过state里不同属性，来选择不同的输出
                    name : "abc"};
            }

            render() {
                return (
                    <div>
                        <h1>Hello, world!</h1>
                        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
                        <h2>现在是 {this.state.name}.</h2>
                    </div>
                );
            }
        }

        ReactDOM.render(
            <Clock />,
            document.getElementById('example')
        );
    </script>
```
>通过this.state={A:xx , B:xx ,C:xx}中不同的属性，在其他地方单独调用A or B or C 来获得界面更新和输出

######通过类来渲染组件，并将类中的render(){}调用到HTML文件中

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
 
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
 
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
 
  tick() {
    this.setState({
      date: new Date()
    });
  }
 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>现在是 {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
 
ReactDOM.render(
  <Clock />,
  document.getElementById('example')
);
```
> constructor和render是必须的，ReactDOM.render(类A，document.getElementById('id')，将类A的render()中的返回结果渲染到HTML页面中。而constructor(props){ super(props);this.state = {A :  xx};}是用来初始化类A参数props和状态state
> componentDidMount() 与 componentWillUnmount() 方法被称作生命周期钩子。在组件输出到 DOM 后会执行 componentDidMount() 钩子，我们就可以在这个钩子上设置一个定时器。
this.timerID 为计算器的 ID，我们可以在 componentWillUnmount() 钩子中卸载计算器。



##核心思想
- React 的核心思想是：封装组件
- 各个组件维护自己的状态和 UI，当状态变更，自动重新渲染整个组件。
- 不再需要不厌其烦地来回查找某个 DOM 元素，然后操作 DOM 去更改 UI。

######组件
- `render`将组件显示到页面上的某个元素里面。
- `props`看作是组件的配置属性，在组件内部是不变的，只是在调用这个组件的时候传入不同的属性来定制显示这个组件
- `state`组件状态，当组件状态有更改的时候，React会自动调用组件的`render`方法重新渲染整个组件的UI-----why?

######JSX
- 可以将HTML直接嵌入到JS代码里面

```
class HelloMessage extends Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
```

######Virtual DOM
- 当组件状态`state`有更改的时候，React会自动调用组件的`render` 方法重新渲染整个组件的UI。

```
render(<HelloMessage name="John" />, mountNode);
```

##开发配置环境
- JSX支持
- ES6支持--编译工具 Babel 

##JSX
######为什么要引入JSX语法
- 就是为了把 HTML 模板直接嵌入到 JS 代码里面，这样就做到了模板和组件关联

######JSX是可选的
- 通过`React.createElement` 来构造组件的 DOM 树。第一个参数是标签名，第二个参数是属性对象，第三个参数是子元素。包含子元素的例子

```
var child = React.createElement('li', null, 'Text Content');
var root = React.createElement('ul', { className: 'my-list' }, child);
React.render(root, document.body);
```
其中`className`相当于js中的`class`，因为跟其他冲突，所以改用className

###使用JSX
利用 JSX 编写 DOM 结构，可以用原生的 HTML 标签，也可以直接像普通标签一样引用 React 组件。这两者约定通过大小写来区分，小写的字符串是 HTML 标签，大写开头的变量是 React 组件。

######使用JavaScript表达式
- 属性值使用表达式，只要用`{}`替换`""`
- 子组件也可以作为表达式使用

```
// Input (JSX):
var content = <Container>{window.isLoggedIn ? <Nav /> : <Login />}</Container>;
// Output (JS):
var content = React.createElement(
  Container,
  null,
  window.isLoggedIn ? React.createElement(Nav) : React.createElement(Login)
);
```

######注释
在 JSX 里使用注释也很简单，就是沿用 JavaScript，唯一要注意的是在一个组件的子元素位置使用注释要用 {} 包起来。

```
var content = (
  <Nav>
      {/* child comment, put {} around */}
      <Person
        /* multi
           line
           comment */
        name={window.isLoggedIn ? window.name : ''} // end of line comment
      />
  </Nav>
);
```

######HTML转义
React 会将所有要显示到 DOM 的字符串转义，防止 XSS。所以如果 JSX 中含有转义后的实体字符比如 `&copy;` (©) 最后显示到 DOM 中不会正确显示，因为 React 自动把 `&copy;` 中的特殊字符转义了。有几种解决办法：

- 使用数组组装`<div>{['cc ', <span>&copy;</span>, ' 2015']}</div>`
- 直接插入原始的 HTML`<div dangerouslySetInnerHTML={{__html: 'cc &copy; 2015'}} />`


######自定义HTML属性
如果在 JSX 中使用的属性不存在于 HTML 的规范中，这个属性会被忽略。如果要使用自定义属性，可以用 data- 前缀。

###属性扩散
有时候你需要给组件设置多个属性，你不想一个个写下这些属性，或者有时候你甚至不知道这些属性的名称，这时候 spread attributes 的功能就很有用了
比如：

```
var props = {};
props.foo = x;
props.bar = y;
var component = <Component {...props} />;
```
`props` 对象的属性会被设置成 `Component` 的属性。
属性也可以被覆盖：

```
var props = { foo: 'default' };
var component = <Component {...props} foo={'override'} />;
console.log(component.props.foo); // 'override'
```
写在后面的属性值会覆盖前面的属性。

