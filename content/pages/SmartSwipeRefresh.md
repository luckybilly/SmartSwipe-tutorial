## SmartSwipeRefresh
---

SmartSwipeRefresh支持下拉刷新和上拉加载更多，并且支持纵向模式和横向模式（左侧右拉刷新、右侧左拉加载更多），支持禁用刷新或者禁用加载更多。

下拉刷新(及上拉加载更多)是DrawerConsumer及SlidingConsumer的一种应用，SmartSwipeRefresh使用它们来分别支持不同的下拉刷新模式，包括：

- drawerMode: <br/> （封装使用的是DrawerConsumer）<br/>抽屉模式，下拉时，被刷新的主体不动，刷新控件显示在主体上方拖动显示
- behindMode: <br/> （封装使用的是SlidingConsumer）<br/>刷新控件被固定在后面，下拉时，被刷新的主体被拖动，从而显示在后面的刷新控件
- scaleMode: <br/> （封装使用的是SlidingConsumer）<br/> 缩放模式(伪)，下拉时，被刷新的主体向下移动，刷新控件的中间位置始终在主体下拉后的空白区域的正中间
- translateMode: <br/> （封装使用的是SlidingConsumer）<br/>平移模式，下拉时，被刷新的主体与刷新控件一起移动

#### 效果演示

模式|mDrawerExpandable|mEdgeAffinity|效果图|备注
:---:|:---:|:---:|:---
drawerMode|-|-|<img width="200" src="/images/refresh_drawer.gif">|默认值：<br/>mOverSwipeFactor=0.5F
drawerMode|-|-|<img width="200" src="/images/refresh_drawer_0.gif">|手动设置为不可越界拖动:<br/>`setOverSwipeFactor(0);`
behindMode|false|false|<img width="200" src="/images/refresh_behind.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0F
behindMode|false|true|<img width="200" src="/images/refresh_behind_edgeAffinity.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0F
behindMode|true|无效|<img width="200" src="/images/refresh_behind_expandable.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0F
scaleMode|false|false|<img width="200" src="/images/refresh_scale.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0.5F
scaleMode|false|true|<img width="200" src="/images/refresh_scale_edgeAffinity.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0.5F
scaleMode|true|无效|<img width="200" src="/images/refresh_scale_expandable.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=0.5F
translateMode|false|false|<img width="200" src="/images/refresh_translate.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=1F
translateMode|false|true|<img width="200" src="/images/refresh_translate_edgeAffinity.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=1F
translateMode|true|无效|<img width="200" src="/images/refresh_translate_expandable.gif">|默认值：<br/>mOverSwipeFactor=0.5F<br/>mRelativeMoveFactor=1F

### 示例代码

为某个view添加下拉刷新功能的示例代码如下:
```java
SmartSwipeRefresh.SmartSwipeRefreshDataLoader loader = new SmartSwipeRefresh.SmartSwipeRefreshDataLoader() {
    @Override
    public void onRefresh(final SmartSwipeRefresh ssr) {
        //加载刷新数据
        loadRefreshData(new Callback() {
        	void success() {
        		ssr.finished(true);
        		//刷新完成后，如果数据不足一页，可以提前设置已加载完成的状态
        		boolean loadCompleted = false;
        		ssr.setNoMoreData(loadCompleted);
        	}
        	void failed() {
        		ssr.finished(false);
        	}
        })
    }

    @Override
    public void onLoadMore(final SmartSwipeRefresh ssr) {
        //加载下一页数据
        loadMoreData(new Callback() {
        	void success() {
        		ssr.finished(true);
        		// 是否已全部加载完成
        		boolean loadCompleted = true;
        		ssr.setNoMoreData(loadCompleted);
        	}
        	void failed() {
        		ssr.finished(false);
        	}
        })
    }
};

SmartSwipeRefresh.behindMode(findViewById(R.id.container), false).setDataLoader(loader);
```



### 基本使用

