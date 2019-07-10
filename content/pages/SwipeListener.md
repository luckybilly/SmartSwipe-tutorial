## SwipeListener

可以通过`swipeConsumer.addListener(listener)`来监听该`SwipeConsumer`对象的运行状态

可以对同一个`SwipeConsumer`对象多次调用`addListener(listener)`来添加多个监听对象，并按照添加的顺序执行回调


### 接口定义
```java
/**
 * listen swipe state of {@link SwipeConsumer} via {@link SwipeConsumer#addListener(SwipeListener)}
 * @author billy.qi
 * @see SimpleSwipeListener
 */
public interface SwipeListener {
    void onConsumerAttachedToWrapper(SmartSwipeWrapper wrapper, SwipeConsumer consumer);
    void onConsumerDetachedFromWrapper(SmartSwipeWrapper wrapper, SwipeConsumer consumer);
    void onSwipeStateChanged(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int state, int direction, float progress);
    void onSwipeStart(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction);
    void onSwipeProcess(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction, boolean settling, float progress);
    void onSwipeRelease(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction, float progress, float xVelocity, float yVelocity);
    void onSwipeOpened(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction);
    void onSwipeClosed(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction);
}
```

~~~
Tips: 提供了一个空实现类：SimpleSwipeListener， 只需要个别状态回调时可继承此类，从而使得代码更简洁
~~~


### 回调顺序

1. 手势打开/关闭((mReleaseMode & SwipeConsumer.RELEASE_MODE_AUTO_OPEN_CLOSE) == RELEASE_MODE_AUTO_CLOSE)
```java
onSwipeStart
onSwipeStateChanged (state = 1)
onSwipeProcess(0个或多个, settling=false)
onSwipeRelease
onSwipeOpened(仅在抬起手指时滑动距离已满足open状态，且释放模式为RELEASE_MODE_AUTO_CLOSE时才会回调)
onSwipeStateChanged (state = 2)
onSwipeProcess(0个或多个, settling=true)
onSwipeStateChanged (state = 0)
onSwipeOpened/onSwipeClosed
```
2. 自动打开/关闭
```java
onSwipeStart
onSwipeStateChanged (state = 2)
onSwipeProcess(多个, settling=true)
onSwipeStateChanged (state = 0)
onSwipeOpened/onSwipeClosed
```


### 简单示例
```java
SmartSwipe.wrap(SwipeBackBezierConsumerActivity.this)
    .addConsumer(new BezierBackConsumer())
    .enableLeft()
    .addListener(new SimpleSwipeListener() {
        @Override
        public void onSwipeOpened(SmartSwipeWrapper wrapper, SwipeConsumer consumer, int direction) {
            finish();
        }
    })
```
