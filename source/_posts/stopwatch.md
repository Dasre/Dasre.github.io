---
title: 簡易碼表設計
date: 2020-04-04 16:26:16
tags:
- w3HexSchool
---

# 簡易碼表
透過setInterval()來固定時間加總秒數。計算啟動時間。

參考code: https://codepen.io/dasre/pen/Baodadp

以下只說明重要的code。

---

# Function
## Start

``` JavaScript
//count 為方便後面停止setInterval使用
count = setInterval(function(){
    if(ms === 100){
      ms = 0;
      if(sec === 60){
        sec = 0;
        min+=1;
      }else{
        sec+=1;
      }
    }else{
      ms+=1;
    }
    
    let re = pad(min,sec,ms);
    document.getElementById('time').innerText = re;
  },10);
```
每10毫秒更新目前的時間。
並做進位動作。
pad()為將<10的數值補為'0X'函式。

## stop

`clearInterval(count);`

## 時間紀錄

抓取目前的min,sec,ms，傳給pad()輸出目前的分、秒、毫秒，並記錄下來。

## reset

將min, sec, ms歸零，重新計時。
並清空時間紀錄。
