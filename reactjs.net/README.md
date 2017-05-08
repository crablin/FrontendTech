# ReactJS.NET Architecture
*ReactJS.NET 運作及架構說明*

## 目錄

1. [初始建置](#初始建置)
1. [編譯及打包](#編譯及打包)
1. [編譯及打包](#編譯及打包)
1. [參考](#參考)

## 初始建置

建立 MVC Web Application 專案
![Alt text](/reactjs.net/1-1.png)

可自行決定是否要為空白專案或基礎專案
![Alt text](/reactjs.net/1-2.png)

安裝 ReactJS.NET NuGet
請依自己的 MVC 版本安裝相對應的 package(目前專案使用的是MVC4)
![Alt text](/reactjs.net/1-3.png)

或者直接執行 NuGet 指令
```
PM > Install-Package React.Web.Mvc4
```

安裝完上述的package即可開始撰寫 React

## 編譯及打包

若會使用到 Babel 轉譯 ES6 就需要額外安裝 NuGet package
```
PM > Install-Package System.Web.Optimization.React
```
安裝package後，可以撰寫下列程式，使用 BabelBundle 需要的 JSX
```cs
using System.Web.Optimization;
using System.Web.Optimization.React;

namespace ReactWebsite
{
    public static class BundleConfig
	{
        public static void RegisterBundles(BundleCollection bundles)
        {
            // 原本 bundle 程式碼 ...

            bundles.Add(new BabelBundle("~/bundles/site")
                Include(".../site.jsx");
        }
    }
}
```
上述語法，JSX 會使用 Babel 編譯並壓縮、醜化，交給 cshtml 引用。
```cs
@Scripts.Render("~/bundles/site")
```


## 參考

* [ReactJS.NET Tutorial](https://reactjs.net/getting-started/tutorial_aspnet4.html)