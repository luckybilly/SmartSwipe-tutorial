## Release处理方式
---

不同的侧滑功能，在用户拖动结束抬起手指时的处理方式可能会不同。

{{book.baseName}}中定义了以下几种Release处理方式，可以组合使用


名称|常量|取值|备注
:---:|:---|:---:|:---
无|SwipeConsumer.RELEASE_MODE_NONE|0x0|不管侧滑进度如何，释放时（手指抬起时）均保持不动
自动关闭|SwipeConsumer.RELEASE_MODE_AUTO_CLOSE|0x1|不管拉动多少距离，释放后自动关闭
自动开启|SwipeConsumer.RELEASE_MODE_AUTO_OPEN|0x2|不管拉动多少距离，释放后自动打开至open状态(mProgress == 1)
自动开启&关闭|SwipeConsumer.RELEASE_MODE_AUTO_OPEN_CLOSE|0x3|优先按照释放时的速度方向判断开启开始关闭，若速率为0，则按照侧滑进度是否过半(mProgress > 0.5F)来决定自动开启还是关闭
保持开启|SwipeConsumer.RELEASE_MODE_HOLE_OPEN|0x4|如果释放时侧滑进度已满(mProgress >= 1),则保持或自动回弹到开启状态

示例讲解：

在[SmartSwipeRefresh][SmartSwipeRefresh]中封装下拉刷新功能时，使用了`RELEASE_MODE_AUTO_CLOSE` 和 `RELEASE_MODE_HOLE_OPEN`的组合

```java
setReleaseMode(SwipeConsumer.RELEASE_MODE_AUTO_CLOSE | SwipeConsumer.RELEASE_MODE_HOLE_OPEN)
```

原因是，下拉刷新需求决定了：需要手动拉满(mProgress >= 1)才能执行刷新，其它情况下都是自动收回(自动关闭)。所以，需要用自动关闭和保持开启2个模式的组合来完成释放时的处理

*注：如果拉动越界了还需要等待自动回弹到开启状态(mProgress == 1)时才执行刷新*






[SmartSwipeRefresh]: /pages/SmartSwipeRefresh.md