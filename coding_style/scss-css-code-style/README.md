# F2E SCSS & CSS Code Style
*前端團隊 Scss/Css 程式碼參考準則*

## 目錄

1. [常用名詞](#常用名詞)
1. [可閱讀性-Readability](#可閱讀性)
1. [模組化-Module](#模組化)
1. [可擴展化-Scalable](#可擴展化)
1. [降低重複的代碼-DRY](#降低重複的代碼)


## 常用名詞

* 一條樣式描述(A declaration)
```css
  background-color: red
```    

* 一個樣式描述塊(A declaration block)
```css
{
  background-color: red;
  background-style: none;
}
```       

* 一條規則(A ruleset or rule)
```css   
div p, #id:first-line {
  background-color: red;
  background-style: none;
}
```

## 可閱讀性

連字號
```css
// ✓ OK
<div class="header-container"> ... </div>

// ✗ avoid 
<div class="headerContainer"> ... </div>
```

空格

* 一組選擇器和括號之間
* 一組選擇器裏頭有多個選擇器時，空一格在逗號和下個選擇器中間

換行

* 開頭的左中括號和第一條聲明之間
* 結尾的右中括號，建議換一行
* 兩個CSS規則中間，建議換一行

```css
header {
  color: $primary-color;
}

div p, #id:first-line {
  background-color: $primary-color;
  background-style: none;
}
```

命名用字以可讀性高為主


## 模組化

#### 檔案結構與檔案放法

1. 底線 : 要被引用的檔案前面加底線，並歸類在相關的資料夾

2. 檔案分類

* layout/ or Site/ : 布局樣式，存放網站主體區塊(頭部、尾部、導航欄、側邊攔...)的樣式表、網格系統、或表單

* base/ or Shared/ : 放置一些檔案定義一些HTML元件公認的標準樣式，以及局部元件的樣式

* 頁面命名/ : 放置頁面獨特樣式

* variables/ : 變數檔案

#### 引用檔案置於文件開頭

* 一個@import引用一個文件

* 一個@import單獨一行

* 可以忽略文件副檔名和底線前缀

```scss
@import "../base/variables";
@import "../base/button_mixins";
```

#### 同類型的規則放置在同一個檔案，基礎樣式先放，若有客製化樣式，考慮開一個新的檔案，引用檔案時，基礎樣式表先引用。


## 可擴展化

使用 SCSS 巢狀結構，盡量不包多於四層
```scss
// ✓ OK
.confirm-button-with-unique-image {
  span {
    &::before {
      background-image: url('.../unique.jpg'); 
    }
  }
}

// ✗ avoid 
.product-page {
  .header {
    .button.confirm {
      span {
	&::before {
	  background-image: url('...'); 
	}
      }
    }
  }
}
```

盡可能不使用權重過重之 !important、行內樣式(inline-style) 等，避免難以修改
```css
// ✓ OK
display: none;

// ✗ avoid 
display: none !important;
```
```html
// ✗ avoid 
<div style="display: none;">...</div>
```

## 降低重複的代碼

強烈建議合併有相同的樣式敘述的選擇器
```css
// ✗ avoid 
.navigation li {
  color: #333;
}

.navigation li a {
  color: #333;
}

// ✓ OK
.navigation li {
  &, a {
    color: #333;
  }
}
```
