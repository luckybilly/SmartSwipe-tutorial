## DoorConsumer
---

DoorConsumer 继承自 [ShuttersConsumer][ShuttersConsumer]

开门效果，侧滑后可显示被其遮挡的view

可用作侧滑删除，也可以用来制作封面效果


如下图所示：


<div align=center><img src="/images/doorConsumer.gif"><br/><br/></div>

### 示例代码

```java
//侧滑删除
SmartSwipe.wrap(view)
    .addConsumer(new DoorConsumer())
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

继承自 [ShuttersConsumer][ShuttersConsumer]，固定叶子数为2个，叶子数量设置无效外，其它参数的设置与之相同







[公共属性]: /pages/consumers/common_settings.md
[ShuttersConsumer]: /pages/consumers/ShuttersConsumer.md