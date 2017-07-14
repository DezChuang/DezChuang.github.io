---
title: 筆記｜前端面試考題｜寫出自己能解釋的程式碼
catalog: true
date: 2017-06-17 15:48:15
subtitle: 「不要提交連你都無法解釋的code給團隊，今天假如你花十分鐘寫完，團隊需要花一小時理解這段code，那這段code就是品質非常不好的。」
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - 前端
    - 面試
    - 筆記
---

### 題目

請用Javascript寫一個`function commaNumber(n)`能傳入一數字n(正負整數、浮點數)，輸出一字串整數部分每三位加一個逗號(不能使用toLocaleString)，如下範例。

<figure class="figure-code code"><div class="highlight"><pre>console.log(commaNumber(123)); //'123'
console.log(commaNumber(123456)); // '123,456'
console.log(commaNumber(123456789)); // '123,456,789'
console.log(commaNumber(-987654321.78965)); // '-987,654,321.78965'
</pre></div>
</figure>

### 解題

面試官有一直提示到：「寫多久都可以，但你要讓這段code能以團隊為導向的品質。」一開始天真地以為是要多加一些像`@param`和註解之類的讓程式碼可讀性高一些，至於function演算法感覺不難，所以參考了[網路做法](https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript)就交出了我的版本：

<figure class="figure-code code"><div class="highlight"><pre>/**
 * Add comma to input number per three digits
 * @param {Number}
 * @returns {String}
 *
 * Reference
 * https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript
 */
function commaNumber(n) {
    var parts = n.toString().split(".");
    parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    return parts.join(".");
}
</pre></div>
</figure>

原本有稍微看了一下RegExp的部分在做什麼，但`\B`的解釋沒看很懂，就想說先交再說好了，然後我就被問成炮灰了X)。首先這段程式碼重點就在`/\B(?=(\d{3})+(?!\d))/g`這一串正規表達式的解釋，可是我無法解釋`\B`與`\b`的差別，更別說後面`?=`與`?!`的意思了。

面試官又給我兩個方向，一是選擇研究後搞懂這段code，另一個是重新寫一個你能解釋的版本，然後我就不知死活的選了前者X(。看了十幾分鐘還是看不懂，不得不說正規表達式真的不熟，直覺不選後者是稍微想了一下只想到暴力解如下：

<figure class="figure-code code"><div class="highlight"><pre>function commaNumber(n) {
    var parts = [];
    var num = [];
    var negative = false;
    if(n &lt; 0) {
        negative = true;
    }
    parts = n.toString().replace("-","").split(".");
    num = parts[0].split("");
    for(var i = num.length - 4; i &gt; 0; i -= 3) {
        num[i] += ",";
    }
    parts[0] = num.join("");
    parts = parts.join(".");
    if(negative) {
        parts =  "-" + parts;
    }
    return parts;
}
</pre></div>
</figure>

可能是覺得不夠優美，不過假如我無法解釋`/\B(?=(\d{3})+(?!\d))/g`的話，那就對於團隊程式碼的品質而言，後者反而是比較好的。

### 後記

沒想到這麼簡單的題目，因為沒注意到細節就直直帶著壞習慣往陷阱裡跳了，真的是魔鬼藏在細節中。被震撼教育了一番後也發現自己的盲點：「常常為了求快，並沒有仔細去理解參考文件的內容。」跟一個非常基本的觀念：「不要提交連你都無法解釋的code給團隊，今天假如你花十分鐘寫完，團隊需要花一小時理解這段code，那這段code就是品質非常不好的。」

不經一事不長一智，從簡單的一個題目能發現盲點與觀念也算是很有收穫，另外就是正規表達式也需要加強。