### [CSS Animation](#)
**介绍**：描述动画的样式规则和用于指定动画开始、结束以及中间点样式的关键帧。

----


### [Introduce](#)
创建动画序列，需要使用 [animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation) 属性或其子
属性，该属性允许配置动画时间、时长以及其他动画细节，但该属性不能配置动画的实际表现，动画的实际表现是由 
[@keyframes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes) 规则实现

一段动画，就是一段时间内连续播放n个画面。每一张画面，我们管它叫做“帧”。一定时间内连续快速播放若干个帧，就成了人眼中所看到的动画。同样时间内，播放的数越多，画面看起来越流畅。

**关键帧**指的是，在构成一段动画的若干帧中，起到决定性作用的2-3帧，就如同曲线运动，指定抛出位置，制高点，落下位置这几帧就行，其他运动帧可以由浏览器给你补全。

CSS 动画有三个主要优点：
- 能够非常容易地创建简单动画，你甚至不需要了解 JavaScript 就能创建动画。
- 动画运行效果良好，甚至在低性能的系统上。渲染引擎会使用跳帧或者其他技术以保证动画表现尽可能的流畅。而使用 JavaScript 实现的动画通常表现不佳（除非经过很好的设计）。
- 让浏览器控制动画序列，允许浏览器优化性能和效果，如降低位于隐藏选项卡中的动画更新频率。

#### [1.1 animation](#)
animation 属性是 animation-name，animation-duration, animation-timing-function，animation-delay，animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 属性的一个简写属性形式。

| 属性                                                         | 描述                                                         | CSS  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
| [@keyframes](https://www.runoob.com/cssref/css3-pr-animation-keyframes.html) | 规定动画。                                                   | 3    |
| [animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation) | 所有动画属性的简写属性。                                     | 3    |
| [animation-name](https://www.runoob.com/cssref/css3-pr-animation-name.html) | 规定 @keyframes 动画的名称。                                 | 3    |
| [animation-duration](https://www.runoob.com/cssref/css3-pr-animation-duration.html) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             | 3    |
| [animation-timing-function](https://www.runoob.com/cssref/css3-pr-animation-timing-function.html) | 规定动画的速度曲线。默认是 "ease"。                          | 3    |
| [animation-fill-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-play-state) | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 | 3    |
| [animation-delay](https://www.runoob.com/cssref/css3-pr-animation-delay.html) | 规定动画何时开始。默认是 0。                                 | 3    |
| [animation-iteration-count](https://www.runoob.com/cssref/css3-pr-animation-iteration-count.html) | 规定动画被播放的次数。默认是 1。                             | 3    |
| [animation-direction](https://www.runoob.com/cssref/css3-pr-animation-direction.html) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          | 3    |
| [animation-play-state](https://www.runoob.com/cssref/css3-pr-animation-play-state.html) | 规定动画是否正在运行或暂停。默认是 "running"。               | 3    |


### [keyframes](#)
关键帧 @keyframes at 规则通过在动画序列中定义关键帧（或 waypoints）的样式来控制 CSS 动画序列中的中间步骤。
和**过渡**相比，关键帧 keyframes 可以控制动画序列的中间步骤。
```css
@keyframes animation-name { keyframes-selector {/*css-styles*/;}}
```
可以按任意顺序列出关键帧百分比；它们将按照其应该发生的顺序来处理。
```css
@keyframes identifier {
  0% {
    top: 0;
  }
  50% {
    top: 30px;
    left: 20px;
  }
  50% {
    top: 10px;
  }
  100% {
    top: 0;
  }
}
```
from 等价于 0%。 to 等价于 100%。
```css
@keyframes slidein {
  from {
    transform: translateX(0%);
  }

  to {
    transform: translateX(100%);
  }
}
```

**小例子**： [向右移动小方块](example/move.html)