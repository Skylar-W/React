## React组件
### React组件的两种创建方式
#### 函数创建组件
  使用JS函数(包括箭头函数)创建的组件
  规定:
  1. 函数名首字母`必须大写`
  2. 函数必须有`返回值`
  3. 函数如果返回`null`则代表什么都不渲染
```jsx
function MyComponent() {
  return (<h1>这是我用函数创建的React组件!</h1>)
}
```
#### 使用ES6中的class创建组件
  规定:
  1. 类名首字母`必须大写`
  2. 类必须继承自React.Component
  3. 必须有render函数且有返回值
```jsx
class MyApp extends React.Component {
  render() {
    return (<h1>您好, 这是一个ES6的类组件<h1>)
  }
}
ReactDOM.render(<MyApp/>, root)
```
### React中的事件绑定
+ 语法与DOM事件相似
+ 语法: on + 事件名称 = {事件处理函数}, 例如: onClick={() => {}}
> 注意: React事件采用驼峰命名法, 比如: onMouseEnter, onFocus
1. 在函数组件中绑定事件


```jsx
function MyApp() {
  function handler() {
    console.log('您点击了一下 button 按钮!')
  }
  return (<button onClick={handler}>点击测试</button>)
}
```
2. 在类组件中绑定事件
```jsx
class MyApp extends React.Component {
  handler() {
    alert('在类组件中点击了一下 button 按钮!')
  }
  render() {
    return (<button onClick={this.handler}>点击测试</button>)
  }
}
```
  > 事件对象
  1. 可以通过事件处理程序的参数获取到事件对象
  2. React中的事件对象叫做: `合成事件(对象)`
  3. 合成事件: 兼容所有的浏览器,无需担心跨浏览器的兼容性
```jsx
const MyApp = () => {
  const handler = (ev) => {
    ev.preventDefault()
    console.log('a标签的页面跳转行为被阻止了@_@', ev)
  }
  return (<a onClick={handler} href="https://www.sina.com/">点击跳转到--新浪首页</a>)
}
```
### 有状态组件和无状态组件
> 状态(state)即数据,状态是私有的,只能在组件内部使用(暂不考虑组件间通讯)
+ 函数组件: 无状态组件,只负责数据展示(静态)
  类组件: 有状态组件,负责更新UI,让页面"动"起来(典型案例: 计数器)
#### state(状态)的基本使用
```jsx
class MyApp extends React.Component {
  state = { count: 0 }
  render() {
    return (<h1>有状态的类组件: {this.state.count}</h1>)
  }
}
```
+ setState()修改状态
  语法: this.setState({key: value})
  > 不能直接修改state中的值,不符合语法规范
  setState作用: 
    1. 修改state 
    2. 更新UI
  思想: 数据驱动视图

#### 事件绑定中的 this 指向
1. 箭头函数
+ 利用箭头函数自身没有this(用到this立即向外部寻找)
  
  > 为保证JSX结构清晰,开发中一般将JS逻辑抽离到单独的方法中,但抽离时应注意this指向
```jsx
class MyApp extends React.Component {
  state = { count: 0 }
  render() {
    return [
      <h1 key={1}>有状态的类组件: --- 计数器: {this.state.count}</h1>,
      <button key={2} onClick={() => {this.setState({count: this.state.count + 5})}>点击计数器 +5</button>
    ]
  }
}
```
*逻辑抽离出来成单独的方法:*
> 此处可以将class的实例方法写成箭头函数,不过值得注意:该语法是实验性语法,但是由于React脚手架中的 `babel` 的存在,故可以直接使用,也是推荐的一种写法
```jsx
class EScomponent extends React.Component {
  handler(e) {
    console.log('阻止了a标签跳转到 新浪网');
    e.preventDefault()
  }
  handlerClick = () => {
    this.setState({
      count: this.state.count + 10
    })
  }
  state = { count: 100 }
  render() {
    return [
      <h2 key={Math.random().toString().slice(2,7)}>这是一个ES6语法创建的组件! ----计数器: {this.state.count}</h2>,
      <a href="https://www.sina.com.cn/" onClick={this.handler} key={Math.random().toString().slice(2,7)}>点击跳转至新浪网</a>,
      <button onClick={this.handlerClick} key={Math.random().toString().slice(2,7)}>点击计数器 +10</button>
    ]
  }
}
```
2. 利用ES5中的bind方法改变this指向
```jsx
class EScomponent extends React.Component {
  constructor() {
    super()
    this.handlerClick = this.handlerClick.bind(this)
  }
  handlerClick() {
    this.setState({
      count: this.state.count + 10
    })
  }
  state = { count: 100 }
  render() {
    return [
      <h2 key={Math.random().toString().slice(2,7)}>这是一个ES6语法创建的组件! ----计数器: {this.state.count}</h2>,
      <button onClick={this.handlerClick} key={Math.random().toString().slice(2,7)}>点击计数器 +10</button>
    ]
  }
}
```
### 表单的处理
+ 受控组件
+ 非受控组件(DOM方式)
#### 受控组件
+ React将state与表单元素值value绑定到一起,有state的值来控制表单元素的值
+ 受控组件: 其值受到React控制的表单元素

