---
title: '筆記｜Dez.tw重構日誌#2｜Redux筆記'
catalog: true
date: 2017-04-27 14:57:59
subtitle: Redux筆記
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - Redux
    - 前端
    - 筆記
---

### Redux核心概念

1.  Redux itself is very simple.
2.  To change something in the `state`, you need to dispatch an `action`.
3.  An `action` is a plain JavaScript object.
4.  Finally, to tie `state` and `actions` together, we write a function called a `reducer`.
5.  `Reducer` is just a function that takes `state` and `action` as arguments, and returns the next state of the app.
6.  And we  may write a root reducer to manages the complete state of our app by calling those reducers

### Redux三大原則

1.  單一數據源(Single source of truth)

        *   整個應用的state被儲存在一棵object tree中，並且這個object tree只存在於唯一一個store中
    *   單一state tree讓開發時調整state(CRUD)更容易，

2.  State是read-only的

        *   唯一能改變state的方法就是發出action
    *   action是把數據從應用(view、server響應、使用者輸入等等)傳至store的payloads，是store的唯一資訊來源
    *   payload是承載資料的封包、訊息或程式碼
    *   這樣確保了view和network請求都不能直接修改state，也就是說它們只能表達想要修改的意圖
    *   因為所有的修改都被集中化處理，且嚴格按照一個接一個的順序執行，因此不用擔心race condition的出現

3.  使用純函數(Reducer)來執行修改

        *   為了描述action如何改變state tree ，你需要編寫reducers
    *   Reducer只是一些純函數，它接收先前的state和action，並return新的state

### Redux vs 先前技術

#### Flux

1.  將模型的更新邏輯全部集中於特定層(Flux的store，Redux的reducer)
2.  Flux 和 Redux 都不允許程序直接修改數據，而是用一個叫作`action`的普通對象來對更改進行描述
3.  不同於 Flux ，Redux 並沒有 dispatcher 的概念。它依賴純函數來替代事件處理器。純函數構建簡單，也不需額外的實體來管理它們
4.  Flux 常常被表述為 `(state, action) =&gt; state`。從這個意義上說，Redux 無疑是 Flux 架構的實現，且得益於純函數而更為簡單。
5.  Another important difference from Flux is that Redux assumes you never mutate your data. You should always return a new object, which is easy with the object spread operator proposal, or with a library like Immutable.

#### Elm

1.  Elm 是一個 Evan Czaplicki 創造並受到 Haskell 影響的 functional programming language。它強制 一個「model view update」架構，而它的 update 有以下的 signature：(action, state) =&gt; state。
2.  跟 Redux 不一樣，Elm 是一個 language，所以它能夠從許多東西獲得好處，像是：強制的 purity、靜態型別、內建 immutability、和模式匹配 (使用 case 表達式)。即使你沒有計劃使用 Elm，你也應該閱讀一下 Elm 的架構，並玩玩看它。

#### Immutable

1.  Immutable 是一個實作了 persistent data structure 的 JavaScript library。
2.  Immutable 和大部份類似的 library 跟 Redux 是互補的。請自由的結合他們一起使用！
3.  Redux 不在意你如何儲存 state—它可以是一個一般物件、一個 Immutable 物件、或任何其他東西。你可能需要一個 (de)serialization 機制以撰寫 universal 應用程式並從伺服器 hydrating 它們的 state，但除此之外，你可以使用任何可以支援 immutability 的資料儲存 library。

### Reference

*   [Redux 繁體中文文件](http://chentsulin.github.io/redux/index.html)
*   [Redux 官方文件](http://redux.js.org/)