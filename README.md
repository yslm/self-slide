# self-slide

> 这里面包含了两种滑动方式
- 一种是通过translate直接移动，还有一种是通过设置元素的scrollLeft来达到滑动的效果
- 这里需要注意的是，直接translate的方式移动元素，可以通过设置过渡时间来实现动画效果
- 通过scrollLeft的方式的话，没有动画效果，需要封装一个函数，不停的增加scrollLeft的值来达到模拟动画的效果

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
