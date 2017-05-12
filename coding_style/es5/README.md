# F2E Javascript ES5 Code Style 
*前端團隊 Javascript ES5 程式碼參考準則*

## 目錄

1. [命名規則](#命名規則)
1. [空白](#空白)
1. [字串](#字串)
1. [函式](#函式)

## 命名規則

所有的變數皆採用小駝峰式命名法（lower camel case）
* 第一個單字以小寫字母開始；第二個單字的首字母大寫，例如：firstName、lastName。
* 單字語意化，可以用變數名稱去解釋撰寫的程式，不需用註解特別說明。

一般型別

* 字串 **(string)**
* 數字 **(int)**
* 布林 **(boolean)**

```js
// ✓ OK
var firstName = 'Crab';
var lastName = 'Lin'
var age = 35;
var online = false;

// ✗ avoid 
var ProductName = 'Book 1';
// ✗ avoid 
var member1 = 'Millie';
var member2 = 'Billy';
```

物件型別特殊規範
* 物件型別的參數名，可以因為後端傳回的數值，不統一為小駝峰式命名法，若前端程式自建自用則需遵守準則

物件型別

* 陣列 **(array)**
* JSON

```js
var list = [1, 2];
var product = {
    name: 'Javascript Guildline',
    originalPrice: 100,
    salePrice: 50
};
```
函數命名也採用小駝峰式命名法（lower camel case）
* 第一個單字儘量使用程式撰寫時常用動詞來說明此函數的使用情境，例如：get, set, post, delete, init。
* 第二個單字使用名詞去表明處理的對象，例如：initialAccount，表示此函式主要是初始帳號物件。
* 函式語意化重於命名，若此功能使用不常用的單字讓人可以更快速的瞭解程式碼，則可討論後使用。

* 函數 **(function)**

```js
// ✓ OK
function getProducts() { ... } 
function updateProducts() { ... }

// ✗ avoid 
function sellBook() { ... }
function buyGoods() { ... }
```

## 空白

空白的使用時機及規則
* 關鍵字後： **if, for, do while, switch**
* 中綴字前後

## 字串

* 字串： **單引號**

```js
// ✓ OK
console.log('hello there');
$('<div class="box"></div>').apppendTo('body');

// ✗ avoid
$(".box").apppendTo('body');
```

JSX reander的HTML標籤的Attribute，一律使用雙引號
```jsx
// ✓ OK
render() {
    return (<div class="box">Crab</div>);
}

// ✗ avoid
render() {
    return (<div class='box'>Millie</div>);
}
```
