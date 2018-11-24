##React中响应式设计思想
- state：复杂存储组件里面的数据

```
this.state = {
	inputValue : '',
	list : []
}
```
- 事件绑定的时候，需要通过bind(this)对函数的作用域进行变更

```
<input
	value = {this.state.inputValue}
	//  当input输入框发生改变时，绑定的事件
	onChange = {this.handleInputChange.bind(this)}
/>
```
- 改变数据项inputValue中的内容，不能直接`this.state.inputValue = e.targer.value`来改变state中的内容。需要使用`this.setState({})`来改变

```
handeInputChange(e) {
	this.setStated({
		inputValue : e.target.value
	})
}
```
- e.targer.value可以获取输入的值

##实现组件的新增删除功能
- 花括号代表JSX表达式，但是每一个JSX表达式都需要一个return的返回效果

```
this.state = {
	list : []
}
<ul>
	{// 执行表达式 --map作用：遍历数组list,执行循环
		// 	 item：内容，index：下标
		this.state.list.map((item, index) =>{
			return <li key={index}>{item}</li>
		//	每一个li都需要一个key值，才不会出现警告。
		//  但是一般不用index做key值。往后讨论为什么.
		})
	}
</ul>
```
- 实现新增功能--通过按钮绑定点击事件(onClick)

```
通过onClick绑定一个方法
<button onClick={this.handleBtnClick.bind(this)}>提交
</button>
```
- 绑定事件的原理

```
handleBtnClick(){
	this.setState({
	//	...含义：代表展开list数组的内容；
	//	此处代码含义：展开list数组内容，再加上inputValue
	//	构成新的数组，再赋值给list数组
		list: [...this.state.list, this.state.inputValue],
	//	实现将输入框置空
		inputValue: ''
	})
}
```
- 实现删除功能--生成li标签的时候，通过点击onClick事件绑定一个方法

```
<ul>
	{//	遍历数组list，将item渲染到页面上
		this.state.list.map((item, index) => {
			return (	// 格式化，需要加（）
			<li 
				key={index} 		
	//添加index  需要将下标传递给handleItemDelete这个方法			
				onClick={this.handleItemDelete.bind(this, index)	
			}>
				{item}
			</li>
			)
		})
	}
</ul>
``` 
- 实现删除功能--handleItemDelete方法的编写

```
handleItemDelete(index) {
	const list = [...this.state.list]
	//	删除数组list下标为index的1个内容
	list.splice(index, 1);
	this.setState({	//改变数据
		list: list
	})
}
```

- React有个概念immutable--state不允许我们做任何改变

`this.state.list`不允许做任何改变

如果需要改变,则创建一个副本，来改变副本
`const list = [...this.state.list];`

##JSX语法细节

- < label >ABC< /label >实现当点击ABC时，焦点聚焦在其他地方

``` 
//实现当点击“输入内容”时，光标聚焦在输入框里
<lable htmlFor="insertArea">输入内容</label>
<input
	id = insertArea
 />
```

##拆分组件与组件之间的传值

######拆分组件--将一个个小组件组成最终的大组件

- 小组件通过使用`export default TodoItem`将小组件导出

- 大组件通过使用`import TodoItem form ',/TodoItem'`引入

- 最终，在大组件中，使用`<TodoItem />`组件标签来使用小组件

######父组件向子组件的传值
- 父组件通过“属性”的形式将数据来传给子组件，子组件通过"this.props.属性"来使用数据

- `<TodoItem content={item} />`通过自定义的content将item传给子组件。

- 组件中，通过prop来使用content

```
return <div>{this.props.content}</div>
```
来使用大组件里传来的content

######子组件如何调用父组件的内容，修改父组件的数据
- 父组件通过属性传值子组件
`<TodoItem content={item} index={index}/>`
- 在复杂的组件开发过程中，将函数的绑定，都放在类的构造函数中

```
constructor(props) {
	super(props);
	this.handleClick = this.handleClick.bind(this);
}
...
render(){
	return (
		<div onClick={this.handleClick}>
			{this.props.content}
		</div>
	)
}
//	通过子组件的handleClick函数调用父组件的handleItemDelete
(index)函数实现删除
handleClick() {
	
}

```
- 子组件调用父组件的方法--则须将父组件的方法传递给子组件

父组件

```
<div>
	<TodoItem
		content={item}
		index={index}
		//	传递时还需要将这方法绑定在父组件上
		deleteItem={this.handleItemDelete.bind(this)}
</div>
```
子组件

```
handleClick() {
	this.props.deleteItem(this.props.index)
}
```
