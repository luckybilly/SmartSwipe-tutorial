## TranslucentSlidingConsumer
---

TranslucentSlidingConsumer 继承自 [SlidingConsumer][SlidingConsumer]

侧滑透明效果，侧滑后可显示被其遮挡的view

可用作侧滑删除，也可以用来制作封面效果


如下图所示：


<div align=center><img src="/images/translucentSlidingConsumer.gif"><br/><br/></div>

### 示例代码

```java
//侧滑删除
SmartSwipe.wrap(view)
    .addConsumer(new TranslucentSlidingConsumer())
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

继承自 [SlidingConsumer][SlidingConsumer]， 但没有抽屉

除以下参数无效外，其它参数的设置与`SlidingConsumer`相同

- 各种setDrawerView方法
- mRelativeMoveFactor
- mDrawerExpandable
- mEdgeAffinity
- mOverSwipeFactor


### 与[SpaceConsumer][SpaceConsumer]的区别

- SpaceConsumer实现的非常简洁，功能也较为简单，其作用仅仅是让被包裹的contentView可以动起来，可以用来优化交互体验
- TranslucentSlidingConsumer继承自SlidingConsumer，虽然没有抽屉view，但也具备一些SlidingConsumer的功能：
	- 在contentView或移动后的留白区域显示遮罩及边缘阴影
	- 另外，由于TranslucentSlidingConsumer的默认滑动距离就是自身的控件尺寸
		- 适用于完成侧滑删除效果（demo中有示例）
		- 适用于完成侧滑封面效果（demo中有示例）
- TranslucentSlidingConsumer还是ActivitySlidingBackConsumer的基类
	- 在TranslucentSlidingConsumer的基础上增加了activity透明相关的逻辑




[公共属性]: /pages/consumers/common_settings.md
[SlidingConsumer]: /pages/consumers/SlidingConsumer.md
[SpaceConsumer]: /pages/consumers/SpaceConsumer.md