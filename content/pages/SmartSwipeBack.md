## SmartSwipeBack
---

实现全局Activity侧滑返回



### 公共参数说明

- application: Application对象，用于注册全局activity生命周期监听
- edgeSize：边缘触发区域尺寸像素值（dp需转换为px），若设置为0，则表示整个activity区域都可触发
- direction：开启侧滑的方向，可设置为上下左右中的一个或多个，其取值可参考[侧滑方向](/pages/directions.md)，为0则不会触发侧滑
- filter: Activity过滤器，用于过滤确定对哪些activity进行侧滑返回的封装，若为null，则不过滤，对所有activity都执行侧滑返回的封装

Activity过滤器接口定义如下：

```java
public interface ActivitySwipeBackFilter {
    /**
     * Determine whether the activity parameter should swipe back
     * @param activity The activity to wrap or not
     * @return true: need to wrap with swipe back, false: do not wrap
     */
    boolean onFilter(Activity activity);
}
```

可以创建一个它的实现类来为SmartSwipeBack添加过滤，例如：

```java
//主Activity不需要侧滑返回功能，其它Activity都采用仿微信侧滑返回效果
SmartSwipeBack.activitySlidingBack(application, new SmartSwipeBack.ActivitySwipeBackFilter() {
    @Override
    public boolean onFilter(Activity activity) {
    	//根据传入的activity，返回true代表需要侧滑返回；false表示不需要侧滑返回
        return !(activity instanceof MainActivity);
    }
});
```


### 已封装的5种全局侧滑返回方案

- 仿手机QQ侧滑返回

<div align=center><img src="/images/stayConsumer.gif"><br/><br/></div>


```java
//activity保持不动，根据最后释放时的滑动速率来确定是否关闭activity
SmartSwipeBack.activityStayBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter, int edgeSize, int direction)

//在上一个方法的基础上，触发区域采用默认设置：左侧边缘20dp范围内
SmartSwipeBack.activityStayBack(this, activitySwipeBackFilter);
```


- 仿微信侧滑返回效果

<div align=center><img src="/images/activitySlidingBackConsumer.gif"><br/><br/></div>

```java
//添加全局透明侧滑返回(5.0以下版本的设备上使用activityStayBack)
// scrimColor: 玻璃颜色，显示在前一个activity之上的半透明遮罩颜色值, 默认为透明色
// shadowColor: 在当前activity移动的边缘显示的阴影颜色，默认为透明色
// shadowSize: shadowColor显示的大小像素值，默认为10dp
// factor: 关联移动系数
//		0: 		前一个activity保持不动，当前activity随着手势滑动而平移，移动后可透视前一个activity
//		(0,1): 	前一个activity与当前activity有关联移动效果，具体效果如上图所示
//		1: 		前一个activity跟随当前activity一起平移（类似于ViewPager的默认平移效果）
SmartSwipeBack.activitySlidingBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter
            , int edgeSize, int scrimColor, int shadowColor, int shadowSize
            , float factor, int direction)

//在上一个方法的基础上，部分参数采用默认设置：
//	触发区域：左侧边缘20dp
//	遮罩玻璃颜色值: Color.TRANSPARENT
//	边缘阴影颜色值: 0x80000000
//	边缘阴影尺寸: 10dp
SmartSwipeBack.activitySlidingBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter, float factor)

//在上一个方法的基础上，关联移动系数采用默认设置：0.5F
SmartSwipeBack.activitySlidingBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter)
```

- 仿MIUI系统贝塞尔曲线侧滑返回

<div align=center><img src="/images/bezierBackConsumer.gif"><br/><br/></div>

```java
// size: 尺寸大小（水平方向侧滑时指的是贝塞尔曲线区域的高度，垂直方向侧滑时指的是贝塞尔曲线区域的宽度）单位为px(dp需转换为px)
// thickness: 侧滑拉满时贝塞尔曲线区域的厚度（水平方线侧滑时指的是最大宽度，垂直方向侧滑时指的是最大高度）单位为px(dp需转换为px)
// color: 贝塞尔曲线和屏幕边缘构成的区域填充色
// arrowColor: 贝塞尔曲线区域内箭头的颜色
SmartSwipeBack.activityBezierBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter
        , int edgeSize, int size, int thickness, int color, int arrowColor, int direction)

//在上一个方法的基础上，部分参数采用默认值：
// thickness: 30dp
// size: 200dp
// 触发方向：左侧
// 触发区域范围需设置
SmartSwipeBack.activityBezierBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter, int edgeSize)

//在上一个方法的基础上，触发区域默认值设置为：左侧20dp范围内
SmartSwipeBack.activityBezierBack(this, activitySwipeBackFilter);
```

