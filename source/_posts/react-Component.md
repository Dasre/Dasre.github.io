---
title: react-Component
date: 2020-01-30 22:39:13
tags: "react"
---

# Component
在react的component中，大致上可以分為Class Component、Functional Component兩種。 

* Class Component是使用ES6 Class的寫法宣告他，同時也可以使用state和自定義的method。
* Functional Component則是使用function的方式宣告他，但他沒有state且無法自定義method，也就是只有render的狀況使用。而其props則是透過函數傳遞進來。

```javascript
//Class Component
class App extends Component{
    state = {
        width: 0,
    }

    addWidth = () => {
        ...
    }

    render(){
        const { width } = this.props;
        return(
            <div>
                <div style={{width: `${width}`}}>
            </div>
        )
    }
}
```

```javascript
//Functional Component
const App = (props) => {
    render(){
        const { width } = this.props;
        return(
            <div>
                <div style={{width: `${width}`}}>
            </div>
        )
    }
}
```

另外還有一種叫做PureComponent，其寫法與Class Component相似，只要把extends後面改成PureComponent即可。
```javascript
//PureComponent
class App extends PureComponent{
    state = {
        width: 0,
    }

    addWidth = () => {
        ...
    }

    render(){
        const { width } = this.props;
        return(
            <div>
                <div style={{width: `${width}`}}>
            </div>
        )
    }
}
```
而PureComponent和Class Component主要是在效能上會有差異。

## 簡單來說如果傳入的props值不變或state值不變，PureComponent就不會重新render。但在Class和Functional Component都會重新render。

至於props和state是否改變，是透過shallow compare。
他只會比較props和state的第一層。

```javascript
class App extends Component{

    //state裡的width和border是第一層，color是第二層。
    state = {
        width: 0,
        border: {
            borderWidth: 1,
        }
    }

    //此add function在Class Component會重新render，在PureComponent則不會
    const add = () => {
        const {border} = this.state;
        border.borderWidth += 1;
        this.setState({
            border: border,
        })
    }
    render(){
        const { width } = this.props;
        return(
            <div>
                <div style={{width: `${width}`}}>
            </div>
        )
    }
}
```

## 小結
* 若Component內不會使用到state和自定義method，可改用Functional Component。
* PureComponet可以解決重複render之問題，但前提要確認state或props的值改變只會在第一層。



