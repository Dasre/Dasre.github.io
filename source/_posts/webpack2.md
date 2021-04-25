---
title: webpack(2)
date: 2020-02-17 08:45:25
tags:
- w3HexSchool
---

銜接前面一篇的webpack安裝與簡易配置，這次說明webpack的loader和plugin。

## webpack loader
我們前面entry的index.js檔案，有使用到es6的語法，雖然目前市面上的browser大多對es6的語法都可以解析了，但並不是全部都如此，也因此這邊會使用到[babel-loader](https://github.com/babel/babel-loader)，幫助我們把es6的語法轉成es5。

index.js
{% asset_img 1.png %}
原本打包完的檔案內容
{% asset_img 1.png %}

---

## 安裝方法
```
npm install -D babel-loader @babel/core @babel/preset-env
```

安裝完後，我們要進webpack.config.js進行設定。在rules的方面，我們可以直接參考github上的使用說明。

```
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```
但我個人喜歡寫成這樣
```
const babel = {
  test: /\.m?js$/,
  exclude: /(node_modules|bower_components)/,
  use: {
    loader: 'babel-loader',
    options: {
      presets: ['@babel/preset-env']
    }
  }
}

module: {
    rules: [ babel ]
  }
```
這樣寫的好處是可以快速的找到原本定義的內容，且如果當你有不只一個loader要使用時，較不會rules下面有一長串的內容。

我們剛剛安裝babel時，安裝了3個babel的package。
* babel-loader基本上就是告訴webpack遇到什麼檔案需要使用我這個loader來編譯。
* @babel/core，babel的核心，使用babel的API。
* @babel/preset-env，可以根據我們對於瀏覽器的設定，來自動尋找要使用的babel版本。或是直接使用最新版的babel即可。

上述webpack設定內容
* test是正規表達式，告訴module我們遇到.js的檔案時，要使用下面的設定來編譯。
* exclude是表示要排除掉哪些檔案，這邊預設已經排除掉node_modules，如果還要排除掉其他的資料夾，如上述的code "|folder"。
* use裡面的loader是告訴webpack遇到.js檔案要使用哪個loader來進行處理。options就是其他的細部設定，可以設定瀏覽器版本等等，更多詳細可以參考[連結](https://babeljs.io/docs/en/babel-preset-env)。

經過babel-loader編譯完成後，內容會變更
{% asset_img 3.png %}
原本的const宣告變數方法變成var，箭頭函式變成原本function(){}的寫法。

---
## SCSS to CSS


