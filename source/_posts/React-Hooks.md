---
title: React-Hooks 文档笔记
date: 2020-05-19 15:02:08
tags: -JavaScrip  -React
---
{% blockquote %}
本文参考于React官方文档，仅供学习使用～
{% endblockquote %}

## Hooks
<font color="#dd0000">特征: 1.完全可选。 2.100% 向后兼容的。  3.现在即可使用。</font>
Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数。Hook 不能在 class 组件中使用，所以不推荐把你已有的组件全部重写，但是你可以立即在新组件里直接开始使用 Hook。

## Hook 使用规则
Hook 就是 JavaScript 函数，但是使用它们会有两个额外的规则：
 1. <font color="#dd0000">只能在函数最外层调用 Hook。</font>不要在循环、条件判断或者子函数中调用。
 2. <font color="#dd0000">只能在 React 的函数组件中调用 Hook，</font>不要在其他 JavaScript 函数中调用。
注：<a href='https://www.npmjs.com/package/eslint-plugin-react-hooks'>eslint-plugin-react-hooks</a>插件可自动执行这些规则。

## 1.useState
useState 是一个 Hook, 为通过在函数组件里调用它来给组件添加一些内部 state。React 会在重复渲染时保留这个 state。
useState 会返回一对值：当前状态和一个让你更新它的函数。它类似 class 组件的 this.setState，但是<font color="#dd0000">它不会把新的 state 和旧的 state 进行合并</font>。
```
import React, { useState } from 'react';  // 引入 React 中的 useState Hook。它让我们在函数组件中存储内部 state。

// 可以在一个组件中多次使用 useState
const [count, setCount] = useState(0);  // 声明state变量
const [fruit, setFruit] = useState('banana');
const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

<p>You clicked {count} times</p>  // 读取 State
<button onClick={() => setCount(count + 1)}>  // 更新 State
    Click me
</button>
```
值得注意的是，不同于 this.state，这里的 state 不一定要是一个对象(可以是别的类型)。
<li><font color="#dd0000">调用 useState 方法的时候做了什么? </font>它定义一个 “state 变量”。变量名由可以任意自定义，这是一种在函数调用时保存变量的方式 —— useState 是一种新方法，它与 class 里面的 this.state 提供的功能完全相同。一般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留。</li>
<li><font color="#dd0000">useState 需要哪些参数？ </font>useState 唯一的参数就是初始 state。这个初始 state 参数只有在<font color="#dd0000">第一次渲染时</font>会被用到。</li>
<li><font color="#dd0000">useState 方法的返回值是什么？</font>返回值为：当前 state 以及更新 state 的函数。这与 class 里面 this.state.count 和 this.setState 类似，唯一区别就是你需要成对的获取它们。</li>
<li><font color="#dd0000">方括号有什么用？</font>这种 JavaScript 语法叫数组解构。当我们使用 useState 定义 state 变量的时候，它返回一个有两个值的数组。第一个值是当前的 state，第二个值是更新 state 的函数。使用 [0] 和 [1] 来访问有点令人困惑，因为它们有特定的含义。所以使用了数组解构。</li>

## 2.useEffect
<font color="#dd0000">副作用</font>：在 React 组件中执行过数据获取、订阅或者手动修改过 DOM等。
而 useEffect 就是为函数组件增加了操作 副作用 的能力。它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有<font color="#dd0000">相同</font>的用途，只不过被合并成了一个 API。
当你调用 useEffect 时，就是在告诉 React 在完成对 DOM 的更改后运行你的“副作用”函数。由于副作用函数是在组件内声明的，所以它们可以访问到组件的 props 和 state。
默认情况下，React 会在<font color="#dd0000">每次渲染后调用副作用函数 (包括第一次渲染的时候)。</font>
```
// 可以在一个组件中多次使用 useEffect

function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);

  // 副作用函数还可以通过返回一个函数来指定如何“清除”副作用
  // 这里，React 会在组件销毁时取消对 ChatAPI 的订阅，然后在后续渲染时重新执行副作用函数
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
```