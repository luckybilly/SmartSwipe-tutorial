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