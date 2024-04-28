# React

### 一、定义

一个用于构建Web和原生交互界面的库

相较于传统基于DOM开发的优势：组件化的开发方式、不错的性能

相较于其它前端框架的优势：丰富的生态、跨平台支持

### 二、开发环境搭建

```react
npx create-react-app react_basic
//react-basic React项目的名称（可以自定义）
cd react_basic
npm start
```

### 三、JSX基础

#### 1、概念和本质

​         **JSX是JavaScript和XML（HTML）的缩写，表示在JS代码中编写HTML模板结构，它是React中编写UI模板的方式**。JSX并不是标准的JS语法，它是JS的语法扩展，浏览器本身不能识别，需要通过解析工具（可以用Babel）做解析之后才能在浏览器中运行

```js
const message="this is message"
function App() {
  return (
    <div>
      <h1>this is title</h1>
      {message}
    </div>
  );
}
```

优势：HTML的声明式模板写法、JS的可编程能力

#### 2、使用场景

JSX中使用JS表达式:

在JSX中通过{}识别JS中的表达式，if语句、switch语句、变量声明属于语句，不是表达式，不能出现在{}中

（1）使用引号传递字符串

（2）使用JS变量

（3）函数调用和方法调用

（4）使用JS对象

```javascript
const count=10;
function getName(){
  return 'Jack'
}
function App() {
  return (
    <div className="App">
      this is App
      {/*使用引号传递字符串 */}
      {'this is message'}
      {/*使用JS变量 */}
      {count}
      {/*函数调用 */}
      {getName()}
      {/*方法调用 */}
      {new Data().getData()}
      {/*使用JS */对象}
      <div style={{color:'red'}}>this is div</div>
    </div>
  )
}
```

JSX中实现列表渲染，在JSX中可以使用JS中的map方法遍历渲染列表

```javascript
const list=[
  {id:1001,name:'Vue'},
  {id:1002,name:'React'},
  {id:1003,name:'Angular'}
]

function App() {
  return (
    <div className="App">
      this is App
      {/*渲染列表 */}
	  {/*map循环哪个结构 return哪个结构 */}
      {/*记住要加上一个独一无二的key 可以说字符串或者Number id，key的作用：它是react框架内部使用的，用来提升更新性能的 */}
      <ul>
        {list.map(item=><li key={item.id}>{item.name}</li>)}
      </ul>
    </div>
  );
}
```

JSX中实现条件渲染

在React中可以通过逻辑与运算符&&、三元表达式 (?:)实现基础的条件渲染

```javascript
const isLogin=true;

function App() {
  return (
    <div className="App">
      this is App
      {/*逻辑与&& */}
      {isLogin && <span>this is span</span>}
      {/*三元运算 */}
      {isLogin ? <span>this is span</span> : <span>loading...</span>}
    </div>
  );
}
```

JSX中实现复杂条件渲染

需求：列表中需要根据文章状态适配三种情况，单图、三图和无图三种模式

解决方案：自定义函数+if判断语句

```javascript
//定义文章类型
const articleType=3;//0 1 3

//定义核心函数
 function getArticleTem(){
  if (articleType==0)
     return <div>这是无图文章</div>;
  else if(articleType==1)
        return <div>这是单图文章</div>;
      else
        return <div>这是三图文章</div>;
 }


function App() {
  return (
    <div className="App">
      this is App
      {/*调用函数渲染不同的模板 */}
      {getArticleTem()}
    </div>
  );
}
```

### 四、应用

#### 1、React基础事件绑定

语法：on+事件名称={事件处理程序}

```javascript
function App() {
  const handleClick=()=>{
    console.log('button被点击了');
  }
  return (
    <div className="App">
      <button onClick={handleClick}>click me</button>
    </div>
  );
}
```

**使用事件对象参数**

语法：在事件回调函数中设置形参e

```javascript
function App() {
  const handleClick=(e)=>{
    console.log('button被点击了',e);
  }
  return (
    <div className="App">
      <button onClick={handleClick}>click me</button>
    </div>
  );
}
```

**传递自定义参数**

语法：事件绑定的位置改造成箭头函数的写法，在执行handleclick实际处理业务函数的时候传递实参

注意：不能直接写函数调用，这里事件绑定需要一个函数引用

```javascript
function App() {
  const handleClick=(name)=>{
    console.log('button被点击了',name);
  }
  return (
    <div className="App">
      <button onClick={()=>handleClick('Jack')}>click me</button>
    </div>
  );
}
```

**同时传递事件对象和自定义参数**

语法：在事件绑定的位置传递事件实参e和自定义参数，handleclick中声明形参，注意顺序对应

```javascript
function App() {
  const handleClick=(name,e)=>{
    console.log('button被点击了',name,e);
  }
  return (
    <div className="App">
      <button onClick={(e)=>handleClick('Jack',e)}>click me</button>
    </div>
  );
}
```

#### 2、React中的组件

**插入图片笔记**

在React中，一个组件就是首字母大写的函数，内部存放了组件的逻辑和视图UI，渲染组件只需要把组件当成标签书写即可

```javascript
//1、定义组件
function Button(){
  //业务组件逻辑
  return <button>click me!</button>
}

function App(){
  return (
    <div className="App">
      {/*2、使用组件*/}
      {/*自闭和 */}
      <Button/>
      {/*成对标签 */}
      <Button></Button>
    </div>
  );
}
```

#### 3、useState基础使用

useState是一个React Hook(函数)，它允许我们向组件添加一个状态变量，从而控制影响组件的渲染结果

```javascript
//useState实现一个计数器按钮
import {useState} from 'react'
function App(){
  //1.调用useState添加一个状态变量
  //count是状态变量
  //setCount是修改变量的方法
  const [count,setCount]=useState(0)
  //2.点击事件回调
  const handleClick=()=>{
    //作用：1.用传入的新值修改count 2.重新使用新的count渲染UI
    setCount(count+1)
  }
  return (
    <div>
     <button onClick={handleClick}>{count}</button>
    </div>
  );
}
```

useState修改状态的规则

状态不可变：在React中，状态被认为是只读的，我们应该始终替换它而不是修改它，直接修改状态不能引发视图更新

修改对象状态的规则：对于对象类型的状态变量，应该始终给set方法一个全新的对象来进行修改

```javascript

import {useState} from 'react'
function App(){
  const [form,setForm]=useState({name:'Jack'})
  const changeForm=()=>{
    //错误写法：直接修改
    //form.name='John'
    //正确写法:调用setForm方法，传入一个全新的对象用作替换
    setForm({
      ...form,
      name:'John'
    })
  }
  return (
    <div>
     <button onClick={changeForm}>修改form {form.name}</button>
    </div>
  );
}
```

#### 4、组件基础样式方案

React组件基础的样式控制有两种方式

（1）行内样式（不推荐）

```javascript
//方式一
function App(){
  return (
    <div>
    <span style={{color:'red',fontSize:'50px'}}>this is span</span>
    </div>
  )
}

//方式二
const style={
    color:'red'
    fontSize:'50px'
}
function App(){
  return (
    <div>
    <span style={style}>this is span</span>
    </div>
  )
}
```

（2）class类名控制

```javascript
//index.css
.foo{
    color:blue
}

//导入样式到App.js
import './index.css'

function App(){
  return (
    <div>
      <span className="foo">this is calss foo</span>
    </div>
  )
}
```

