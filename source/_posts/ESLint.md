---
title: ESLint 安裝與使用
date: 2020-02-04 21:24:32
tags:
- w3HexSchool
---

# ESLint 安裝與使用
ESLint是一個code的檢查工具，他可以幫我們檢查我們code是否符合我們所設定的規範，並指出在哪裡。而code有著一定的規範，可以讓我們在團隊開發中，有著統一的標準，讓整個團隊開發更一致；對於個人則是可以讓我們學習許多資深開發者的想法，有助於個人程式碼攥寫的提升。

---

## ESLint 安裝
自己較常是全域安裝
```
npm install eslint -g
```
要注意ESLint對於node和npm版本有要求，如果不符合ESLint所要求的版本，請將node和npm安裝到目前的穩定版本。
{% asset_img 2.png %}
一般我會使用
```
npm install npm -g
```
將npm重新安裝
再透過
```
npm install n -g
```
安裝n模組
這個n模組其實就是node的版本管理工具
<br>
可參考[官方介紹](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm#osx-or-linux-node-version-managers)&[n模組](https://github.com/tj/n)
<br>
在中國的SegmentFault也看到一篇不錯的說明，有興趣的可以[參考](https://segmentfault.com/a/1190000016956077)

<b><i>請注意上述node版本更新不適用windows上，windows上請直接下載最新版的node並進行覆蓋，或參考官方說明文件，如何在windows上安裝多版本node</i></b>

---

## ESLint建置
在專案中，使用
```
eslint --init
```
建置.eslintrc.js文件
{% asset_img 1.png %}
建置過程中會問一些設定的問題，根據選項去選擇即可。至於style guide部分，有三種規範可以使用。分別是
* Google
* Airbnb
* Standard

其中Airbnb規範最嚴謹，Google次之，個人是選擇Airbnb。關於Airbnb的規範，可參考
此[連結](https://github.com/airbnb/javascript)。目前已經有繁體中文版，上方連結可找到。

在選擇完規範後，會選擇要以yaml、js還是json儲存規範，這邊選擇js。

重開安裝eslint的專案，在問題部分就會提醒你code哪邊不符合規範。
{% asset_img 3.png %}
比如說括號前後要空格，或是props參數沒用到等等。

在.eslintrc.js的文件中，會有許多的設定。
* parser是解析器，可以根據專案設定不同。
* extends可以設定一些設定好的規範，如前面提到的Airbnb規範，就是寫在extends裡。
* rules是指額外的規則，像上方圖片裡面有一條提到js文件裡面不能寫JSX，這個問題我們即可在reles加入```"react/jsx-filename-extension": [1, { "extensions": [".js", ".jsx"] }],```來解決。
* ecmaFeatures是設定可以使用哪些額外功能，像可以使用JSX等等。
* plugins使用一些額外的第三方套件。
* env環境，個人主要在browser和node上。
關於eslint文件設定方法和參數設定，可參考[連結](https://eslint.org/docs/user-guide/configuring)。


 
