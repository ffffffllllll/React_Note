######Table不添加分页效果

```
<Table
	pagination={{hideOnSinglePage:true,pageSize:100}}
/>
```
######给Modal增加高度
- 可以给Modal的内容添加一个div，给div一个高度，从而可以增加Modal的高度

```
 <Modal
	title="Modal"
	visible={visible}
	onOk={this.handleOk}
	onCancel={this.handleCancel}
	okText="确认"
	cancelText="取消"
	width="650px"
	heigth="1000px"
 >
	<div style={{height:400}}>  //可以增加高度
		<AePopModal data={data}/>
	</div>
 </Modal>
```
######不可编辑
父组件

```
let operationFlag = this.judgeOperation(record.i);
return (
	<span>
		<EditSpan
			disabled={operationFlag}
		/>
	</span>
)
/**判断编辑可否功能 */
judgeOperation = (record) => {
	if(record==="不通过"){
		return false;
	}else{
		return true;
	}
};
/**---------------------- */
```
子组件

```
<a
	href="#"
	disabled={this.props.disabled}
>删除</a>
```