步骤:
1. 在state中添加一个状态,作为表单元素的value值(控制表单元素值的来源)
2. 给表单元素绑定change事件,将表单元素的值设置为state的值(控制表单元素值的变化)
> 多表单元素的优化步骤:
1. 给表单元素添加name属性,名称与state相同
2. 根据表单元素类型获取对应值
3. 在change事件处理程序中通过[name]来修改对应的state
```jsx
class EScomponent extends React.Component {
  handler(e) {
    console.log('阻止了a标签跳转到 新浪网');
    e.preventDefault()
  }
  handlerClick = () => {
    this.setState({
      count: this.state.count + 10
    })
  }
  handlerChange = ev => {
    //获取当前事件对象
    const target = ev.target
    //根据类型获取值
    const value = target.type === "checkbox" ? target.checked : target.value
    //获取name属性值
    const name = target.name
    //修改数据
    this.setState({
      [name]: value
    })
  }
  state = { 
    count: 100,
    txt: '',
    content: '',
    city: 'SH',
    isChecked: false
  }
  render() {
    return [
      <h2 key={Math.random().toString().slice(2,7)}>这是一个ES6语法创建的组件! ----计数器: {this.state.count}</h2>,
      <a href="https://www.sina.com.cn/" onClick={this.handler} key={Math.random().toString().slice(2,7)}>点击跳转至新浪网</a>,
      <button onClick={this.handlerClick} key={Math.random().toString().slice(2,7)}>点击计数器 +10</button>,
      <hr key={123}/>,
      <div key={234} className='form-wrap'>
        {/* 文本输入框 */}
        <input type="text" name="txt" value={this.state.txt} onChange={this.handlerChange}/>
        {/* 富文本框 */}
        <textarea name='content' value={this.state.content} onChange={this.handlerChange}></textarea>
        {/* 下拉框 */}
        <select name='city' value={this.state.city} onChange={this.handlerChange}>
          <option value="SH">上海</option>
          <option value="BJ">北京</option>
          <option value="GZ">广州</option>
          <option value="SZ">深圳</option>
        </select>
        {/* 复选框 */}
        <input name='isChecked' type="checkbox" checked={this.state.isChecked} onChange={this.handlerChange}/>
      </div>
    ]
  }
}
```
#### 非受控组件
`[参考文档]`: https://react.docschina.org/docs/uncontrolled-components.html#the-file-input-tag "非受控组件"

在 HTML 中，`<input type="file">` 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 File API 进行控制。
因为它的 value 只读，所以它是 React 中的一个非受控组件。
**使用步骤:**
1. 调用React.createRef()方法创建一个ref对象
```jsx
constructor() {
  super()
  this.txtRef = React.createRef()
}
```
2. 将创建好的ref对象添加到文本框中
```jsx
<input type="text" ref={this.txtRef} />
```
3. 通过ref对象获取到文本框的值
```jsx
console.log(this.txtRef.current.value)
```

> 有时使用受控组件会很麻烦，因为你需要为数据变化的每种方式都编写事件处理函数，并通过一个 React 组件传递所有的输入 state。当你将之前的代码库转换为 React 或将 React 应用程序与非 React 库集成时，这可能会令人厌烦。在这些情况下，你可能希望使用非受控组件, 这是实现输入表单的另一种方式。

> 在大多数情况下，我们推荐使用 受控组件 来处理表单数据。在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。

> 要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 使用 ref 来从 DOM 节点中获取表单数据。