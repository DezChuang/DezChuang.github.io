---
title: 筆記｜Javascript基本觀念｜Closure與Free variable
catalog: true
date: 2017-06-11 14:54:59
subtitle: Closure與Free variable觀念重點摘要與理解。
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - 前端
    - JavaScript
    - 筆記
---

直接看例子：

<figure class="figure-code code"><div class="highlight"><pre>function doSome() {
    var x = 10;
    function f(y) {
        return x + y;
    }
    return f;
}

var foo = doSome();
console.log(foo(20));  // 30
console.log(foo(30));  // 40
</pre></div>
</figure>

上面的f即為Closure(閉包)，x稱為free variable(閒置變數)。閒置變數x是指相對於f而言，是既非區域變數也非參數的變數，而擁有閒置變數的運算式則稱為Closure。

對於f而言，x是外部函式變數，而f將變數x關入自己的範圍，x在doSome執行完畢後，原本應該結束自己的生命週期，但f建立了closure並傳回，使得x能繼續存活。

而如果f並沒有捕捉任何變數，例如下面例子：

<figure class="figure-code code"><div class="highlight"><pre>function doSome() {
    var x = 10;
    function f(y) {
        return y + 10;
    }
    return f;
}
</pre></div>
</figure>

則f就只是單純的(一級)函式而已。

其他關於closure的例子：

<figure class="figure-code code"><div class="highlight"><pre>function doSome() {
    var x = 10;
    function f(y) {
        x = x + y;
        return x;
    }
    return f;
}

var foo = doSome();
console.log(foo(20));  // 30
console.log(foo(30));  // 60
</pre></div>
</figure>

<figure class="figure-code code"><div class="highlight"><pre>function doSome() {
    var x = 10;
    function f(y) {
        x = x + y;
        return x;
    }
    return f;
}

var foo1 = doSome();
var foo2 = doSome();
console.log(foo1(20));  // 30
console.log(foo2(20));  // 30

</pre></div>
</figure>

### Reference

*   [JavaScript 語言核心（12）Closure 與一級函式](http://www.codedata.com.tw/javascript/essential-javascript-12-closure-first-class-function/)