# F2E Project Architecture
*前端技術架構及應用說明*

## 目錄
1. [ReactJS.NET](reactjs.net/)
1. [UnitTest](unit_test/)
1. [外部套件](#外部套件)

## 主要目的
### ReactJS.NET
* 將前端程式建置整合在.NET Project，後端也可以運行前端架構並共同合作
* 運用 .NET MVC 的好處，Route 路徑轉導的工作交給原生的 MVC 
* 支援 Isomorphic Javascript，便於SEO及後端 model 介接調整

### UnitTest
* 檢測單元，讓每一個component 維持高品質的正確性
* 使用 jest 測試 React components
* 導入 storybook 建置 component reference site

### 外部套件
目前專案中，為節省開發時間，常用的外部件如下:
* [jQuery 2.2.0](https://code.jquery.com/jquery/)
* [jQuery Validation Plugin](https://jqueryvalidation.org/)
* [Lodash 4.6.1](https://lodash.com/)
* [React-Slick](https://github.com/akiran/react-slick)
* [classNames](https://github.com/JedWatson/classnames)
* [Reflux](https://github.com/reflux/refluxjs)