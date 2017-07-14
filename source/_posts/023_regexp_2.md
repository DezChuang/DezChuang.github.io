---
title: 筆記｜Javascript正規表達式#2 - 基本語法
catalog: true
date: 2017-07-12 15:31:25
subtitle: 筆記所有常用的正規表達式符號說明，理解後用自己的方式整理下來。
header-img: "../../../../img/header_img/bg.png"
tags:
    - Javascript
    - RegExp
    - Regular Expression
    - 正規表達式
    - 筆記
    - 教學
    - 前端
---


## 創建正規表達式
有兩種方法，分別應用在不同的使用情境：

| 種類 |  用途    |
| ----| -------- |
| `var re = /ab+c/;` |  一般最常用的方法，將正規表達式寫在包在`/.../`中，適用於正規表達式為定值時，可獲得較佳效能。 |
| `var re = new RegExp("ab+c");` |  呼叫RegExp物件建構子函數即時編譯正規表達式，適用於事先未知匹配模式或將從其他地方取得值。|

---

## 字元類別 (Character Classes)

字元類別匹配任何包含於`[...]`內的字元：

| 正規式 | 說明 |
| ---| --- |
| `/[dez]/` | 匹配d、e、z字元|
| `/[0-9]/` | 匹配0到9之間所有數字  |
| `/[^0-9]/` | 匹配0到9以外的字元(`^`寫在pattern中間時表示否定) |
| `/[a-z]/` | 匹配a到z之間所有字母  |
| `/[a-zA-Z0-9]/` | 匹配所有大小寫字母與數字0到9  |


其他常用的特定表示字元類別：

| 字元 | 說明 |
| ---| --- |
| `.` | 除了newline與end of line外所有字元|
| `\w` | 意同`[a-zA-Z0-9_]`，任一字母、數字、與`_`  |
| `\W` | 意同`[^a-zA-Z0-9_]`，`\w`以外任何字元 |
| `\d` | 意同`[0-9]` |
| `\D` | 意同`[^0-9]`|
| `\s` | 任何Unicode空白字元，包含`\t`、`\v`等  |
| `\S` | 任何非Unicode空白字元 |

---

## 重複次數 (Repetition)

使用在需限制一定範圍的次數時，使用以下語法能節省一直重複寫同一個字元類別的麻煩：

| 字元 | 說明 |
| ---| --- |
| `{n}` | 匹配前一個規則的字元剛好n次|
| `{n,}` | 匹配前一個規則的字元n次以上  |
| `{n, m}` | 匹配前一個規則的字元在n到m之間 |
| `+` | 意同`{1, }`，一個以上 |
| `*` | 意同`{0, }`，0個以上|
| `?` | 意同`{0,1}`，0個或1個  |