- 开门式侧滑返回

> 支持的最低api等级为21，即android 5.0 [如何兼容5.0以下版本？](#兼容50以下系统版本)
>
> 不支持含有SurfaceView/GLSurfaceView/TextureView/VideoView等控件的页面（将会在这些view的区域显示透明），如果app内有页面含有这些控件，需要在ActivitySwipeBackFilter中将其过滤

<div align=center><img src="/images/activityDoorBackConsumer.gif"><br/><br/></div>


```java
// scrimColor: 玻璃颜色，即开门过程中透视前一个activity时的遮罩颜色
// refreshable: 是否实时刷新，若页面有动画等内容时，需要设置为true以取得更好的用户体验，否则可设置为false以获得更好的性能
SmartSwipeBack.activityDoorBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter
        , int direction, int edgeSize, int scrimColor, boolean refreshable)

//在上一个方法的基础上，使用的默认值为：
// scrimColor: 0x80000000
// refreshable: true
// 触发区域默认值设置为：左侧20dp范围内
SmartSwipeBack.activityDoorBack(this, activitySwipeBackFilter);
```

- 百叶窗式侧滑返回

> 支持的最低api等级为21，即android 5.0 [如何兼容5.0以下版本？](#兼容50以下系统版本)
> 
> 不支持含有SurfaceView/GLSurfaceView/TextureView/VideoView等控件的页面（将会在这些view的区域显示透明），如果app内有页面含有这些控件，需要在ActivitySwipeBackFilter中将其过滤

<div align=center><img src="/images/activityShuttersBackConsumer.gif"><br/><br/></div>

```java
// scrimColor: 玻璃颜色，即开门过程中透视前一个activity时的遮罩颜色
// refreshable: 是否实时刷新，若页面有动画等内容时，需要设置为true以取得更好的用户体验，否则可设置为false以获得更好的性能
SmartSwipeBack.activityShuttersBack(Application application, SmartSwipeBack.ActivitySwipeBackFilter filter
            , int direction, int edgeSize, int scrimColor, boolean refreshable) {

//在上一个方法的基础上，使用的默认值为：
// scrimColor: 0x80000000
// refreshable: true
// 触发区域默认值设置为：左侧20dp范围内
SmartSwipeBack.activityShuttersBack(this, activitySwipeBackFilter);
```

### 兼容5.0以下系统版本

由于采用了activity透明的方式来透视前一个activity，在5.0以下版本系统上存在兼容性问题，导致仿微信侧滑返回、开门式侧滑返回和百叶窗式侧滑返回只在5.0以上版本的设备生效。

由于5.0以下版本的设备占比已经较少，与其采用会修改主题为透明并在onCreated中在变为不透明等较为复杂的方案，不如分别为5.0以上及以下的采用不同的侧滑返回方案


示例代码：

```java
if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
	//5.0以下用仿MIUI系统的贝塞尔曲线侧滑返回
    SmartSwipeBack.activityBezierBack(application, null);
} else {
	//5.0及以上用百叶窗返回
	SmartSwipeBack.activityShuttersBack(application, null);
}
```

*注：仿微信侧滑返回(activitySlidingBack)方案在5.0以下默认使用仿手机QQ的侧滑方案(activityStayBack)*



### 自定义全局侧滑返回方案

如果以上封装的方案并不能满足您当前的实际需求，可以仿照这些封装的实现方式来自定义一个更合适当前需求的侧滑方案

首先，需要定义一个SwipeBackConsumerFactory接口的实现类，用于为不同的activity创建侧滑返回功能对应的{{book.baseName}}

```java
public interface SwipeBackConsumerFactory {
    /**
     * Create SwipeConsumer to do swipe back business for activity
     * @param activity activity to wrap with swipe back
     * @return SwipeConsumer
     */
    SwipeConsumer createSwipeBackConsumer(Activity activity);
}
```

然后，在合适的位置调用以下方法， 传入自定义的SwipeBackConsumerFactory对象，将其应用于全局activity

```java
SmartSwipeBack.activityBack(Application application, SwipeBackConsumerFactory factory, ActivitySwipeBackFilter filter)
```






