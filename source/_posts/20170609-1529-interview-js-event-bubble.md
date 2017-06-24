---
title: 筆記｜前端面試考題｜事件冒泡
catalog: true
date: 2017-06-09 21:16:09
subtitle: 昨天開始了第一次的前端面試，整體而言準備不夠周全，後來查了一些資料發現有一些還是前端面試經典題(汗)。慢慢補上一些沒回答得很好的題目，並找出可能的解法，有錯歡迎來信或留言指正。
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - JavaScript
    - stopPropagation
    - 前端
    - 筆記
---

### [JS] 事件冒泡

#### 今天要對一個id操作要怎麼寫？要對這個id加事件怎麼做？要強制把動作停下來，有什麼方法？

<figure class="figure-code code"><div class="highlight"><pre>&lt;body&gt;
  &lt;a id="child" href="http://www.google.com/"&gt;
    子元素
  &lt;/a&gt;
  &lt;script type="text/javascript"&gt;
    var child = document.getElementById("child");
    child.addEventListener("click",function(e){
      e.preventDefault(); //終止預設行為
      console.log("click-child");
    },false);
  &lt;/script&gt;
&lt;/body&gt;
</pre></div>
</figure>

答案應該是**`e.preventDefault()`**，因為沒用過沒回答出來，看了[這篇](https://dotblogs.com.tw/harry/2016/09/10/131956)講得很詳細：

「event.preventDefault()就是終止預設行為(Stop Event Flow)；以『超連結』為例，瀏覽器看到頁面上有超連結，只要偵測到超連結被點擊到，隨即會幫我做『導向連結』的動作，『導向連結』即是超連結的預設行為。」

#### 今天有a與b，都有click事件，a在b的內部，那點擊a後行為是先a再b，那今天我不想先a後b，我有什麼方法可以把兩者分開？

這是個自學中沒遇過的問題，所以被問到的當下只能回答「沒使用過」，面試結束後自己寫小程式測試，但還是不太理解題意，在Slack群組稍微問了一下得到回覆：

*   「兩個DOM元素彼此包起來之後，onClick事件其實好像蠻常看到的，像是圖+文字合成一個區塊。」
*   「之前摸公司的專案也有遇過類似的問題，上層選單點了，下層的物件也被觸發了XDDD」
*   「如果要處理在 html 裡面兩個互為父子的元素，關鍵字：js 事件冒泡」

才發現這是在前端工作很常碰到的問題，這題完全精闢地區分了前端經驗的多寡，感謝各位大大的提示。

後來針對事件冒泡看了一些文件，[這篇](http://www.cnblogs.com/bfgis/p/5460191.html)講的蠻清楚的。所以題意應該如下：

<figure class="figure-code code"><div class="highlight"><pre>&lt;body&gt;
    &lt;a id="parent"&gt;
        父元素
        &lt;a id="child"&gt;
            子元素
        &lt;/a&gt;
    &lt;/a&gt;
    &lt;script type="text/javascript"&gt;
        var parent = document.getElementById("parent");
        var child = document.getElementById("child");

        parent.addEventListener("click",function(e){
            console.log("click-parent");
        },false);

        child.addEventListener("click",function(e){
            e.stopPropagation();
            console.log("click-child");
        },false);
    &lt;/script&gt;
&lt;/body&gt;
</pre></div>
</figure>

答案即是在子元素加上**`e.stopPropagation()`**如下：

<figure class="figure-code code"><div class="highlight"><pre>child.addEventListener("click",function(e){
    e.stopPropagation();
    console.log("click-child");
},false);
</pre></div>
</figure>

#### Reference

*   [[jQuery] event.preventDefault() 與 event.stopPropagation() 的差異](https://dotblogs.com.tw/harry/2016/09/10/131956)
*   [JavaScript 详说事件机制之冒泡、捕获、传播、委托](http://www.cnblogs.com/bfgis/p/5460191.html)