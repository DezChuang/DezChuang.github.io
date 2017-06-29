---
title: 心得｜新手架站心得｜Node.js & MongoDB
catalog: true
date: 2017-04-06 22:15:00
subtitle: 或許架站並不難做，套套版型不用幾天就能完成，但想想我也是從頭到尾用sublime刻出來的，容我稱呼自己為「那個造輪子的魔術師」吧！
header-img: "../../../../img/header_img/bg.png"
tags:
    - Node
    - MongoDB
    - Blog
    - ejs
    - 後端
    - 初學
    - 心得
---

首先先附上成果： [http://52.197.26.179/](http://52.197.26.179/)，這是還沒買網域的AWS EC2網址，請安心前往。

<br>

## 前言
由於一直想架一個部落格，雖然辦了logdown還是覺得手癢，趁著最近需要弄些作品集，決定把之前在Udemy的[The Web Developer Bootcamp](https://www.udemy.com/the-web-developer-bootcamp/)課程中，還未完成的「YelpCamp實作」學完，並剛好利用這個版型，自己重構成專屬的部落格。

這門課是之前原本打算學Python時，偶然從[Pala.tw](http://pala.tw/learn-web-development-on-udemy/#course_4)得知的網頁開發入門，也正式讓我的學習步入正軌。從最基本的前端HTML/CSS/Javascript/jQuery，到後端Node.js/Express/MongoDB/Authentication/Flash Message等等，雖然並沒有時下最潮React，但算是能讓初學者從前到後打通任督二脈的課程，由於課程內容非常豐富，前前後後斷斷續續我就這樣學了半年，直至近一個月，卯起來把最後的後端功能補齊，正式完成了YelpCamp project。
<br>
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1675245/rjYLXicrTGOgG4xRq0NQ_Screen%20Shot%202017-04-06%20at%208.30.23%20PM.png">
<br>
這個YelpCamp練習，後端的功能含有註冊登入等Authentication、文章與評論的新增修改刪除，剛好是個很適合改造成部落格的版型，大概在兩個禮拜前開工，在改造前腦力激盪了許多需要設計與新增的功能：
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1675245/XW9Bx3L8Rgd87bV4mOHA_Screen%20Shot%202017-04-06%20at%208.59.02%20PM.png">
<br>

## 模仿與重構
實際上自己動手做才是挑戰的開始，想不出來的問題，沒有課程錄影可以直接給解答，只能常常把chrome開滿滿的分頁，自己挖每一個問題的解法，不過這樣每完成一項自己開發的功能，就得到一點成就感，或許就是網頁開發迷人的地方，就算整天都在整理這個不知道會不會被看到的小東西，卻也不覺得疲乏，大概就是這樣我才選擇成為前端工程師(雖然我現在一直在玩Node.js...)

從上面的表就可以看到，目前還有一些未完成的功能，也有一些雖然看起來完成的項目，其實也只是做個暫時的walk around，像是我一直想解決如何做一個administrator的schema、如何把評論系統做成nested comment、發文的圖片上傳插入功能、如何套用markdown view engine等等，這些功能很有趣，但基於時間的考量暫且擱置。

以下就讓我們來看一下半成品的畫面：
### (1)歡迎頁
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1675245/wGuxnASyRva36RXVfjYV_Screen%20Shot%202017-04-06%20at%208.11.13%20PM.png">

歡迎頁是參考課程助教Ian的[background-slide](https://github.com/nax3t/background-slider)來實作，是一些CSS animation的操作，比較有趣的是原來背景投影片使用\<li\>的tag來排序：
```
.slideshow li {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  background-size: cover;
  background-position: 50% 50%;
  background-repeat: no-repeat;
  opacity: 0;
  z-index: 0;
  animation: imageAnimation 60s linear infinite;
}
```
再暫時套用幾張Pixabay的CC0高解析圖片，莫名的就很有質感。
<br>
### (2)文章列表
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1675245/oCwwF7REKt8w22RUhYlA_Screen%20Shot%202017-04-06%20at%209.20.54%20PM.png">

由於Bootstrap中喜歡的套件不多，所以我又引入Semantic UI來做一些styling，目前整體的畫面還是稍微粗糙，未來在重構時打算將小元件與背景色系統一，或許再把歡迎頁與主頁的header合併。

在實作這整個部落格時，常常需要切版與調整font-size，這時原本不熟練的Google Chrome DevTools都慢慢地上手了，感覺像修改style有playground的感覺，再直接套到主CSS檔即可完成，不過目前全部集中在同一隻main.css中，算是之後需要重構的地方。

這裡要特別提一下最上方的Navbar，RWD與burger button是原本YelpCamp就有的功能，不過要讓它浮動在最上方倒是加上了一些jQuery來實作，甚至額外又加上去的回到頂端功能也是jQuery，一直有一個哪天用React重構成SPA的想法，不知道到時候會不會整個view都要砍掉重練(汗...)
<br>
### (3)文章與評論
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1675245/gAKzRm5nTma43rAL23nJ_Screen%20Shot%202017-04-06%20at%209.34.07%20PM.png">

時間的部分使用了[Moment.js](https://momentjs.com/)，其中遇到的問題是前幾天deploy時，資料庫的所在地是美國，所以需要再另外加上時區的API才能正確顯示+8的時間：
```
<%= moment.tz(post.created, 'Asia/Taipei').format('MMM Do YYYY') %> | <%= moment.tz(post.created, 'Asia/Taipei').format('hh:mm:ss a') %>
```
以及讓文章在文章列表能依發布時間反向堆疊也是做了一點手腳：
```
<% posts.reverse().forEach(function(post){%>
```
而一定要提的就是評論的部分，因為在schema部分是接在每篇文章後，不禁讓我想到那段被多維指標陣列荼毒的日子，雖然這是個稍微複雜的功能，但後來發現沒有nested reply整個就是很不滿意，大可直接套用DISQUS即可(還可以貼圖片哩)。又因為我也還沒實作修改個人資料等功能，若把我的評論系統拿掉的話，遊客註冊會員就變成一個雞肋的存在了，所以暫時放著以示我是真的會做，之後打算仿照DISQUS來做一套schema。
<br>
## 部署選擇
關於部署(Deploy)平台有兩個選擇，之前放demo作品的Heroku與從頭自己來的AWS EC2，稍微研究了一下Heroku的收費方案，如果用免費的package，每次要開啟網站時還得先等一段叫醒server的時間，整個UX可能沒那麼好。於是這兩天我一直在玩AWS EC2的部署，意外地沒那麼難，不過也是零零散散搜集了一些2010~2014年間「EC2 nodejs deploy」之類的文章才終於架好，實在有必要在[筆記一篇](http://dez.logdown.com/posts/2017/04/07/aws-ec2-deploy-nodejs-web-app)2017年版的，供未來健忘的我與入門的人們做參考。

目前部署的成果有以下兩個：
* Heroku: [http://deztwblog.herokuapp.com/](http://deztwblog.herokuapp.com/)
* AWS EC2: [http://52.197.26.179/](http://52.197.26.179/)

之後打算再去買個網域與更換比較好用的資料庫方案，目前是暫時使用[mLab](https://mlab.com/)的免費方案，之後打算來研究AWS S3與DynamoDB，或是尋找一個比較好的MongoDB部署平台，從這邊開始感受到全端一條龍兩頭燒的感覺啊...
<br>
## 後記
或許這個看似陽春的部落格並不難做，可以套套版型不用幾天就能完成，但想想我也是從頭到尾用sublime刻出來的，容我稱呼自己為「那個造輪子的魔術師」吧！

最近後端玩得太久了，差不多該開始把當初還沒看完的React再起爐灶，之後會趁著想玩點新東西時把「暫時不做的功能」補完，也有打算利用這個部落格為模板，做為我練習React的作品，如果可以做成SPA、ajax應該會很酷。

目前需要再補上React Router、Flux、Redux、ImmutableJS、Isomorphic等等的技能，之後可能就會開始多一些討論React相關的文章了！

### Apr. 13th更新
還可以再利用Gulp來優化網站，[參考文章](http://ithelp.ithome.com.tw/articles/10185976)。

<br>

## Reference
* [The Web Developer Bootcamp](https://www.udemy.com/the-web-developer-bootcamp/)
* [Pala.tw - Udemy與網頁設計課程推薦](http://pala.tw/learn-web-development-on-udemy/#course_4)
* [Full Screen Background Image Slider](https://github.com/nax3t/background-slider)