- 使用默认的header和footer (ClassicHeader和ClassicFooter)
```java
//xxxMode第二个参数为false，表示工作方向为纵向：下拉刷新&上拉加载更多
//如果第二个参数设置为true，则表示工作方向为横向：右拉刷新&左拉加载更多
SmartSwipeRefresh.drawerMode(view, false).setDataLoader(loader);
SmartSwipeRefresh.behindMode(view, false).setDataLoader(loader);
SmartSwipeRefresh.scaleMode(view, false).setDataLoader(loader);
SmartSwipeRefresh.translateMode(view, false).setDataLoader(loader);
```

- 全局使用自定义的Header和Footer构造器：`SmartSwipeRefreshViewCreator`

	- 实现SmartSwipeRefreshViewCreator接口，接口的定义如下：
	```java
	/**
	 * creator of {@link SmartSwipeRefreshHeader} and {@link SmartSwipeRefreshFooter}
	 */
	public interface SmartSwipeRefreshViewCreator {
	    /**
	     * create the refresh header view
	     * @param context context
	     * @return header
	     */
	    SmartSwipeRefreshHeader createRefreshHeader(Context context);
	    /**
	     * create the refresh footer view
	     * @param context context
	     * @return footer
	     */
	    SmartSwipeRefreshFooter createRefreshFooter(Context context);
	}
	```
	- 将构造器设置为全局使用

	```java
	SmartSwipeRefresh.setDefaultRefreshViewCreator(creator);
	```
	- 使用全局构造器给指定的View添加下拉刷新功能(与使用默认的header和footer的方式一样)
	```java
	SmartSwipeRefresh.drawerMode(view, false).setDataLoader(loader);
	SmartSwipeRefresh.behindMode(view, false).setDataLoader(loader);
	SmartSwipeRefresh.scaleMode(view, false).setDataLoader(loader);
	SmartSwipeRefresh.translateMode(view, false).setDataLoader(loader);
	```

- 为当前view使用特殊的自定义header和footer
```java
//xxxMode的第三个参数为false，表示不使用全局和默认的header和footer
SmartSwipeRefresh.drawerMode(view, false, false).setDataLoader(loader).setHeader(header).setFooter(footer);
SmartSwipeRefresh.behindMode(view, false, false).setDataLoader(loader).setHeader(header).setFooter(footer);
SmartSwipeRefresh.scaleMode(view, false, false).setDataLoader(loader).setHeader(header).setFooter(footer);
SmartSwipeRefresh.translateMode(view, false, false).setDataLoader(loader).setHeader(header).setFooter(footer);
```

### 数据加载回调
```java
/**
 * The refresh data loader
 * When refresh or load more event emits, its methods will be called
 */
public interface SmartSwipeRefreshDataLoader {
    /**
     * Called when {@link SmartSwipeRefreshHeader} swipe released and it has been fully swiped
     * @param ssr {@link SmartSwipeRefresh}
     */
    void onRefresh(SmartSwipeRefresh ssr);
    /**
     * Called when {@link SmartSwipeRefreshFooter} swipe released and it has been fully swiped
     * @param ssr {@link SmartSwipeRefresh}
     */
    void onLoadMore(SmartSwipeRefresh ssr);
}
```

在侧滑事件符合下拉刷新或上拉加载更多时，会回调对应的`onRefresh`/`onLoadMore`方法，并且header和footer开始播放加载中的动画

`SmartSwipeRefreshDataLoader`中加载完数据时，需要调用`ssr.finished(success)`来告诉SmartSwipeRefresh加载过程已结束，开始播放结束动画

如果数据已加载完成，可以通过设置`ssr.setNoMoreData(true)`来显示全部加载完成的状态，并且继续下拉时不再触发网络请求（不会回调`onLoadMore`方法）

### 其它功能

