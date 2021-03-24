---
title: react-redux
date: 2020-03-05 13:07:17
tags:
- w3HexSchool
- react
---

# react-redux
在上一篇redux的文章中，有大致上說明了redux的三個基礎Action、Reducer和Store。這篇文章將說明react如何連結redux。

---

## connect
在上一篇文章中有提到要觸發action，可以透過dispatch來觸發。你要在react用redux提供的dispatch來觸發也沒有問題，但在react-redux中，提供了connect()幫你解決問題。

connect()是一個High Order Component，我們傳入一個component，在經過一些修飾加工，會產生新的component給我們。
詳細HOC的功能這邊不多做說明。

一般我們connect會這樣寫
```JavaScript
export default connect(mapStateToProps, mapDispatchToProps)(Test)
```
connect後面的括弧會放mapStateToProps和mapDispatchToProps。

前者就是提供store的資料透過props傳給component，後者則是把dispatch功能提供給component。


最後面Test則是代表我們要傳入的component。

---
### mapStateToProps
```JavaScript
const mapStateToProps = (state) => ({
  center: state.center,
  zoom: state.zoom,
  data: state.data,
});
```
mapStateToProps裡面可以指定這個component要提供儲存在store的哪些資料。

---
### mapDispatchToProps
```JavaScript
const mapDispatchToProps = dispatch => {
  return {
    // dispatching plain actions
    increment: () => dispatch({ type: 'INCREMENT' }),
    decrement: () => dispatch({ type: 'DECREMENT' }),
    reset: () => dispatch({ type: 'RESET' })
  }
}
```
mapDispatchToProps 基本上寫法也一樣，return一個object說明，每個key是要做什麼action。

上述的mapDispatchToProps是最基本寫法，如果你的mapDispatchToProps裡面有帶參數的，可以參考[官方說明](https://react-redux.js.org/using-react-redux/connect-mapdispatch)。

---
### Provider
Provider也是react-redux內提供的，我們會把他寫在最外層，也就是你要與HTML介接的內個JS中。
```JavaScript
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root'),
);
```
Provider的功用就是要內下層的component可以透過connect取得redux store的資料。

---

## 補充
在mapDispatchToProps中，如果沒有要做什麼重新命名或其他行為時，我一般會偷懶直接使用redux action的那個function名稱。

```JavaScript
export default connect(
  mapStateToProps,
  { getTestData, getCoordinates, getNowSelectData },
)(Test);
```
不過如果你有使用eslint的Airbnb code style，他會要求你寫成mapDispatchToProps就是了。

而假使你這個component中，並沒有要使用到redux store的資料或dispatch行為，可以將內個參數改為null。

```JavaScript
export default connect(
  mapStateToProps,
  null,
)(Test);
```






