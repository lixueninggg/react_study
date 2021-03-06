# react基础篇

> 在React基础篇中我会结合官网中的示例记录自己的学习过程,这个过程中尽可能把自己的问题也记录下来,同时尽可能从自己的角度的总结一下React的知识网络

### 基础语法

#### JSX语法
1. 简单的声明一个变量

```javascript
const element = <h1>Hello, world!</h1>;
```

2. JSX中使用表达式

> 在 JSX 当中的表达式要包含在大括号里 (ps: 刚开始react写起来可能真的没有vue那么舒服,可能刚接触起来也需要一些时间适应)
```javascript
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

当我看到官网此示例中有几个问题

Q1: 为什么 document.getElementById('root')

A1: react和vue都会将DOM渲染到一个根节点下,在官网的后续介绍中也会提到

3. 判断和循环显示

- 条件渲染

> 写页面的时候少不了条件渲染和循环渲染

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

- 循环渲染

> 循环渲染在官网事例中并没有很好的demo, 于是就google了一个比较好的例子

```javascript
const users = [
  { username: 'Jerry', age: 21, gender: 'male' },
  { username: 'Tomy', age: 22, gender: 'male' },
  { username: 'Lily', age: 19, gender: 'female' },
  { username: 'Lucy', age: 20, gender: 'female' }
]

class User extends Component {
  render () {
    const { user } = this.props
    return (
      <div>
        <div>姓名：{user.username}</div>
        <div>年龄：{user.age}</div>
        <div>性别：{user.gender}</div>
        <hr />
      </div>
    )
  }
}

class Index extends Component {
  render () {
    return (
      <div>
        {users.map((user, i) => <User key={i} user={user} />)}
      </div>
    )
  }
}

ReactDOM.render(
  <Index />,
  document.getElementById('root')
)
```
注: react和vue的循环渲然都需要key来作为唯一标识去更快的寻找到对应dom

此DEMO摘自 http://huziketang.mangojuice.top/books/react/lesson13

在react中循环渲然使用es6的map方法去生成对应的dom

> class 属性在react中变为 className
> React.createElement() 为react语法糖

### react 属性

#### Props

> 个人理解: 向组件传递的属性值(和vue的props相似)

```javascript
// 父组件
<PropComponent name="姓名" />
// 子组件
Class PropComponent extends React.Component {
  render () {
    const { name } = this.props
    <div>
      姓名: {name}
    </div>
  }
}

export default PropComponent
```

> props函数式写法

```javascript
// 父组件
<PropComponent name="姓名" />
// 子组件
const PropComponent = (props) => {
  render () {
    const { name } = props
    <div>
      姓名: {name}
    </div>
  }
}

export default PropComponent
```
> props属性是只读的,无法直接更改如果改变需要通过state来改变

#### State

> state可以使组件内部数据改变 <br />
this.setState() 是更新state的唯一途径

```javascript
// 父组件
<StateComponent />
// 子组件
Class StateComponent extends react.componexts {
  constructor (prop)  {
    super(prop)
    this.state = {
      likes: 0
    }
    // 注: 在onClick绑定事件中如果未使用箭头函数需要修正指针
    // this.clickFn = this.clickFn.bind(this)
  }
  clickFn () {
    this.setState({
      likes: ++this.likes
    })
  }
  render () {
    <div>
      <button type="button" onClick={ () => {this.clickFn()} }>
      </button>
    </div>
  }
}
```

### 生命周期

> 组件初始化 <br /> 组件更新 <br /> 组件卸载

```
创建时 -> constructor(构造函数|构造器) -> render -> react更新DOM和refs -> componentDidMount

更新时 -> New props 和 set State() 和 force Update() -> react更新DOM和refs -> componentDidUpdate

卸载时 -> componentWillUnmount
```

```javascript
// 父组件
// 子组件
class DigitalClock extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      date: new Date()
    }
  }
  componentDidMount() {
    this.timer = steInterval(() => {
      this.setState({
        date: new Date()
      })
    }, 1000)
  }
  componentWillUnmount() {
    clearInterval(this.timer)
  }
  componentDidUpdate(currentProps, currentState) {
    // 检测更新
    console.log(currentState)
  }
  render() {
    <div>
      {this.state.date.tolocaleTimeString()}
    </div>
  }
}

export default DigitalClock
```


### 表单forms

> forms(表单) <br /> 表单元素和其他DOM的区别 <br /> Controlled Components - 受控组件

```javascript
// 受控组件
class CommentBox extends React.Component{
  constructor(props) {
    super(props)
    this.state = {
      value: ''
    }
    this.handleChange = this.handleChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }
  handleChange (event) {
    this.setState({
      value: event.target.value
    })
  }
  handleSubmit(e) {
    alert(this.state.value)
    e.preventDefault()
  }
  render() {
    return(
      <form onSubmit={this.handleSubmit}>
        <input type='text' onChange={this.handleChange} placeholeder='请输入内容' value={this.state.value}>
        <button type='submit'>按钮</button>
      </form>
    )
  }
}
export default CommentBox
```

```javascript
// 非受控组件
class CommentBox extends React.Component{
  constructor(props) {
    super(props)
    this.handleSubmit = this.handleSubmit.bind(this)
  }
  handleSubmit(e) {
    alert(this.textInput.value)
    e.preventDefault()
  }
  render() {
    return(
      <form onSubmit={this.handleSubmit}>
        <input type='text' placeholeder='请输入内容' ref={(textInput) = > { this.textInput = textInput }}>
        <button type='submit'>按钮</button>
      </form>
    )
  }
}
export default CommentBox
```

### Context

> Context提供了数组中共享此类值的方法 <br /> 共享全局组件来说的全局数据

```javascript
// DEMO主题切换
// 父组件
// import  ThemeContext from './theme-context'
// 引入对应路径的Context
// import  ThemedBar from './ThemedBar'
// 引入对应路径的ThemedBar
const themes = {
  light: {
    classnames: 'btn btn-primary',
    bgColor: '#eee',
    classnames: '#000',
  },
  dark: {
    classnames: 'btn btn-light',
    bgColor: '#222',
    classnames: '#fff',
  }
}

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      theme: 'light'
    }
  }
  changeTheme(theme) {
    this.setState({
      theme
    })
  }
  rander() {
    return (
      <ThemeContext.Provider value={themes.[this.state.theme]}>
      <div className="App">
        <header>
          <h1>主题</h1>
          <a href="#theme-switcher"
             onClick = { () => {this.changeTheme('light')} } className="btn btn-light">浅色主题</a>
          <a href="#theme-switcher"
             onClick = { () => {this.changeTheme('dark')} } className="btn btn-light">深色主题</a>
        </header>
        <ThemedBar />
      </div>
      </ThemeContext.Provider>
    )
  }
}
// context.js
import React from 'react'
const ThemeContext = React.createContext()
export default ThemeContext
// ThemedBar.js
import React from 'react'
import ThemeContext from './theme-context'
const ThemedBar = () => {
  return {
    <ThemeContext.Consumer>
      {
        theme => {
          console.log(them)
          return (
            <div
             className="alter mt-5"
             style={ {backgeound: theme.bgColor, color: theme.color} }
            >
              样式区域
              <button className={theme.classnames}>
                样式按钮
              </button>
            </div>
          )
        }
      }
    </ThemeContext.Consumer>
  }
}
export default ThemedBar
```