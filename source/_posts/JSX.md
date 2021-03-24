---
title: JSX
date: 2019-03-13 21:22:22
tags: "react"
---

# JSX

### JSX用於React裡面快速定義HTML模板，其與HTML的差異
* HTML有些標籤不用有 "/"結尾符號，在JSX裡面一定要
  * ```HTML
      correct
      <!-- self-closing element -->
      <input type="text" />
      <!-- close tag element -->
      <input type="text"></p>
      error
      <input type="text">
    ```

* HTML裡面的"class"要變成"className", "for"要變成"htmlFor"
  * ```HTML
      <div className="..."></div>
      <label htmlFor="check"></label> 
    ```

* 駝峰式命名
  * ```HTML
      onclick -> onClick
      className
      ...
    ```

* 大括號{} 可用大括號定義值或表達式，沒用大括號就如同HTML是字串