---
title: '學習｜Huli''s Course#1｜基本 HTML/CSS 練習'
catalog: true
date: 2017-04-16 15:02:40
subtitle: HW1
header-img: "../../../../img/header_img/bg.png"
tags:
    - Front-End
    - HTML
    - Flexbox
    - 切版
    - 前端
    - 初學
---

### Question Set

#### 1.請問 CSS 的屬性position有哪幾種值？

> `static`、`relative`、`fixed`、`absolute`

#### 2.承上，請問那幾種值有哪些區別？請講出適合應用的地方。

<table>
<thead>
<tr>
<th>種類</th>
<th style="text-align: left">用途</th>
</tr>
</thead>
<tbody>
<tr>
<td>static</td>
<td style="text-align: left">
`static` 是預設值。任何套用 `position: static` 的元素「不會被特別定位」在頁面上特定位置，而是照著瀏覽器預設的配置自動排版在頁面上，所有其他的屬性值都代表該元素會被定位在頁面上。</td>
</tr>
<tr>
<td>relative</td>
<td style="text-align: left">在一個設定為 `position: relative` 的元素內設定 `top` 、 `right` 、 `bottom` 和 `left` 屬性，會使其元素「相對地」調整其原本該出現的所在位置，而不管這些「相對定位」過的元素如何在頁面上移動位置或增加了多少空間，都不會影響到原本其他元素所在的位置。</td>
</tr>
<tr>
<td>fixed</td>
<td style="text-align: left">
`position: fixed`的元素會相對於瀏覽器視窗來定位，這意味著即便頁面捲動，它還是會固定在相同的位置。和 `relative` 一樣，我們會使用 `top` 、 `right` 、 `bottom` 和 `left` 屬性來定位。</td>
</tr>
<tr>
<td>absolute</td>
<td style="text-align: left">
`absolute` 與 `fixed` 的行為很像，不一樣的地方在於 `absolute` 元素的定位是在他所處上層容器的相對位置。如果這個套用 `position: absolute` 的元素，其上層容器並沒有「可以被定位」的元素的話，那麼這個元素的定位就是相對於該網頁所有內容（也就是 `&lt;body&gt;` 元素）最左上角的絕對位置，看起來就是這張網頁的絕對位置一樣，所以當你的畫面在捲動時，該元素還是會隨著頁面捲動。</td>
</tr>
</tbody>
</table>

