# EIT F2E Bundler 
*東森訊息科技前端團隊 CSS與JavaScript 打包壓縮方式*

## 目錄

1. [目的](#目的)
1. [打包方式](#打包方式)
1. [安裝工具](#安裝工具)
1. [打包設定檔](#打包設定檔)
1. [打包JavaScript](#打包javascript)
1. [打包JSX](#打包jsx)
1. [編譯與打包樣式](#編譯與打包樣式)

## 目的
透過打包與壓縮，讓各頁面只載入所需要的Style與JavaScript，並將多個不同檔案打包為單一檔案，減少Http Request數量並增進網頁讀取效能。

## 打包方式
打包方式分為: MS Build 與透過 Web Compiler 進行打包與壓縮。
目前現有的前端專案，多使用 MS Build，CSS 部份則是透過 Web Compiler 將 SCSS 檔案轉譯為 CSS 檔後，再透過 MS Build 進行打包。

## 安裝工具
1. [Web Essential](http://vswebessentials.com/)
1. [Web Compiler](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.WebCompiler)

## 打包設定檔
1. 位於: App_Start/BundleConfig.cs
1. 分為三大區塊，分別處理Style, JavaScript與JSX的Bundle
1. 記錄要如何Bundle檔案

![Alt text](/bundler/1-1.png)

### 打包JavaScript
1. 於JsBundleCollection中，新增要Bundle的檔案集合
```cs
// BundleConfig.cs

internal class JsBundleCollection
{
  // YourBundleName: 為這次要Bundle的目的，通常為頁面的名稱或功能的名稱
  internal static string[] YourBundleName =
  {
    // YourFileStoragePath: 檔案存放路徑。
    // YourFileName.js: 檔案的名稱，需包含副檔名。
    // 如果有多個檔案要一起Bundle，則使用「,」隔開。
    "~/Scripts/YourFileStoragePath/YourFileName.js",
    "~/Scripts/YourFileStorePath2/YourFileName2.js"

  };
}

```

2. 在「#region ScriptBundle」與對應的「#endregion」中，新增下面的範例

```cs
// BundleConfig.cs

#region ScriptBundle

// 使用ScriptBundle進行打包
// YourBundleKey 是要導入網頁(cshtml)中所使用的Key，
// 通常命名方式為「~/bundles/頁面或功能名稱」
bundles.Add(new ScriptBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName));

// 如果要串接多個打包集合 資料, 
// 可以透過 .Include 串接 JsBundleCollection 中不同的檔案路徑組合
bundles.Add(new ScriptBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName)
  .Include(JsBundleCollection.YourBundleName2));

#endregion

```

3. 在*.cshtml中引用已Bundle的檔案
```cs
// *.cshtml

// 在@Scripts.Render中指定於2.所設定的「YourBundleKey」
@Scripts.Render("YourBundleKey")


```


### 打包JSX
1. 於JsxBundleCollection中，新增要Bundle的檔案集合

```cs 
// BundleConfig.cs

internal class JsxBundleCollection
{
  // YourBundleName 為這次要Bundle的目的，通常為頁面的名稱或功能的名稱;
  internal static string[] YourBundleName =
  {
    // YourFileStoragePath: 檔案存放路徑。 
    // YourFileName.jsx: 檔案的名稱，需包含副檔名。
    // 如果有多個檔案要一起Bundle，則使用「,」隔開。
    "~/Scripts/YourFileStoragePath/YourFileName.jsx",
    "~/Scripts/YourFileStorePath2/YourFileName2.jsx"
  };
}
```

2. 在「#region ReactBundle」與對應的「#endregion」中，新增下面的範例
```cs
// BundleConfig.cs

#region ReactBundle

// 使用BabelBundle進行打包
// YourBundleKey 是要導入View中所使用的Key，
// 通常命名方式為「~/bundles/頁面或功能名稱」
bundles.Add(new BabelBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName))

// 如果要串接多個bundle資料, 
// 可以透過.Include串接JsBundleCollection中不同的檔案路徑組合
bundles.Add(new BabelBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName)
  .Include(JsBundleCollection.YourBundleName2))

#endregion

```

3. 在*.cshtml中引用已Bundle的檔案
```cs
// *.cshtml
// 在@Scripts.Render中指定於2.所設定的「YourBundleKey」
@Scripts.Render("YourBundleKey")

```

### 編譯與打包樣式
這裡分為兩大步驟: 1. 編譯; 2. 打包。
SCSS 檔案在 Bundle 之前，要先透過「Web Compiler」將 SCSS 檔編譯成 CSS 檔案

#### 編譯 Style
1. 於 SCSS 檔案上按右鍵 > Web Compiler > Compile File
![Alt text](/bundler/1-3.png)
1. 刪除預設產生的css與min.css
![Alt text](/bundler/1-4.png)
1. 開啟compilerconfig.json
1. 找到方才加入的scss檔案，並修改outputFile路徑到Content目錄下
```json
// compilerconfig.json

// 原本
{
  "outputFile": "Content/scss/test.css",
  "inputFile": "Content/scss/test.scss"
}

// 修正後
{
  "outputFile": "Content/test.css",
  "inputFile": "Content/scss/test.scss"
}
```
5. 在「compilerconfig.json」上按右鍵 > Web Compiler > Re-compile all files。
6. 會依所設定的輸出路徑，重新產生css與min.css檔案

#### 打包Style
1. 在「#region StyleBundle」與對應的「#endregion」中，新增下面的範例
```cs
// BundleConfig.cs

#region StyleBundle

// 使用 StyleBundle 進行打包
// YourBundleKey 是要導入網頁中所使用的Key，
// 通常命名方式為「~/Content/頁面或功能名稱」
// 這裡加入的檔案是已編譯後的css檔，而不是scss檔
bundles.Add(new StyleBundle("YourBundleKey").Include(
            "~/YourFileStoragePath/YourFileName.css",
            "~/YourFileStoragePath/YourFileName2.css"));

#endregion

```

2. 在*.cshtml中引用已Bundle的檔案
```cs
// *.cshtml
// 在@Scripts.Render中指定於2.所設定的「YourBundleKey」
@Styles.Render("YourBundleKey")

```