另外還有一個進階的寫法是Non-greedy Repetition，將重複次數後面再加一個問號如`+?`, `*?`, `??`，由於不常見寫在後面的[補充](https://dezchuang.github.io/2017/07/12/023_regexp_2/#補充)。

---

## 旗標 (flag)

正規表達式有幾個選擇性的flag可以使用，寫在`/.../`後面，例如`/xyz[0-9]/g`，以下列出常用的，可以個別或搭配使用：

| 種類 |  用途    |
| ----| -------- |
| g   |  Global search，比對字串中所有符合的配對，否則只找一個，見下例。 |
| i   |  case-Insensitive search，規則不區分大小寫。|
| m   | Multi-line search，多行比對，`^`比對每行開頭，`$`比對每行結尾。 |

```javascript
var str = "xyz123xyz456";
str.match(/xyz[0-9]/); // 符合 xyz1
str.match(/xyz[0-9]/g); // 符合 xyz1 和 xyz4
```

---

## 錨點元字符 (Anchoring Metacharacters)

中文照翻，我也不知道什麼是元字符，不過錨點就比較好理解，使用錨點符號可以決定匹配規則必須在特定的位置，簡單地說就是「設定規則的邊界」：

| 字元 | 說明及範例 |
| ---| --- |
| `^` | 在字串開頭，或m旗標狀態下每行開頭  |
| `$` | 在字串結尾，或m旗標狀態下每行結尾  |
| `\b` | 比對word(`\w`)邊界，詳細見[補充](https://dezchuang.github.io/2017/07/12/023_regexp_2/#補充)  |
| `\B` | 比對non-word(`\W`)邊界，詳細見[補充](https://dezchuang.github.io/2017/07/12/023_regexp_2/#補充)  |
| `x(?=y)` | x符合的條件要加上x之後必須接y，詳細見[補充](https://dezchuang.github.io/2017/07/12/023_regexp_2/#補充)  |
| `x(?!y)` | x符合的條件要加上x之後不可接y，詳細見[補充](https://dezchuang.github.io/2017/07/12/023_regexp_2/#補充)  |

---

## 分組

| 字元 | 說明及範例 |
| ---| --- |
| `(x)` |找出x，並且記憶此匹配到結果中  |
| `(?:x)` | 找出x，但此匹配不會被記憶到結果中  |

`(x)`就像四則運算中使用的括號，括起來的部分為同一組，例如`/(dog|cat){2}/`用來匹配出現兩次`dog`或`cat`的字串，利用括號也能提高可讀性。

而`(?:x)`的用途就是只是判斷是否符合x條件，但不會記住判斷的字，例如判斷電話號碼與分機：

```javascript
var str="12345678#123";
var result = /(\d+)(?:#)(\d+)/.exec(str);
console.log(result[1]); // 123445678
console.log(result[2]); // 123
```

---

## 其他

| 字元 | 說明及範例 |
| ---| --- |
| `\` | 有特殊意義的符號如果要匹配自己必須加上反斜線。  |
| `|` | or 運算子，`x|y`指匹配x或y都可以。  |


剩下其他還有許多比較少見的符號，之後有碰到再補充吧。

---

## 補充

### 非貪進重複 (Non-greedy Repetition)
看起來不是很常用的寫法，不過因為跟重複次數有關，補充進來，直接參考以下[例子](https://stackoverflow.com/questions/3075130/what-is-the-difference-between-and-regular-expressions/3075150#3075150)

有兩正規表達式`A.*Z`與`A.*?Z`，給予一個字串`eeeAiiZuuuuAoooZeeee`。匹配的結果如下：

* `A.*Z`得到結果`AiiZuuuuAoooZ`。[Demo](http://www.rubular.com/r/qM00Ovbvdr)
* `A.*?Z`得到結果`AiiZ`與`AoooZ`。[Demo](http://www.rubular.com/r/Dv168cXzeX)

一般的`.*`會讓`.`一直無限制地去找字元，找到字尾時`...Zeeee`發現沒有匹配到Z，此時就會一個一個往回比對，直到找到Z為止，所以得到結果為`AiiZuuuuAoooZ`。

```
eeeAiiZuuuuAoooZeeee
   \_______________/
    A.* matched, Z can't match
    
eeeAiiZuuuuAoooZeeee
   \______________/
    A.* matched, Z still can't match
    
eeeAiiZuuuuAoooZeeee
   \__________/
    A.* matched, Z can now match
    
eeeAiiZuuuuAoooZeeee
   \___________/
    A.*Z matched
```


而非貪進的`.*?`找法是，會先匹配盡可能少的重複次數`.`，但如果找不到匹配的字元才會再往回找，也稱為Reluctant(不情願的) Repetition。

```
eeeAiiZuuuuAoooZeeee
   \__/r   \___/r      r = reluctant(non-greedy, lazy)

```

---

### `\b`與`\B`的差別 ([參考](http://www.rexegg.com/regex-boundaries.html#wordboundary))

簡單來說可以這樣理解：**有`\b`的地方就是不能出現word，有`\B`的地方就是一定要有word**(注意這邊的word指的是`\w`，也就是`[a-zA-Z0-9_]`)。

這個b或B指的是boundary(邊界)，分別代表的是Word Boundary與Not-a-word-boundary，用途就如其名是用來設定word的邊界，直接看例子：

今天有以下字串：

1. "a black `cat`"
2. "`cat`atonic"
3. "tom`cat`"
4. "certifi`cat`e"
5. "_`cat`"
6. "`cat`25"

若規則是`\bcat\b`，指的是cat的前後都不能有[a-zA-Z0-9_]出現，故只有1符合，因為2、4、6後面有word，而3、4、5前面有word。同理可推若規則是`\bcat`，則有1、2、6符合。若規則是`cat\b`，則有1、3、5符合。

而如果看懂`\b`後，`\B`其實就不難了，因為他判斷的邊界就是與`\b`相反，看起來很雞肋，但有時候的確會派上用場，例如之前面試時碰到[幫數字加逗號的例子](https://dezchuang.github.io/2017/06/17/019_interview-coding/)。

所以續上例，`\Bcat\B`，指的是cat的前後都需要有[a-zA-Z0-9_]出現，故只有4符合，因為1、3、5後面沒有word，而2、6前面沒有word。同理可推若規則是`\Bcat`，就是cat前要有word，則有3、4、5符合。若規則是`cat\B`，則有2、4、6符合。

更詳細的說明與定義可以參考標題連結。

---

### `?=`與`?!`的差別 ([參考](http://syunguo.blogspot.tw/2013/04/jsregular-expressions.html))

這個看例子還蠻好懂的：

#### ?=

```javascript
str = "Hello Wendy";
str.replace(/e(?=l)/gi,'x'); // Hxllo Wendy
```

`e(?=l)`就是需要找e後面有接l的，所以只有Hello的e被換掉。

#### ?!

```javascript
str = "Hello Wendy";
str.replace(/e(?!l)/gi,'x'); // Hello Wxndy
```

`e(?!l)`就是需要找e後面沒有接l的，所以只有Wendy的e被換掉。

---

## 系列文章

* [筆記｜Javascript正規表達式#1 - Email驗證](https://dezchuang.github.io/2017/07/05/022_regexp_1/)
* [筆記｜Javascript正規表達式#3 - 字串處理與應用](#)

---

## 參考資料
* 統一整理在[下一篇](#)