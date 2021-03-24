---
title: redux
date: 2020-02-24 09:50:51
tags:
- w3HexSchool
---

# redux
redux這個名詞，相信有接觸過react的人應該都會聽過。而在網路上，你也可以收尋到許多redux的詳細教學文章，不管是英文的、中文的都有。之所以想寫這篇文章，主要是覺得官方的文件並不能讓人容易將觀念串連起來，而中文的教學雖然也有，但一開始就看到那麼詳細的內容，有時候會越看越模糊。因此想要嘗試用簡單的話語從官方的範例中進行說明，也強化自己的觀念。

在這篇文章中，不會去介紹redux的歷史、以往的技術等等。我們會從直接從react-redux官方[範例](https://codesandbox.io/s/9on71rvnyo)來進行說明。有些在導入react-redux後不會使用的內容還是會提出來。

---

## redux 三個基礎

### <font color=red>Action</font>
主要是用來說明要對目前的資料進行什麼行為。

參考範例 /src/redux/actions.js
``` JavaScript
export const addTodo = content => ({
  type: ADD_TODO,
  payload: {
    id: ++nextTodoId,
    content
  }
});
```
這邊抓範例裡面的addTodo來說明。

action又可以分為三個部分
* Action 指的就是
``` JavaScript
{
  type: ADD_TODO,
  payload: {
    id: ++nextTodoId,
    content
  }
}
```
這段，type描述要進行什麼行為，payload則是要帶什麼參數去後面更改資料使用。

* Action Creator簡單來說就是建立Action的function
```JavaScript
export const addTodo = content => ({
  type: ADD_TODO,
  payload: {
    id: ++nextTodoId,
    content
  }
});
```
上面整段就是一個Action Creator

* store.dispatch() 觸發一個定義好的行為。但如果是在react中使用redux，通常我們會採用react-redux提供的connect()。
參考範例 /src/components/AddTodo.js
```JavaScript
export default connect(
  null,
  { addTodo }
)(AddTodo);
```
關於這部分會等到導入react-redux再進行說明。

### <font color=red>Reducer</font>
簡單來說就是更新state的地方，會將從action的payload傳進來的參數和目前的state產生一新state。

參考範例 /src/redux/reducers/todo.js
```JavaScript
const initialState = {
  allIds: [],
  byIds: {}
};

export default function(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO: {
      const { id, content } = action.payload;
      return {
        ...state,
        allIds: [...state.allIds, id],
        byIds: {
          ...state.byIds,
          [id]: {
            content,
            completed: false
          }
        }
      };
    }
    case TOGGLE_TODO: {
      const { id } = action.payload;
      return {
        ...state,
        byIds: {
          ...state.byIds,
          [id]: {
            ...state.byIds[id],
            completed: !state.byIds[id].completed
          }
        }
      };
    }
    default:
      return state;
  }
}
```
initialState初始化state資料。下方透過switch case偵測action type來決定要將state進行怎樣的更新，最後再回傳新state。如果state狀態都沒有要更動，就回傳舊的state。

Reducer幾個重點
* 永遠不要更改到舊的state資料，而是要產生新的state。所以這邊有用到ES6的object spread。
* reducer是一個pure function，任何的非同步、call api等行為都不應該在此出現，任何有side effect的行為也是。

### <font color=red>Store</font>
串連Action和Reducer使用。

參考範例 /src/redux/store.js
```JavaScript
import { createStore } from "redux";
import rootReducer from "./reducers";

export default createStore(rootReducer);
```

* Redux只會有一個Store。
* getState()取得state資料。
* dispatch(action)觸發action事件，更新state。
* subscribe(listener)監聽事件。

---

## 資料流走向
觸發action -> store找到createStore所帶入的reducers，並傳入action和舊的state -> 產生新的state或回傳舊的state，store會再整合成一新單一state。

---

## 補充-reducers合併

我們在store部分有提到，redux只會有一個store。但假如我們的reducers拆分成多個檔案時，也就是每個reducers只管跟他有關的東西，我們就可以透過redux的combineReducers將他合併起來，並在建立store時，引入合併好的那個reducers即可。

參考範例 /src/redux/reducers/index.js
``` JavaScript
import { combineReducers } from "redux";
import visibilityFilter from "./visibilityFilter";
import todos from "./todos";

export default combineReducers({ todos, visibilityFilter });
```
關於combineReducers時，也可以給他不同key值，例如寫成
```JavaScript
{
  a: todos,
  b: visibilityFilter
}
```
