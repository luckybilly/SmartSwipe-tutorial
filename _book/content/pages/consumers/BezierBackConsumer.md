## BezierBackConsumer
---


主体contentView侧滑过程中保持不动，在滑动方向的边缘出现一个贝塞尔曲线背景的返回箭头，完全打开后释放将回调[SwipeListener][SwipeListener]的`onSwipeOpen`方法

可以用来实现侧滑返回


如下图所示：


<div align=center><img src="/images/bezierBackConsumer.gif"><br/><br/></div>

### 示例代码

```java
//activity侧滑返回
SmartSwipe.wrap(this)
    .addConsumer(new BezierBackConsumer())
    .enableAllDirections()
    .addListener(new SimpleSwipeListener() {
        @Override
        public void onSwipeOpened(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction) {
            finish();
        }
    })
    ;
```


### 属性设置

继承自 [SwipeConsumer][SwipeConsumer]，[公共属性][公共属性]中的参数设置尽皆有效

*getter/setter都一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mSize|int|>0|200dp|贝塞尔曲线的尺寸（横向侧滑时代表贝塞尔曲线区域的高度，纵向侧滑时代表宽度）
mOpenDistance|int|>0|30dp|此变量在父类中用作从关闭状态到打开状态所需的侧滑距离，此处赋予另一个功能：作为贝塞尔曲线的最大显示厚度
mColor|int|颜色值|0|此颜色值alpha部分无效，只取RGB部分，alpha根据侧滑进度计算：<br/>alpha = 0xFF * mProgress, mProgress有效范围为[0.2, 0.8]
mArrowSize|int|>0|4dp|箭头尺寸的一半，例如:左侧侧滑时，箭头的高度像素值 = 2 * mArrowSize
mArrowColor|颜色值|>0|0xFFF2F2F2|此颜色值alpha部分无效，只取RGB部分，alpha根据侧滑进度计算：<br/>alpha = 0xFF * mProgress, mProgress有效范围为[0, 1]
mCenter|boolean|true/false|false|是否固定居中显示，为false时中心点为手指按下时的位置。例如：左侧侧滑时中心的坐标为(0, pointerDownY)







[公共属性]: /pages/consumers/common_settings.md
[SwipeConsumer]: /pages/SwipeConsumer.md
[SwipeListener]: /pages/SwipeListener.md