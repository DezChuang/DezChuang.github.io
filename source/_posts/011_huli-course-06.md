---
title: '學習｜Huli''s Course#6｜Vanilla.js'
catalog: true
date: 2017-05-18 15:01:15
subtitle: 自己實作原生JS，初步做完改寫看起來不太困難，因為基本的程式邏輯在hw5大致上架構都完成了，只是將jQuery的部分依序改成vanilla。
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - Vanilla
    - JavaScript
    - 前端
    - 初學
---

### Properties

*   `main.js`改名為`index.js`

*   移除`jquery-3.2.1.min.js`

*   改寫.ajax

<figure class="figure-code code"><div class="highlight"><pre>let xhr = new XMLHttpRequest();
xhr.open(method, twitchAPI, true);
xhr.onload = (data) =&gt; {
    if (xhr.status &gt;= 200 &amp;&amp; xhr.status &lt; 400) {
        data = JSON.parse(xhr.responseText);
        callback(null, data);
    } else {
        console.log(err);
    }
};
xhr.onerror = (err) =&gt; {
    console.log(err);
};
xhr.send(null);
</pre></div>
</figure>

*   改寫appendData

<figure class="figure-code code"><div class="highlight"><pre>-      const $container = $('.container');
+      const container = document.querySelector('.container');
       for(const stream of streams) {
-        $container.append(templateData(stream));
+        container.insertAdjacentHTML('beforeend', templateData(stream));
</pre></div>
</figure>

*   改寫scroll

<figure class="figure-code code"><div class="highlight"><pre>-  $(window).scroll(function() {
+  window.addEventListener('scroll', function() {
</pre></div>
</figure>

*   改寫infinite scroll高度判斷

<figure class="figure-code code"><div class="highlight"><pre>-      if($(window).scrollTop() + $(window).height() &gt; $(document).height() - reservedHeight) {
+      if(document.body.scrollTop + window.innerHeight &gt; document.body.scrollHeight - reservedHeight) {
</pre></div>
</figure>

*   修正hw5時，avatar佔位圖無法正確顯示的issue，加上`background-size: 100% 100%`

* * *

### Question Set

#### 用 vanilla js 有哪些優缺點？

Pure Javascript to access the DOM can be faster as you can cut the overhead that jQuery has on this. However it doesn’t always have to be faster as you could write some major mistakes that slow things down again.

<figure>![](http://i.imgur.com/3nDAgQ8.png "jQuery vs Vanilla")</figure>

* * *

### References

*   [YOU MIGHT NOT NEED JQUERY](http://youmightnotneedjquery.com/)
*   [You Don't Need jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery/blob/master/README.zh-CN.md)
*   [使用XMLHttpRequest](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)
*   [document.querySelector](https://developer.mozilla.org/zh-TW/docs/Web/API/Document/querySelector)
*   [Element.insertAdjacentHTML()](https://developer.mozilla.org/zh-TW/docs/Web/API/Element/insertAdjacentHTML)
*   [10.3 How do I find the size of the window?](http://jibbering.com/faq/#getWindowSize)
*   [What is offsetHeight, clientHeight, scrollHeight?](http://stackoverflow.com/questions/22675126/what-is-offsetheight-clientheight-scrollheight)
*   [Get the browser viewport dimensions with JavaScript](http://stackoverflow.com/questions/1248081/get-the-browser-viewport-dimensions-with-javascript)
*   [JavaScript 獲取頁面高度（多種瀏覽器)](https://dotblogs.com.tw/aquarius6913/2011/01/03/20538)
*   [Which is faster (execution-wise): JavaScript or jQuery?](https://www.quora.com/Which-is-faster-execution-wise-JavaScript-or-jQuery)
*   [jQuery vs JavaScript Performance Comparison](https://jsperf.com/jquery-vs-javascript-performance-comparison/14)

* * *

### Troubleshooting

*   Chrome, Safari, Edge pass. IE fail.

* * *

### 直播筆記

*   瀏覽器兼容性

        *   此為為何要用jQuery的理由之ㄧ，已幫忙我們處理不同瀏覽器的兼容性
    *   EX: 此作業的高度

            *   可參考ponpon

*   為什麼要用Vanilla JS

        *   trade-off(權衡/選擇)：考慮優缺點
    *   缺點

            *   不好維護
        *   考慮瀏覽器兼容性問題
        *   團隊合作困難

        *   優點

            *   效率高(考慮到程式規模，規模太小無法發揮作用)
        *   節省網路流量(節省jQuery lib的空間)

*   什麼時候需要用

        *   考慮需求的場景

            *   網速慢的地區
        *   規模大的公司如google、FB
        *   給手機使用的網站

*   appendChild()也可以拿來append HTML tag

* * *

### 後記

自己實作原生JS，初步做完改寫看起來不太困難，因為基本的程式邏輯在hw5大致上架構都完成了，只是將jQuery的部分依序改成vanilla，不過若是考慮跨瀏覽器的問題的話，目前測試使用Chrome、Safari、Edge都可以正常工作，IE 11完全是空白的XD，看來真正的挑戰好像是跨瀏覽器的判斷嗎?

* * *

### Demo

[https://dezchuang.github.io/frontend-intermediate-course/answers/hw6/index.html](https://dezchuang.github.io/frontend-intermediate-course/answers/hw6/index.html)