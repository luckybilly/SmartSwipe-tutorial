## ShuttersConsumer
---

百叶窗效果，侧滑后可显示被其遮挡的view

可用作侧滑删除，也可以用来制作封面效果


如下图所示：


<div align=center><img src="/images/shuttersConsumer.gif"><br/><br/></div>

### 示例代码

```java
//侧滑删除
SmartSwipe.wrap(view)
    .addConsumer(new ShuttersConsumer())
    .enableHorizontal() //启用左右两侧侧滑
    .addListener(new SimpleSwipeListener(){
        @Override
        public void onSwipeOpened(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction) {
        	//侧滑打开时，移除
            ViewParent parent = wrapper.getParent();
            if (parent instanceof ViewGroup) {
                ((ViewGroup) parent).removeView(wrapper);
            }
            //adapter.removeItem(getAdapterPosition());// 也可用作从recyclerView中移除该项
        }
    })
    ;
```


### 属性设置

继承自 [SwipeConsumer][SwipeConsumer]，[公共属性][公共属性]中的参数设置尽皆有效

*getter/setter都一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mScrimColor|int|颜色值|0|遮罩玻璃颜色色值，如果为0，则不显示遮罩
mLeavesCount|int|1~100|5|百叶窗叶子数
mRefreshable|boolean|true/false|false|侧滑时是否实时刷新，为true时能实时刷新显示，对于页面中有动画，能实时显示动画；为false时只取一次view的快照，但性能损耗低
mWaitForScreenshot|boolean|true/false|true|在获取view的快照成功前是否开始侧滑。在作为入场动画时较为有用：<br/>先设置为false，立即打开，再设置为true，然后平滑关闭
setRefreshFrameRate|int|1~60|30|当mRefreshable为true时，设置自动刷新的帧率






[公共属性]: /pages/consumers/common_settings.md
[SlidingConsumer]: /pages/consumers/SlidingConsumer.md