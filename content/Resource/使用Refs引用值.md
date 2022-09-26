---
date created: 2022-09-13
date modified: 2022-09-27
---

想要组件记住一些信息，但不想触发新的渲染，可以使用ref

### 在组件中添加ref

```js
const ref = useRef(0)

{
	current: 0
}
```

通过ref.value访问ref的当前值  

1. 可变的  
2. 黑盒——react不会追踪  
——> 单向数据流的逃生舱

**ref是一个可以读取和修改current属性的原生JavaScript对象  
refs 在重新渲染之间由 React 保留，不会触发重渲染**

1. 数据驱动UI——状态  
2. 数据持久化——ref

| refs                                                                                  | state                                                                                                                     |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| useRef(initialValue) 返回 { current: initialValue }  | `useState(initialValue)` 返回状态变量和状态设置函数的当前值 ( `[value, setValue]`) |
| 改变它不会触发重渲染  | 改变它触发重渲染                                        |
| 可变的——你能在渲染进程之外修改和更新current的值 | “不可变的”—你能使用状态设置函数去修改状态变量以排队重渲染   |
| 你不应该在渲染期间读取和重写current值  | 你能在任意时间读取值.然而，每个渲染都有它自己的状态快照，该状态不会改变  |

虽然useState和useRef都是React提供的，但原则上，useRef 可以在useState之上实现

```js
// Inside of React
function useRef(initialValue) {
	const [ref, unused] = useState({ current: initialValue });
	return ref;
}
```

React提供了useRef的内置版本

**场景**

- 存储timout ID
- 存储和操作DOM元素
- 存储计算 JSX 不需要的其他对象

**refs的最佳实践**

- 将refs视为逃生舱口
- 在渲染期间不要读写ref.current
