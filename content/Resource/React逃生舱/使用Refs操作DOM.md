---
date created: 2022-09-13
date modified: 2022-09-28
---

React自动更新DOM去匹配渲染输出  
在一些场景下，你需要访问由React管理的DOM元素：节点聚焦、滚动到节点或测量其大小和位置

```js
const myRef = useRef(null);

<div ref={myRef}>

// 你可以使用任意的浏览器API
myRef.current.scrollIntoView();
```

传递函数给ref属性——ref callback

保存从项目 ID 到 DOM 节点的映射

```js
<li  

key={cat.id}  

ref={node => {  

const map = getMap();  

if (node) {  

// Add to the Map  

map.set(cat.id, node);  

} else {  

// Remove from the Map  

map.delete(cat.id);  

}  

}}  

>
```

访问其它组件的 DOM 节点  
React 默认不允许组件访问其它组件的 DOM 节点

想要公开其 DOM 节点的组件必须选择加入该行为  
组件可以指定将其引用“forwards”到其子级之一

```js
const MyInput = forwardRef((props, ref) => {  
	return <input {...props} ref={ref} />;  
});
```

本质上是转发行为  
**将由外部定义的ref触发的行为传递给内部指定的 DOM 来触发**

- 低级组件（按钮、输入框）——将其引用转发到其 DOM 节点
- 高级组件（表单、列表）——不会公开其 DOM 节点，以避免对 DOM 结构的意外依赖

使用命令式句柄公开 API 的子集——约束转发行为  
使用 ImperativeHandle 指示 React 提供您自己的特殊对象作为父组件的ref值

```js
const MyInput = forwardRef((props, ref) => {
  const realInputRef = useRef(null);
  useImperativeHandle(ref, () => ({
    // Only expose focus and nothing else
    focus() {
      realInputRef.current.focus();
    },
  }));
  return <input {...props} ref={realInputRef} />;
});
```

React更新

- 在 render 期间，React 调用你的组件来弄清楚屏幕上应该有什么
- 在 commit 期间，React 应用更改到 DOM 中

在 commit 期间设置ref.current。在更新 DOM 之前，React 会将受影响的ref.current 值设置为null。更新 DOM 后，React 会立即将它们设置为相应的 DOM 节点

刷新状态与刷新同步同步更新  
React 状态更新已排队,因此 DOM 操作是过时的

您可以强制 React 同步更新（“刷新”）DOM。为此，请从 react-dom 导入 flushSync，并将状态更新包装到 flushSync 调用中

```js
flushSync(() => {  
	setTodos([ ...todos, newTodo]);  
});  

listRef.current.lastChild.scrollIntoView();
```

使用 refs 进行 DOM 操作的最佳实践  
Refs 是一个逃生舱口。只有当你必须“走出 React”时，你才应该使用它们

- 避免更改由 React 管理的 DOM 节点
- 您可以安全地修改 React 没有理由更新的 DOM 部分
