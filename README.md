# map

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

# geometryFunction
> 默认的绘制类型只有四种类型，如果我想绘制一个矩形框呢？当然有一个拉框交互可以实现这个效果，这里我们使用 draw 交互来实现拉框的效果，这个要结合 maxPoints 加以限制。

> 该函数有两个参数，一个是坐标集合、一个是勾绘的 geometry 类型，返回构造好的 geometry 对象，用来初始化要素。如下，我们给 draw 增加 geometryFunction 参数和 maxPoints 参数：