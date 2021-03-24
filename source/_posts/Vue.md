---
title: Vue
date: 2019-02-19 17:56:23
tags:
---

### 模板語法
---
* 雙大括號中插入數值或文字
* v-once : (只印出一次，後續不會改變值)
* v-html : 可印出包含html字串的東西。有危險(可以包<script></script>可能有攻擊行為)
* v-bind : 指定屬性為資料，要使用v-bind(ex: v-bind:checked="selected")
    * 縮寫(:) 
    * ex:
    ``` javascript
    v-bind:checked="count%2===0" -> :checked="count%2===0"
    ```
* v-on : 偵聽DOM輸入事件的方法
    * 縮寫(@)
    * ex: 
    ``` javascript
    v-on:click="add" -> @click="add"
    ```
* v-model : 雙向綁定
    * ex: https://codepen.io/dasre/pen/dqoeWO
---

### Vue 實例(instance)
---
* 實例 instance(具體的個體)
* 類別 Class(廣泛描述的概念)


Human -> class
const me = new Human(); -> instance
const you = new Human(); -> instance

me !== me

Vue -> 
   ``` javascript 
   const vm  = new Vue({}) 
   ```
   
* el : 元素(使用CSS選擇器)
* $mount : 掛載(連結el的動作)
    * 掛載只會掛載到第一個符合的元素上(所以一般都會用id)
    * ex : https://codepen.io/dasre/pen/ZMGZEV?editors=1010
    * 掛載方式2 : https://codepen.io/dasre/pen/vzOMNy?editors=1010
    * 需要時再掛載
    ``` javascript
    vm.$mount('#app');
    //也可以寫成
    vm.$mount(document.getElementById('app'));
    ```
* template : 模板(定義掛載上去長怎樣)
    * ex : 可以寫在js裡面取代html那個東西
    ``` javascript
    const vm = new Vue({
        el:'#app',
        template:"<div><h2>{{msg}}{{msg}}</h2></div>",
        data:{
            msg:'Hello',
        },
    });
    ```
* data : 狀態
    * ex : https://codepen.io/dasre/pen/RYPOjZ?editors=1011
* methods : 方法
    * methods和data裡面的命名不要一樣(避免衝突)
    * methods裡面不可以使用箭頭函式(會造成this是global的)
    * ex : https://codepen.io/dasre/pen/xaGemE?editors=1010
* computed : 計算屬性
    * ex : https://codepen.io/dasre/pen/oPXKEY
* watch : 偵聽器
    * 偵聽data methods computed裡面的值
    * ex : https://codepen.io/dasre/pen/WgQevB
    * 不要濫用watch

---

### 條件判斷
---
* v-if : 控制元素要不要看的見(不是用CSS方法看不見，而是直接刪除那段code)
    * template : ``` HTML5     
    <template></template> ```
    * 用來包住其他元素
    * ex : https://codepen.io/dasre/pen/NLNYqZ
* v-else : 一定要接再v-if之後
    * v-if 和 v-else之間不能有其他tag存在
    * v-else和v-if東西不會共存(不是用CSS方法看不見)
    * ex : https://codepen.io/dasre/pen/NLNYRQ
* v-else-if : 提供不只兩種選擇
    * ex : https://codepen.io/dasre/pen/yxOKXJ
* v-show : show或不show(顯示出來或不顯示出來)
    * 用 display:none CSS這個方法
    * v-show後面沒有v-else
    * v-show不能和template在一起(因為HTML本來就沒有template這東西)，所以如果是True的狀況，無法顯示
    * ex : https://codepen.io/dasre/pen/NLNYwP
    