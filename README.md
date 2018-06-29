# 基于vue 开发的 大转盘 小游戏 demo

> A Vue.js project

## 介绍

- 首先确定九宫格的dom排列顺序，达到正确的 环形 运动轨迹
- 运动开始时，发送获奖的接口，同时缓慢加速，达到一定速度后，固定运行（设置运行最少圈数值）当接口返回，调取减速的方法，使动画速度越来越慢，达到最低速度时，运行一定圈数，然后选定停下来的模块点


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

## 有问题？

Welcome PR or Issue！


## 许可

MIT &copy; [ivan](https://github.com/ivanYF)
