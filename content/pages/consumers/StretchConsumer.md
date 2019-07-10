## StretchConsumer
---

滑动后弹性拉伸的一种UI效果，如下图所示：

<div align=center><img src="/images/stretchConsumer.gif"><br/><br/></div>

### 示例代码

```java
SmartSwipe.wrap(view)
	.addConsumer(new StretchConsumer())
	.enableVertical();
```