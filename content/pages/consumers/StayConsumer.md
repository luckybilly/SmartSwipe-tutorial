## StayConsumer
---


主体contentView侧滑过程中保持不动，侧滑结束时按照手势滑动的方向和速率来确定是否开启，若开启将回调[SwipeListener][SwipeListener]的`onSwipeOpen`方法

可以用来实现侧滑返回


如下图所示：


<div align=center><img src="/images/stayConsumer.gif"><br/><br/></div>

### 示例代码

```java
//activity侧滑返回
SmartSwipe.wrap(this)
    .addConsumer(new StayConsumer())
    .enableAllDirections()
    .addListener(new SimpleSwipeListener(){
        @Override
        public void onSwipeOpened(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction) {
            finish();
        }
    })
    ;
```


### 属性设置

虽然继承自 [SwipeConsumer][SwipeConsumer]，[公共属性][公共属性]中的参数都可设置，但由于没有任何UI呈现，大多数设置是无效的

有效的仅有
- 第1条: 侧滑方向的启用与锁定
- 第6条: 可以获取但不能设置的属性




[公共属性]: /pages/consumers/common_settings.md
[SwipeConsumer]: /pages/SwipeConsumer.md