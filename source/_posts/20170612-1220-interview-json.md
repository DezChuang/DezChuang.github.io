---
title: 筆記｜前端面試考題｜確認JSON是否有值
catalog: true
date: 2017-06-12 21:16:12
subtitle: 如果JSON有個資料是a.b.c，那在js中要怎麼寫來確定a.b.c存在？
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - JSON
    - 前端
    - 面試
    - 筆記
---

被問到這題時，直覺地回答是`if(a.b.c == null)`，面試官好像很驚訝地問：「你的js寫多久了？」，看這反應是答得不對，不確定答案也是跟沒刷leetcode與平常自己實作並沒什麼做error判斷，於是來研究一下JS的一些基本觀念。

#### JSON值

*   JSON值可以是object, array, number, string, true, false, or null。而undefined並非合法JSON值，即使undefined在Javascript中合法。

#### == 與 === 差別

<figure class="figure-code code"><div class="highlight"><pre>1 == '1';  //true, == 會對被判別的變數做轉換型別的動作
1 === '1'; //false
</pre></div>
</figure>

#### undefied 與 null 差別

*   null表示"沒有對象"，即該處不應該有值
*   undefined表示"缺少值"，就是此處應該有一個值，但是還沒有定義。
*   undefined 不是一個有效的 JSON，而 null 是有效的。

<figure class="figure-code code"><div class="highlight"><pre>null == undefined // true
null === undefined // false
</pre></div>
</figure>

#### 解答

一開始以為是因為undefied與null的關係，不過既然JSON中並沒有undefied，那跟這就沒關係，也去看了一下twitch API，裡面的確只有null值，但還是筆記一下。綜合上述，要檢查JSON中是否有值，應該是`if(a.b.c === null)`就可以判斷。

#### Reference

*   [[Javascript] 等於的運用 - ==和 ===的不同之處](https://dotblogs.com.tw/alantsai/2013/06/27/106134)
*   [JSON undefined value type](https://stackoverflow.com/questions/13796751/json-undefined-value-type)
*   [undefined与null的区别](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
*   [undefined 和 null 的差別](http://www.jstips.co/zh_tw/javascript/differences-between-undefined-and-null/)