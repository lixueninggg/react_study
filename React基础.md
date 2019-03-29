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

条件渲染

> 写页面的时候少不了条件渲染和循环渲染

```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

循环渲染

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