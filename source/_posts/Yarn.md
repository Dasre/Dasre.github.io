---
title: Yarn
date: 2019-02-20 21:47:57
tags:
---

## NPM與Yarn基本指令

* 目前的npm安裝速度已經大幅的上升了，但整體上仍是yarn稍微快一點。
* 但目前yarn在網路上有發生衝突的狀況
* 以下為個人較常使用之指令

NPM    | YARN  | 敘述
:-----:|:-----:|:-----:|
npm install | yarn install | 
npm install --save [package] | yarn add [package] | 專案中使用之套件 
npm install --save-dev [package] | yarn add [package] --dev | 專案中開發模式中使用之套件
npm install --global [package] | yarn global add [package] | 全域套件（與前面--dev擺放位置不同）
npm uninstall [package] | yarn remove [package] | 移除套件

