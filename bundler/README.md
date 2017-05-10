# EIT F2E Bundler 
*東森訊息科技前端團隊 CSS與JavaScript 打包壓縮方式*

## 目錄

1. [目的](#目的)
1. [打包方式](#打包方式)
1. [安裝工具](#安裝工具)
1. [BundleConfig.cs](#BundleConfig.cs)
1. [JavaScript Bundle](#JavaScriptBundle)
1. [JSX Bundle](#JSXBundle)
1. [SCSS/Style Bundle](#SCSS/StyleBundle)
1. [](#)

## 目的
透過打包與壓縮，讓各頁面只載入所需要的Style與JavaScript，並將多個不同檔案打包為單一檔案，減少Http Request數量並增進網頁讀取效能。

## 打包方式
打包方式分為: MS Build 與透過 Web Compiler 進行打包與壓縮。
目前東森購物小網前端專案，多使用 MS Build，CSS 部份則是透過 Web Compiler 將 SCSS 檔案轉譯為 CSS 檔後，再透過 MS Build 進行打包。

## 安裝工具
1. [Web Essential](http://vswebessentials.com/)
1. [Web Compiler](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.WebCompiler)

## BundleConfig.cs
1. 位於: App_Start/BundleConfig.cs
1. 分為三大區塊，分別處理Style, JavaScript與JSX的Bundle
1. 記錄要如何Bundle檔案

![Alt text](/bundler/1-1.png)



### JavaScriptBundle
1. 於JsBundleCollection中，新增要Bundle的檔案集合
```cs
// BundleConfig.cs
internal class JsBundleCollection
{
  
  // YourBundleName: 為這次要Bundle的目的，通常為頁面的名稱或功能的名稱
  internal static string[] YourBundleName = 
  {
    // YourFileStoragePath: 檔案存放路徑; 
    // YourFileName.js: 檔案的名稱，需包含副檔名。
    // 如果有多個檔案要一起Bundle，則使用「,」隔開。
    "~/Scripts/YourFileStoragePath/YourFileName.js", 
    "~/Scripts/YourFileStorePath2/YourFileName2.js"
  };
}
```

2. 在「#region ScriptBundle」與對應的「#endregion」中，新增下面的範例
```cs
// BundleConfig.cs

// 使用ScriptBundle進行打包
// YourBundleKey 是要導入網頁(cshtml)中所使用的Key，
// 通常命名方式為「~/bundles/頁面或功能名稱」
bundles.Add(new ScriptBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName));

// 如果要串接多個打包集合 資料, 
// 可以透過 .Include 串接 JsBundleCollection 中不同的檔案路徑組合
bundles.Add(new ScriptBundle("YourBundleKey")
  .Include(JsBundleCollection.YourBundleName)
  .Include(JsBundleCollection.YourBundleName2));



```

3. 在*.cshtml中引用已Bundle的檔案
```cs
// *.cshtml
// 在@Scripts.Render中指定於2.所設定的「YourBundleKey」
@Scripts.Render("YourBundleKey")

```


### JSX Bundle
1. 於JsxBundleCollection中，新增要Bundle的檔案集合
```cs 
// BundleConfig.cs

internal class JsxBundleCollection
{
  // YourBundleName 為這次要Bundle的目的，通常為頁面的名稱或功能的名稱;
  internal static string[] YourBundleName = 
  {
    // YourFileStoragePath: 檔案存放路徑; 
    // YourFileName.jsx: 檔案的名稱，需包含副檔名。
    // 如果有多個檔案要一起Bundle，則使用「,」隔開。
    "~/Scripts/YourFileStoragePath/YourFileName.jsx",
    "~/Scripts/YourFileStorePath2/YourFileName2.jsx"
  };
}
```

2. 在「#region ReactBundle」與對應的「#endregion」中，新增下面的範例
```cs
// BundleConfig.cs

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

```

3. 在*.cshtml中引用已Bundle的檔案
```cs
// *.cshtml
// 在@Scripts.Render中指定於2.所設定的「YourBundleKey」
@Scripts.Render("YourBundleKey")

```

### SCSS/Style Bundle
SCSS檔案在Bundle之前，要先透過「Web Compiler」將SCSS檔編譯成CSS檔案
1. 於SCSS檔案上按右鍵 > Web Compiler -> Compile File
![Alt text](/bundler/1-3.png)

2. 刪除預設產生的css與min.css