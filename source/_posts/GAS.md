---
title: 使用Google Apps Script(GAS)串接Google Sheets
date: 2020-03-16 16:26:20
tags:
- w3HexSchool
---

# Google Apps Script
Google Apps Script(GAS)是什麼，可以參考[wiki](https://en.wikipedia.org/wiki/Google_Apps_Script)的介紹。但我一般會把它解釋成一個後端，類似nodejs之類的。

在GAS裡面，你可以透過JavaScript去連接Google的各類服務，或是去連接Google的Firebase資料庫也是可以的。這邊我們會使用GAS來串接Google Sheets。

## GAS連結Google表單
要開啟GAS的編輯器，可以從Google表單上方的工具列 <b>工具>指令碼編輯器</b> 或是在 <b>Google雲端硬碟右鍵>更多>Google Apps Script（要先連結GAS應用程式）</b>，開啟後副檔名應該會是gs。

目前的GAS是可以使用es6語法的，但因為要做一些對應的設定，這邊我們會使用較舊的JavaScript語法撰寫。

```JavaScript
function doPost(e) {
  //取得參數
  var params = JSON.parse(e.postData.contents);
  var num = params.num;
  var one = params.one;
  var one_other = params.one_other;
  var boss_one = params.boss_one;
  var to = params.name;
  var date = params.date;
 
  //sheet資訊
  var SpreadSheet = SpreadsheetApp.openById("");
  var Sheet = SpreadSheet.getSheets()[0];
  
  
  //setValue...
  ...

  return ContentService.createTextOutput(params);
}
```

上述我們撰寫了一個doPost的function。
doPost其實就是我們在Call這隻gs檔的API，進行post時會觸發的function。

我們可以先透過e這個參數取得post的資料。
接下來透過`SpreadsheetApp.openById("")`選擇要開啟哪個Google表單的檔案，再透過`SpreadSheet.getSheets()[0]`綁定好選擇的檔案裡面的哪張表(0表示第一張表)。

選擇好表後，就可以透過getRange()取得表的指定格子位置，並透過setValue()或setFormula()方法來將值存入。

最後的return則是要回傳什麼內容。

## GAS部署
在寫完GAS的code後，我們要部署並產生API。
選擇<b>發佈>部署爲網路應用程式</b>，將具有應用程式存取權的使用者改爲 “Anyone, even anonymous“ ，並點選部署。

接下來第一次部署會出現權限核對的一些設定。
基本上就是<b>核對權限>選擇自己的帳戶>進階>前往>允許</b>。
點選完畢後會出現下圖
![](api.png)

那串URL就是你的API路徑。

---
## 補充
* 在GAS內沒有console.log()，要使用Logger.log()

* GAS的goGet()和doPost()方法，不能直接return一個object。但可以轉成JSON回傳，詳細可[參考](https://developers.google.com/apps-script/guides/content)。






