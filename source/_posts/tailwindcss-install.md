---
title: Tailwind CSS - 安裝
date: 2021-09-22 23:31:55
tags:
- Tailwind
---

# Tailwind CSS安裝方式
這邊紀錄三種安裝方式，詳細可參考 [安裝方式](https://tailwindcss.com/docs/installation)。

## Webpack
這種作法主要是要將Tailwind CSS做為postcss的插件，並透過webpack打包處理。

1. 安裝tailwindcss package，若本身已經配置postcss和autoprefixer，後面兩個package可以不安裝。
```shell
$ npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```
  若為現有專案要加入tailwind，且postcss不是最新的8.x.x版本，可參考連結去進行調整。[連結](https://tailwindcss.com/docs/installation#post-css-7-compatibility-build)


2. postcss加入tailwindcss plugins
```JavaScript
// postcss.config.js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

3. 產tailwindcss設定檔
```shell
$ npx tailwindcss init
```
透過上述指令，產生`tailwind.config.js`檔案，其格式如下
```JavaScript
// tailwind.config.js
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}
```
4. 最終將tailwindcss樣式庫加入css檔案，並導入會被webpack打包之js檔案。


## Tailwind CLI

## Tailwind CDN