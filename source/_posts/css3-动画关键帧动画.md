title: css3 动画关键帧动画
author: 校宝在线
date: 2019-03-28 10:41:37
tags: 
- css3
- 帧动画
categories: 
- css3
---

### 网页当中常见的3种动画表现形式:

### 1. css3动画

>优点：浏览器可以对动画进行优化，强制使用硬件加速（通过GPU来提高动画性能）。

>缺点：CSS动画只能暂停,不可以固定动画中某个特定的时间点，不能中间反转动画，不能控制时间的灵活度，不能在某个特殊的位置添加回调函数或是绑定回放事件,没有进度报告。

<!--more-->
### 2. gif动画/flash动画

>优点：容易操作，对计算机的硬件要求低。

>缺点：Flash动画必须要插件来支持，如果没安装插件，浏览器是解析不了的。

### 3. js脚本动画

>优点：灵活度高，可做到对动画的随时控制。
>缺点：动画实现难度比css3动画高，在浏览器的主线程中运行，可能受到其他程序逻辑影响。



## 关键帧动画是什么？
关键帧动画是让元素从一种样式逐渐变化为另一种样式，可以根据需要更改任意数量的CSS属性，次数没有限制。要使用CSS动画，您必须首先为动画指定一些关键帧。关键帧保存元素在特定时间具有的样式。

[示例](https://codepen.io/huanglilijie/pen/XGaaoO)


## 动画的构成
动画由两部分组成

>关键帧 - 定义动画的阶段和样式

>动画属性 - 用 @keyframes 创建动画，把它绑定到一个选择器并且定义如何动画

## 关键帧keyframes

每个keyframes由**动画名称**、**动画阶段**、**css属性**三部分组成

#### 语法

```
@keyframes animationname {keyframes-selector {css-styles;}}
animationname 动画的名字
keyframes-selector动画时长的百分比（0%-100%为有效数值或者from-to）
css-styles;一个或多个合法的 CSS 样式属性
```

我们定义一个简单的关键帧动画，名字为“fandeIn”。它共有两个阶段，从第开始阶段（0%）不透明度0到结束阶段（100%）变化为不透明度为1
```
@keyframes fandeIn{
0%{
opacity:0
}
100%{
opacity:1
}
}
```

## 动画属性
动画属性做两件事：

1. 将@keyframes分配给要动画的元素。
2. 定义它是如何动画的。

以下是@keyframes的所有动画属性：

| 属性 | 描述 |
| ------ | ------ |
| animation | 所有动画属性的简写属性，除了 animation-play-state 属性。 |
| animation-name |  规定 @keyframes 动画的名称。 |
| animation-duration | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。 |
| animation-timing-function | 规定动画的速度曲线。默认是 "ease"。 |
| animation-fill-mode | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
| animation-delay | 规定动画何时开始。默认是 0。 |
| animation-iteration-count |   规定动画被播放的次数。默认是 1。 |
| animation-direction | 规定动画是否在下一周期逆向地播放。默认是 "normal"。 |
| animation-play-state | 规定动画是否正在运行或暂停。默认是 "running"。 |


### 1. 时间函数（animation-timing-function）

`animation-timing-function`属性定义了动画的播放速度曲线。

可选配置参数为:
`ease`、
`ease-in`、
`ease-out`、
`ease-in-out`、
`linear`、
`cubic-bezier(number, number, number, number)`

[对比示例](https://codepen.io/huanglilijie/pen/modLyw?editors=1100)

### 2.动画方向（animation-direction）

`animation-direction` 表示CSS动画是否反向播放。

可选参数为:

* animation-direction: normal 正序播放
* animation-direction: reverse 倒序播放
* animation-direction: alternate 交替播放
* animation-direction: alternate-reverse 反向交替播放

[对比示例](https://codepen.io/huanglilijie/pen/RdZLRp)

### 3.动画延迟（animation-delay）
`animation-delay`属性定义动画是从何时开始播放，即动画应用在元素上的到动画开始的这段时间的长度。

默认值0s，表示动画在该元素上后立即开始执行。

单位为s，或者ms。
![fsdf](https://www.w3cplus.com/sites/default/files/blogs/2016/1603/5s_delay.png)
一旦动画开始运行，延迟的值将不会再起作用,之后的循环也不会有延迟时间。
### 4.动画迭代次数（animation-iteration-count）

`animation-iteration-count`该属性就是定义我们的动画播放的次数。次数可以是1次或者无限循环。
默认值只播放一次。

参数为：数字或者infinite

### 5.动画填充模式（animation-fill-mode）

`animation-fill-mode`是指给定动画播放前后应用元素的样式。

可选参数为:

* animation-fill-mode: none 动画执行前后不改变任何样式
* animation-fill-mode: forwards 保持目标动画最后一帧的样式
* animation-fill-mode: backwards 保持目标动画第一帧的样式
* animation-fill-mode: both 动画将会执行 forwards 和 backwards 执行的动作。


### 6.动画播放状态（animation-play-state）

`animation-play-state`定义动画是否运行或者暂停。可以确定查询它来确定动画是否运行。

默认值为running

当动画暂停时，它会保留动画的所有最后的计算值：
![paused](https://www.w3cplus.com/sites/default/files/blogs/2016/1603/paused_state.png)

在JavaScript中使用此属性可在动画的一个控制周期中暂停和播放。
![play](https://www.w3cplus.com/sites/default/files/blogs/2016/1603/running_timeline.png)
此时动画会从暂停时的状态继续播放，不会回到0%的位置重新播放。






