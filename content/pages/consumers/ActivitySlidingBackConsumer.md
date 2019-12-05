## ActivitySlidingBackConsumer
---

仿微信侧滑返回效果

专为完成Activity的侧滑而做的一种效果，支持与前一个Activity的联动效果 

> 建议使用封装后的工具类[SmartSwipeBack][SmartSwipeBack]，更舒适
> 
> issue中相关的说明：[#28](https://github.com/luckybilly/SmartSwipe/issues/28#issuecomment-520708861)


效果图如下所示：


<div align=center><img src="/images/activitySlidingBackConsumer.gif"><br/><br/></div>

### 示例代码

```java
//activity侧滑返回
SmartSwipe.wrap(this)
    .addConsumer(new ActivitySlidingBackConsumer(this))
    .setRelativeMoveFactor(0.5F)
    .enableAllDirections()
    ;
```


### 属性设置

继承自 [TranslucentSlidingConsumer][TranslucentSlidingConsumer]，属性设置与之相同

同样支持联动系数设置，不过指的是前一个Activity与当前activity的联动，联动效果等同于[SlidingConsumer][SlidingConsumer]中抽屉view的联动效果

### 注意事项

<font color=red>支持的最低android API版本为21 (Lollipop)，即android 5.0</font>

[如何兼容5.0以下系统版本？][SmartSwipeBack]



[SmartSwipeBack]: /pages/SmartSwipeBack.md
[公共属性]: /pages/consumers/common_settings.md
[TranslucentSlidingConsumer]: /pages/consumers/TranslucentSlidingConsumer.md
[SlidingConsumer]: /pages/consumers/SlidingConsumer.md