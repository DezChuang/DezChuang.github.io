---
title: 筆記｜在AWS EC2部署Node.js web教學
catalog: true
date: 2017-04-09 12:22:00
subtitle: 趁著記憶還新鮮，把這幾天蒐集關於「AWS EC2部署Node.js網頁應用程式」的步驟資訊都整理在這一篇，提供未來健忘的我以及路過的人們參考。
header-img: "../../../../img/header_img/bg.png"
tags:
    - AWS
    - EC2
    - Node
    - deploy
    - 部署
    - 教學
    - 筆記
---

續前一篇所提到的部署平台選擇，雖然網路上可以找到蠻多寫得很仔細的教學，但整個流程還是需要從不同的文件查找，所以就汲取每一篇的精華整理在一起，不管是官方文件、其他人的分享等等，並利用自己的話紀錄「在AWS EC2部署Node.js網頁應用程式的步驟」，分成以下幾大項：
* AWS EC2部署
    1. 註冊與Launch Instance
    2. IP綁定與登入主機
    3. 在主機上設定Node.js環境
    4. 開始運作Node.js網頁應用
* Troubleshooting
* 後記
* References

***

## AWS EC2部署

### 註冊與Launch Instance

1. 註冊AWS帳號與認證
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/gElEfHeJS92ysNb8sN7B_001.png">
首先當然需至[AWS](https://aws.amazon.com/tw/)註冊一個帳號，填完基本資料後，會需要完成信用卡認證以及電話認證。

需要注意的是電話認證的部分，在請系統打電話給你時，需注意用`清楚的英文念出四位PIN碼`，即可認證通過(一開始用keypad輸入試了好幾次都失敗，最後還超過了驗證次數)，最後選擇免費方案即完成註冊。

2. 進入主控台後，在左上角的Services中選擇EC2
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/2qM9nOYDRMOZ6uuP4a8b_002.png">

3. 在EC2主頁處(EC2 Dashboard)，選擇`Launch Instance`

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/B2ifMovVQk23naxOOI2o_003.png">

在Launch前稍微提一下，可以從右上角的地方選擇Tokyo，這樣在台灣的[反應時間會比較快](http://www.vedfolnir.com/aws-amazon-web-services-location-choice-7368.html)。

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/v3dNytJcQt2QRezM6RTM_004.png">

4. 選擇作業系統，這裡使用`Amazon Linux AMI 64-bit`

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/TEZmtpwQ5KtZo7W6dDge_005.png">

5. 選擇主機類型，使用免費方案`t2.micro`

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/XgrMGlowSeOefaYJq7sQ_006.png">

6. 其他設定

* Add Tags，可以自行增加tag方便管理

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/ISZDht7FTfuN1KIqdP0x_007.png">

* Configure Security Group，加入SSH/HTTP/HTTPS的設定如下：

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/j610JezeSWaQvsAK2npx_008.png">

這裡HTTP(80)與HTTPS(443)兩個port的Source都打開為`Anywhere`(::/0為IPV6的格式)，這樣才能讓全世界連進來;至於SSH(22)這個是管理者自己在後台開發所使用的port，所以我將它設為`My IP`，至於怎麼設定比較安全需要研究下。


7. Review與下載keypair
最後Review確定設定無誤，按下Launch。此時會跳出keypair的視窗，這是等等需要拿來登入後台的key file，選擇`Create a new key pair`，並輸入你想取的名字並下載保存好，再按下`Launch Instances`即完成launch的步驟。

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/h3HSNNLdTiumdIFnUpOR_009.png">

### IP綁定與登入主機

1. Allocate新IP

* 點一旁的`Elastic IPs`，並點選`Allocate new address`
<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/QdnpL6GhTZSe7sUa9mIc_010.png">

* 再點選`Allocate`即可得到一個IP如下示意：

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/j0f5xwotROiV1hELoUDV_011.png">

2. Associate address到Instance

* 對著新建IP按右鍵後，點選Associate address

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/ltKkdIk6RCW3Nvqz6UmG_012.png">

* 選擇剛剛建立的Instance後，點選Associate即完成IP綁定

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/ZS5XrQhQeOLxbS1qFJYx_013.png">


3. 回到左方Instances標籤，即可看到全部設定的review
以我的deztwblog為例，此時複製剛剛綁定的`Public IP`：

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/tTETSUVhQGuOTHstsvc7_014.png">

4. 打開terminal，輸入command後即可登入主機

```
ssh -i [key.pem] -l ec2-user [your public IP]
```

其中`key.pem`使用你剛剛下載下來的key file絕對路徑，`your public IP`使用上一步複製的IP，`ec2-user`為預設登入帳號

5. 其他設定
* 建議可以在`.bashrc`(或像我是用`.zshrc`)中加入alias，即可使用指令快速登入

```
alias sshAWS='ssh -i [key.pem] -l ec2-user [your public IP]'
```

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/Grj8Q7tRGCGm8J13JUXA_015.png">

* 也可以從Filezilla登入直接從local端傳檔案

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/9ojSN3KUTTist2b176Cc_017.png">

### 在主機上設定Node.js環境

(建議可以參考[AWS Tutorial](http://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html))

1. 由於之後會使用screen來常駐執行server，所以以下全程使用root來安裝

```
sudo -i
```

2. 安裝NVM
建議是使用NVM來安裝Node.js，如此還可方便做Node版本切換

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
```


3. 使用以下command啟動NVM

```
. ~/.nvm/nvm.sh
```

4. 安裝你所需要的Node版本

* 這裡可以先在本機查看自己local開發的node version

```
node -v
```

* 再到EC2主機安裝你所需要的版本

```
nvm install 6.10.0
```

5. 使用以下指令確認安裝成功與版本

```
node -e "console.log('Running Node.js ' + process.version)"
```

6. 可能需要的其他設定

* 檢查更新

```
sudo yum update
```

* 安裝git

```
sudo yum install git
```

* 在AWS EC2主機上設定`.bashrc`(optional)

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/cAE7B1JpQgm3gMvHRdj4_016.png">

### 開始運作Node.js網頁應用

1. 上傳專案source code到EC2主機上
這邊我是直接從Github上將自己的code git clone下來，或者你也可以利用FTP來上傳code。

2. 執行screen，主機上預設是有安裝screen

```
screen
```

3. 執行你的Node主程式

```
node index.js
```

`ctrl + a`然後`d`來做screen的detach後，即可讓自己的網站應用always on，不過感覺上用screen不太像是標準的作法，或許還有更好的方法。

4. 到網頁輸入你的public IP，即可看到成果

<img class="center" src="http://user-image.logdown.io/user/25993/blog/24964/post/1679358/M3l7RB4iTymmmFd2Wsfo_018.png">

## Troubleshooting

* 針對AWS Security Group設定如何做比較安全？
* 在EC2主機上run `node index.js`時，除了用screen有什麼更好的做法？


## 後記
在EC2部署完後，算是正式讓這個作品上線了，試著將logdown的文章貼過去，圖片的展示卻是未達「開發者體驗」的滿意標準，更別說UX了，這時又覺得logdown真的是應有盡有。

不過所謂萬事起頭難，既然已經起了個頭，讓它漸入佳境也只是時間上的問題，想想Facebook也是[隨著時間成長](https://www.quora.com/How-has-Facebooks-UI-changed-over-time)來著，接下來就是把Dez.tw用React重構了。


## References
* [Amazon Web Services AWS 伺服器機房的區域選擇](http://www.vedfolnir.com/aws-amazon-web-services-location-choice-7368.html)
* [Tutorial: Setting Up Node.js on an Amazon EC2 Instance](http://docs.aws.amazon.com/zh_cn/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)
* [[教學]十分鐘快速建立Amazon EC2免費主機](https://blog.json.tw/teaching-ten-minutes-to-quickly-build-a-free-amazon-ec2-host)
* [在Amazon EC2 上部署node.js應用](http://fanli7.net/a/bianchengyuyan/C__/20130126/297941.html)


## 5/4補充
今天突然收到AWS寄Bill給我，其中有兩項被收費：
* `$0.05 per GB-Month of snapshot data stored - Asia Pacific (Tokyo)   1.058 GB-Mo $0.05`
參考[這篇](https://aws.amazon.com/premiumsupport/knowledge-center/snapshot-in-use-error/)把快照刪除，希望下個月不會再收到Bill...

* `$0.005 per Elastic IP address not attached to a running instance per hour (prorated)    568.400 Hrs $2.84`
參考[這篇](http://stackoverflow.com/questions/12913278/keeping-ec2-free-instances-free)，原來是有多申請一個閒置的靜態IP，結果忘了release，就被白白收費了！