- 自动刷新
```java
smartSwipeRefresh.startRefresh(); //立即开启自动刷新
```
- 自动加载更多
```java
smartSwipeRefresh.startLoadMore();
```
- 禁用刷新功能
```java
smartSwipeRefresh.disableRefresh();
```
- 禁用加载更多
```java
smartSwipeRefresh.disableLoadMore();
```
- 获取SwipeConsumer对象，从而可以使用更多相关api，例如：
```java
SmartSwipeRefresh.scaleMode(findViewById(R.id.container), false)
    .setDataLoader(dataLoader)
    .getSwipeConsumer() //获取SwipeConsumer
    .as(SlidingConsumer.class)	//类型转换
    .setEdgeAffinity(true); //刷新控件在下拉空间超出控件大小时紧贴边缘，不随被刷新主体的拖动而继续移动
```



### 自定义Header和Footer

{{book.name}}内置了布局较为简洁的`ClassicHeader`和`ClassicFooter`。

可以仿照`ClassicHeader`和`ClassicFooter`来创建自定义的header和footer

自定义header需要实现接口： `SmartSwipeRefreshHeader`

自定义footer需要实现接口： `SmartSwipeRefreshFooter`

接口定义及解释如下：

```java
private interface RefreshView {
    /**
     * 获取刷新控件的UI布局，不能为null
     */
    View getView();

    /**
     * 初始化回调方法，在RefreshView作为header或footer被添加到{@link DrawerConsumer} or {@link SlidingConsumer}之前被调用
     * @param horizontal true: 工作方向为横向
     *                   false: 工作方向为纵向
     */
    void onInit(boolean horizontal);

    /**
     * 开始工作时回调
     */
    void onStartDragging();

    /**
     * 工作过程中回调
     * @param dragging 是否用户手势拖动
     * @param progress 进度，关闭状态为0，完全打开状态为1，取值范围：[0F, 1F + overSwipeFactor]，超过1时是否会有回弹效果
     */
    void onProgress(boolean dragging, float progress);

    /**
     * 当{@link SmartSwipeRefresh#finished(boolean)} 调用时回调此方法
     * @param success 数据加载是否成功
     * @return 延迟多少毫秒开始执行收回的动画（等待RefreshView动画结束并显示加载成功状态相关的UI）
     */
    long onFinish(boolean success);

    /**
     * 开始加载数据，展示加载数据时的相关UI动画
     */
    void onDataLoading();
}
/** header需要实现的接口 */
public interface SmartSwipeRefreshHeader extends RefreshView {

}

/** footer需要实现的接口 */
public interface SmartSwipeRefreshFooter extends RefreshView {
    /**
     * 给footer打标记，表示数据已全部加载完成，展示相关UI
     * @param noMoreData 	true: 已全部加载完成
     						false: 未加载完成，下次上拉还需要继续执行下一页数据的加载工作
     */
    void setNoMoreData(boolean noMoreData);
}
```

创建好header和footer之后，再创建一个`SmartSwipeRefreshViewCreator`为全局下拉刷新功能创建自定义的刷新控件

```java
SmartSwipeRefresh.setDefaultRefreshViewCreator(new SmartSwipeRefreshViewCreator() {
    @Override
    public SmartSwipeRefreshHeader createRefreshHeader(Context context) {
        return new MyHeader(context);
    }

    @Override
    public SmartSwipeRefreshFooter createRefreshFooter(Context context) {
        return new MyFooter(context);
    }
});
```

在`smart-swipe-refresh-ext`扩展包中, 将提供一些对炫酷的第三方header和footer效果的封装，最新版本： [![Download](https://api.bintray.com/packages/hellobilly/android/smart-swipe-refresh-ext/images/download.svg)](https://bintray.com/hellobilly/android/smart-swipe-refresh-ext/_latestVersion))

其中包含：

[Ifxcyr/ArrowDrawable](https://github.com/Ifxcyr/ArrowDrawable)|更多...
:---:|:--:
<img width="200" src="/images/refresh_arrow.gif">|[期待你来添加](https://github.com/luckybilly/SmartSwipe/pulls)








