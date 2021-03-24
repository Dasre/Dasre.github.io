---
title: react-Hooks
date: 2020-01-31 22:56:02
tags: "react"
---

# Hooks-useState

Functional Component上，我們都知道無法使用state和自定意函式，勢必要改成Class Component。而到了React 16.8版後，推出了Hooks，讓我們不必寫Class Component就可以使用state。這次先說明Hooks裡面的useState。

下方會以計數器來進行說明
```javascript
//以Class Component攥寫
import React, { Component } from "react";

class App extends Component {
  state = {
    count: 0
  };

  addCount = () => {
    this.setState({
      count: this.state.count + 1
    });
  };

  render() {
    const { count } = this.state;
    return (
      <div>
        <h1>{count}</h1>
        <button onClick={this.addCount}>ADD</button>
      </div>
    );
  }
}
module.exports = App;
```
上方code是傳統上以Class Component進行攥寫的。
下方code為改成Hooks的做法。
```javascript
//以Functional Component攥寫
import React, { useState } from "react";

const App = () => {
  const [count, countFun] = useState(0);
  const addCount = () => {
    countFun(c => {
      return c + 1;
    });
  };

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={addCount}>ADD</button>
    </div>
  );
};
module.exports = App;
```

<hr>

## 說明

``` javascript
const [count, countFun] = useState(0);
```
呼較useState並且給予初始的狀態（這邊的初始狀態是0），會給予一個array。array裡面的第一個值就是初始狀態（狀態並不一定是要object），第二個值是狀態的函式。

我們的目標是做一個計數器，所以countFun這個函式我們會設定成count+1。
``` javascript
const addCount = () => {
    countFun(c => {
      return c + 1;
    });
};

return (
    <div>
      <h1>{count}</h1>
      <button onClick={addCount}>ADD</button>
    </div>
);
```
所以當button觸發onClick這個addCount的函示，會去觸發countFun這個函式。countFun可以直接指定數值等等（ex: countFun(count+1)），同樣的也可以給予一個函式，我們這邊將這個函式定義為count+1(這邊用c代表count，表示舊的state，新的state是c+1)。

* useState不可以放在任何條件式裡面

<hr>

## 補充

```javascript
const [{num1, num2}, numFun] = useState({num1: 0, num2: 1});

//正確
const addNum1 = () => {
    numFun((state) => ({...state, num1: state.num1 + 1}))
}
//錯誤
const addNum1 = () => {
    numFun((state) => ({num1: state.num1 + 1}))
}
```

在以前處理state的值時，我們每次更新不一定需要將每個state的狀態寫出來，class會自動幫我們把缺少的state補齊。但在useState裡，則是要自己組合。