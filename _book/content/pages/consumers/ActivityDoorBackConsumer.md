## ActivityDoorBackConsumer
---

开门式侧滑返回，专为完成Activity的侧滑返回而做的一种效果

如下图所示：


<div align=center><img src="/images/activityDoorBackConsumer.gif"><br/><br/></div>

### 示例代码

```java
//activity侧滑返回
SmartSwipe.wrap(this)
    .addConsumer(new ActivityDoorBackConsumer(this))
    .setScrimColor(0x7F000000)
    .enableAllDirections()
    ;
```


### 属性设置

继承自 [DoorConsumer][DoorConsumer]，属性设置与之相同


### 注意事项

<font color=red>支持的最低android API版本为21 (Lollipop)，即android 5.0</font>

[如何兼容5.0以下系统版本？][SmartSwipeBack]



[SmartSwipeBack]: /pages/SmartSwipeBack.md
[公共属性]: /pages/consumers/common_settings.md
[DoorConsumer]: /pages/consumers/DoorConsumer.md