![](https://internetingishard.com/html-and-css/advanced-positioning/css-positioning-schemes-790d5b.png)

#### 3.display的三個值inline, block, inline-block有什麼異同？可以試著舉出幾個例子嗎？

<table>
<thead>
<tr>
<th>display種類</th>
<th style="text-align: left">異同</th>
</tr>
</thead>
<tbody>
<tr>
<td>block</td>
<td style="text-align: left">A block element has some whitespace above and below it and does not tolerate any HTML elements next to it.</td>
</tr>
<tr>
<td>inline</td>
<td style="text-align: left">An inline element has no line break before or after it, and it tolerates HTML elements next to it.</td>
</tr>
<tr>
<td>inline-block</td>
<td style="text-align: left">An inline-block element is placed as an inline element (on the same line as adjacent content), but it behaves as a block element.</td>
</tr>
</tbody>
</table>

![](https://i.stack.imgur.com/mGTYI.png)

#### 4.有哪些 HTML 元素是 inline, 哪些是 block？

<table>
<thead>
<tr>
<th>種類</th>
<th>例子</th>
</tr>
</thead>
<tbody>
<tr>
<td>inline</td>
<td>
`&lt;span&gt;`、`&lt;em&gt;`、`&lt;strong&gt;`、`&lt;a&gt;`、`&lt;img&gt;` ...</td>
</tr>
<tr>
<td>block</td>
<td>
`&lt;div&gt;`、`&lt;h1&gt; - &lt;h6&gt;`、`&lt;p&gt;`、`&lt;form&gt;` ...</td>
</tr>
</tbody>
</table>

**[補充]**

每個HTML元素render時都被以box的樣貌顯示，其中分為`inline`與`block`兩種屬性

![](https://internetingishard.com/html-and-css/css-box-model/inline-vs-block-boxes-f3e662.png)

<table>
<thead>
<tr>
<th>種類</th>
<th style="text-align: left">異同</th>
</tr>
</thead>
<tbody>
<tr>
<td>block</td>
<td style="text-align: left">
`block`屬性的元素如其名，預設會以區塊的樣貌顯現並影響排版。預設上，他的`width`會受其parent container影響，而`height`會受其所含內容多寡影響。</td>
</tr>
<tr>
<td>inline</td>
<td style="text-align: left">
`inline`屬性的元素並不會影響block的排版，而是用來設定block內元素的外型。</td>
</tr>
</tbody>
</table>

<figure class="figure-code code"><div class="highlight"><pre>h1, p {
  background-color: #DDE0E3;    /* Light gray */
}

em, strong {
  background-color: #B2D6FF;    /* Light blue */
}
</pre></div>
</figure>

![](https://internetingishard.com/html-and-css/css-box-model/block-boxes-and-inline-boxes-7cfa0a.png)

#### 5.當我設定一個元素的width為300px，並且padding設成10px之後，這個元素的寬度應該會是多少？

> 320px， 可使用`box-sizing:border-box`預設為300px

參考[這篇](http://pinkyvivi.pixnet.net/blog/post/1131260-css%E2%88%A5%E6%8E%92%E7%89%88%E8%A7%80%E5%BF%B5-boxmodel%3Emargin%E3%80%81padding)

![](http://pic.pimg.tw/pinkyvivi/1164624374.gif)

#### 6.這次實作的畫面當頻道名稱字太多的時候，會超出一格的大小或者會直接被卡掉，有沒有辦法讓字太多的時候在尾巴顯示...？例如原本名稱叫做：「1234567」，顯示的時候變成：「12345...」？

<figure class="figure-code code"><div class="highlight"><pre>p {
  white-space: nowrap;      /* no newline */
  overflow: hidden;         /* crop text */
  text-overflow: ellipsis;  /* ... */
}
</pre></div>
</figure>

### References

*   [https://internetingishard.com/html-and-css](https://internetingishard.com/html-and-css)
*   [http://zh-tw.learnlayout.com/position.html](http://zh-tw.learnlayout.com/position.html)
*   [https://www.w3schools.com/html/html_blocks.asp](https://www.w3schools.com/html/html_blocks.asp)
*   [http://stackoverflow.com/questions/9189810/css-display-inline-vs-inline-block](http://stackoverflow.com/questions/9189810/css-display-inline-vs-inline-block)
*   [http://pinkyvivi.pixnet.net/blog/post/1131260-css%E2%88%A5%E6%8E%92%E7%89%88%E8%A7%80%E5%BF%B5-boxmodel%3Emargin%E3%80%81padding](http://pinkyvivi.pixnet.net/blog/post/1131260-css%E2%88%A5%E6%8E%92%E7%89%88%E8%A7%80%E5%BF%B5-boxmodel%3Emargin%E3%80%81padding)

### Troubleshooting

*   thumbnail與meta中間的空白
調整如下可解決，可以再仔細研究一下display的屬性，或許thumbnail與avatar使用img會比較好：

<figure class="figure-code code"><div class="highlight"><pre>.stream-item {
  display: inline-block;
    ...
}

.thumbnail {
    ...
}

.meta {
  display: inline-block;
    ...
}

.avatar {
  display: inline-block;
    ...
}

.content {
  float: right;

}
</pre></div>
</figure>

*   最底部container與body間的不知名空白

*   不知道為什麼 `.bgmask { display: inline-block; ... }` 這行拿掉會沒有半透明遮罩

### Demo

[https://dezchuang.github.io/frontend-intermediate-course/answers/hw1/index.html](https://dezchuang.github.io/frontend-intermediate-course/answers/hw1/index.html)