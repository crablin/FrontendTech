# React Base
*React 基本設計理念*

## 目錄

1. [撰寫方針](#撰寫方針)
1. [生命周期](#生命周期)
1. [參考](#參考)

## 撰寫方針

### 前言

React Component 主要設計理念是依 **「資料」** 產生 **「畫面」**。

簡單來說，當資料建立時，應該要有相對應的顯示，當資料更改時也需要重新繪製畫面。

所以 React 的程式碼撰寫，會以更新動作為主，去額外撰寫動作之前及之後，在限定的function method去撰寫你需額外執行的內容。

在 [生命周期](#生命周期) 的主題中，會再次介紹如何界定撰寫的內容。

### 第一步

你準備好要顯示的區塊名稱，例如：我要做一個商品清單
名稱我們使用 ProductList
* 命名首字一定要**大寫**
* render 出一個空白的div 

``` jsx
// ProductList.jsx
var ProductList = React.createClass({
    render () {
        return (<div></div>);
    }
});
```

照上述的做法，你可得到一個名為 ProductList 的 Component

### 第二步

我們需要一個一個的商品，需要一個 Component 去顯示單一商品的樣式
名稱我們使用 ProductItem
* 再次記得首字需要 **大寫**
* 建一個空白的ProductItem
* 預設Product要顯示的資料呢，使用 **getDefaultProps** 

```jsx
// ProductItem.jsx
var ProductItem = React.createClass({

    getDefaultProps () {
        return {
            id: 1,
            name: 'Product',
            price: 100,
            imageUrl: '100001.jpg'
        }
    },

    render () {
        return (<div></div>);
    }
})
```

* 套入預設資料，修改一下 ProductItem render 的結果
* HTML 要設定 CSS class 一定要使用 **className** 
```jsx
var ProductItem = React.createClass({

    getDefaultProps () {
        return {
            id: 1,
            name: 'Product',
            price: 100,
            imageUrl: '100001.jpg'
        }
    },

    render () {
        return (
            <div className="productItem">
                <img src={this.props.imageUrl} />
                <div className="productName">{this.props.name}</div>
                <div className="productPrice">{this.props.price}</div>
            </div>
        );
    }
})
```

## 第三步

有列表(ProductList)及項目(ProductItem)的 Component，再來調整列表的Component，來決定列表需要顯示多個項目。這裡我們需要瞭解**props**與**state**的差別：

* **props** : 外部賦予的資料，不可從內部變數的資料
* **state** : 內部動作賦予的資料(如：ajax)，可變動的資料

這裡設計的方式是透過ajax取得資料後，改變state以顯示商品清單，所以使用 **getInitialState**

* 先預設商品清單的資料

```jsx
// ProductList.jsx
var ProductList = React.createClass({

    getInitialState () {
        return {
            products: [
                // 預設商品資料
                {
                    id: 1,
                    name: 'Product',
                    price: 100,
                    imageUrl: '100001.jpg'
                }, {
                    id: 2,
                    name: 'Product2',
                    price: 200,
                    imageUrl: '100002.jpg'
                }
        
            ]
        };
    },

    render () {
        return (<div></div>);
    }
});
``` 

* 新增 **renderProductItems**
* 修改 render 商品清單

```jsx
// ProductList.jsx
var ProductList = React.createClass({

    getInitialState () {
        return {
            products: [
                // 預設商品資料
                {
                    id: 1,
                    name: 'Product',
                    price: 100,
                    imageUrl: '100001.jpg'
                }, {
                    id: 2,
                    name: 'Product2',
                    price: 200,
                    imageUrl: '100002.jpg'
                }
        
            ]
        };
    },

    renderProductItems() {
        var result = this.state.products.map(function(product) {
            return (<ProductItems key={product.id} {...product}/>);
        })
    },

    render () {

        var productItems = this.renderProductItems();

        return (<div className="productList">{productItems}</div>);
    }
});
```
測試成功後，再將資料改成用 ajax 的方式取得商品清單資料。 

* 這裡需小心，當ajax success callback 時，this 並不代表這個ProductList Component，可以使用bind(this)，建立function的初始this參數，方可使用 **this.setState(...)**。

```jsx
// ProductList.jsx
var ProductList = React.createClass({

    getInitialState () {
        return {
            products: []
        };
    },

    getProducts() {
        $.ajax({
            type 'POST',
            url: '/products/get',
            dataType: 'json',
            success: function(json) {
                this.setState({products: json});
            }.bind(this)
        })

    },

    renderProductItems() {
        var result = this.state.products.map(function(product) {
            return (<ProductItems key={product.id} {...product}/>);
        });
    },

    render () {

        var productItems = this.renderProductItems();

        return (<div className="productList">{productItems}</div>);
    }
});
``` 

設定 ajax 讀取資料的時機，通常我們會放在 componentDidMount。主要是因為當建置完初始 component 時，再擷取新的資料後再更改資料重繪，較不會發生同時繪製的問題。

```jsx
// ProductList.jsx
var ProductList = React.createClass({

    getInitialState () {
        return {
            products: []
        };
    },

    componentDidMount() {
        this.getProducts();
    },

    getProducts() {
        $.ajax({
            type 'POST',
            url: '/products/get',
            dataType: 'json',
            success: function(json) {
                this.setState({ products: json });
            }.bind(this)
        })

    },

    renderProductItems() {
        var result = this.state.products.map(function(product) {
            return (<ProductItems key={product.id} {...product}/>);
        });
    },

    render () {

        var productItems = this.renderProductItems();

        return (<div className="productList">{productItems}</div>);
    }
});
``` 

依上述描敘就可以看到商品清單，完成最簡單的資料即顯示的部分。


## 生命周期

React Component 的生命周期可以先分成三個部分

* Mount - 建置
* Update - 更新
* Unmount - 解構

![Alt text](/react_base/1-1.jpg)

### Mount 
當Component被建置時，會執行下列的流程：
建置動作每一個套件都只會執行一次，所以可以將數值初始及建置事件一併講解

* getDefaultProps
    - !!! **在這個階段，不能使用setState** 
* getInitialState
    - !!! **在這個階段，不能使用setState**
    - !!! **這個階段可以讀取Props的資料，可以用外部預設資料更新**
* componentWillMount
* render
    - !!! **在這個階段，不能使用setState** 
    - 依初始資料繪製畫面
* componentDidMount
    - ajax 取資料完成回寫 State
    - 設定額外 Event Handler 

## Update
當State或Props的資料被更新後，都會執行下列的流程：

* componentWillReceiveProps(nextProps)
    - 通常不太會使用
* shouldComponentUpdate(nextProps, nextState)
    - !!! **不太推薦在這使用setState**
* componentWillUpdate(nextProps, nextState)
    - !!! **不太推薦在這使用setState**
* render
* componentDidUpdate(prevProps, prevState)

## Unmount
當Component被移除前，僅執行下列的流程：

* componentWillUnmount
    - 移除額外設定的 Event Handler 

## 參考

* [Understanding React.js](https://staminaloops.github.io/undefinedisnotafunction/understanding-react/)
