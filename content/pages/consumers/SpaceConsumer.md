## SpaceConsumer
---

滑动后弹性留白的一种UI效果，默认添加的侧滑距离计算器为`new ScaledCalculator(0.5F)`

效果如下图所示：

<div align=center><img src="/images/spaceConsumer.gif"><br/><br/></div>

### 示例代码

```java
SmartSwipe.wrap(view)
	.addConsumer(new SpaceConsumer())
	.enableVertical();
```

### 属性列表

继承自 [SwipeConsumer][SwipeConsumer]，[公共属性][公共属性]中的参数设置尽皆有效


[公共属性]: /pages/consumers/common_settings.md
[SwipeConsumer]: /pages/SwipeConsumer.md