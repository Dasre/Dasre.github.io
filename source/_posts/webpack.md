---
title: webpack安裝與簡易配置(1)
date: 2020-02-16 18:40:32
tags:
- w3HexSchool
---

# webpack
以往我們在開發網頁時，基本上就是HTML、CSS和JavaScript三元素。然而在許多的Preprocess框架出現後，開始出現改變。這些的preprocess框架如Sass、Vue等等都是，以Sass為例，他可以有效的幫助我們維護CSS，但瀏覽器本身並不了解Sass，這時候我們就可以透過webpack幫我們把Sass轉譯成CSS。或是說目前常用的ES6語法，也可以透過webpack幫助我們轉譯成browser看得懂的語法。

---

## webpack安裝
要使用webpack之前，請先在專案中產生package.json檔案，可以透過
```
npm init
```
產生。這邊是使用之前介紹過的express generator產生專案。

接下來就可以安裝webpack了。

```
npm install webpack webpack-cli --save-dev
```
此時，package.json內應該就會有剛剛安裝的webpack版本資訊。
![](/webpack/1.png)

接下來我們就是要設定webpack的設定檔。

---

## webpack設定

通常一般會在與package.json同一層的目錄下建立一webpack.config.js檔案。

```javascript
const path = require('path');

module.exports = {
  entry: {
    'main': './public/javascripts/index.js',
  },
  output: {
    path: path.resolve(__dirname, 'public'),
    filename: '[name].bundle.js',
  },
  module: {

  },
  plugins: []
}
```
webpack的設定檔中，大致上有4個屬性可以使用。
其中entry和output是一定要填寫的。
* entry裡面就是放我們要讀的檔案，如果要讀多個檔案，可以使用object的key-value方式來填寫。這邊的檔案可以使用相對路徑，當然要使用path套件來取得絕對路徑也是可以。
* output則是檔案經過webpack打包後的存放的路徑和檔名。這邊要使用絕對路徑，所以可以透過path套件來取得目前資料夾的路徑。
  * filename的部分，如果像上述[name]的寫法，name會輸出成entry所輸入key的內容。
  * 通常經過webpack打包完的檔案，我們會使用bundle.js。關於module、chunk、bundle的後續會再說明。
* loader的部分，我個人是把它想成一個處理器的概念。他可以幫我們把任何東西轉換成JavaScript，讓webpack能看得懂。
* plugins顧名思義就是外掛的意思。有些webpack的套件需要傳入參數，這時就可以在plugin裡設定。

---

## webpack在package.json設定
在設定完webpack.config.js後，我們要在package.json中的scripts設定webpack啟用的指令。
一般我會設定三個：
* "build": "webpack --mode=production"
* "build:dev": "webpack --mode=development"
* "watch": "webpack --mode=development --watch"

關於mode後面的production和development主要是讓module在打包的時候會選擇使用哪種方式。production通常會讓打包出來的檔案較小，而development主要是要加速開發的速度，也因此在檔案壓縮得部分較小，會盡快把需要編譯的東西編譯完。

watch則是不必每次修改檔案重新使用webpack打包時，需要重新下指令，方便開發時減少指令的輸入。

---
## 補充：關於NODE_EVN問題
一般來說，我們會設定開發過程中和部署的時候做不同的行為，如抓取不同位置的API、ERROR抱錯的產生等等。我們大多都會使用NODE_ENV來設定。

也因此在package.json中的scripts中，我們可能會設定執行甚麼指令會做什麼環境設定。

在linux和mac中，設定的方式為
```
export NODE_ENV=production
```
windows為
```
set NODE_ENV=production
```
也因此如果要跨平台使用，我們可以借用cross-env套件。
安裝方法
```
npm install cross-env --save-dev
```

而在webpack 4後，我們package.json的scripts設定，` --mode production `和` --mode development `其實就會幫我們分別設定NODE_ENV=production或development。
但如果還有其他環境的話，還是可以透過`cross-env NODE_ENV="env"`來進行設定了


