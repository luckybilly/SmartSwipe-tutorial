## {{ book.baseName }}
---

作为所有具体Consumer的父类，{{ book.baseName }}本身是一个抽象类，其主要职能为：

- 对滑动事件的捕获、加工及传递等逻辑进行统一封装
- 对滑动方向是否启用、滑动方向是否锁定、滑动距离限制、自动滑动动画的插值器等一系列[公共属性][公共属性]的处理并提供相应的设置API

### 工作流程

1. 捕获侧滑事件
	- 从静止状态开始侧滑: tryAcceptMoving, 返回true表示捕获
	- 非静止状态开始侧滑: tryAcceptSettling, 返回true表示捕获
2. 开始侧滑事件
	- onSwipeAccepted, 捕获到侧滑事件后，会立即回调
3. 计算并消费滑动距离(0次或多次)
	- 计算侧滑距离
		- clampDistanceHorizontal
		- clampDistanceVertical
	- 消费滑动距离
		- onDisplayDistanceChanged
4. 手势释放事件(手势触发的侧滑才会产生手势释放事件)
	- onSwipeReleased, 根据[Release处理方式][Release处理方式]进行相应处理
5. 已关闭/已打开
	- onOpened/onClosed
6. 结束侧滑事件

### 如何自定义一个{{ book.baseName }}？

1. 新建一个类，继承{{ book.baseName }}
2. *[可选]*在构造方法中进行一些初始化（需要context对象才能初始化的属性，可以放在onAttachToWrapper方法中初始化）
3. *[可选]*如果有额外的捕获逻辑，可以重写父类的`tryAcceptMoving`和`tryAcceptSettling`方法
4. 重写onSwipeAccepted方法，由于此时已经确定捕获了侧滑事件，并确定好了侧滑的方向(mDirection)，可以为本次侧滑事件做一些初始化工作
5. *[可选]*重写`clampDistanceHorizontal`及`clampDistanceHorizontal`方法，可在满足一定条件下才真正执行侧滑
6. 重写onDisplayDistanceChanged方法，执行具体的侧滑的UI效果呈现
7. *[可选]*如果UI呈现效果中包含布局控件的移动，需要重写onLayout方法，在此方法中也要按照侧滑后的逻辑进行控件布局定位
8. 重写onDetachFromWrapper方法，还原现场，移除当前consumer的所有改动痕迹

具体细节可参考内置的各种{{ book.baseName }}:

- [DoorConsumer][DoorConsumer] (开门：从中间向两边分开)
- [ShuttersConsumer][ShuttersConsumer] (百叶窗)
- [DrawerConsumer][DrawerConsumer] (在上层显示滑动抽屉)
- [SlidingConsumer][SlidingConsumer] (在下层显示可相对滑动的抽屉)
- [SpaceConsumer][SpaceConsumer] (主体滑动后弹性留白)
- [StayConsumer][StayConsumer] (滑动时主体保持不动，根据释放时的方向和速率来确定是否打开，可用于实现仿手机QQ的滑动返回效果)
- [StretchConsumer][StretchConsumer] (拉伸，仿MIUI中的拉伸效果)
- [TranslucentSlidingConsumer][TranslucentSlidingConsumer] (SpaceConsumer的进阶版，可实现侧滑删除的效果)
- [BezierBackConsumer][BezierBackConsumer] (贝塞尔曲线返回)
- [ActivityDoorBackConsumer][ActivityDoorBackConsumer] (Activity开门返回)
- [ActivityShuttersBackConsumer][ActivityShuttersBackConsumer] (Activity百叶窗返回)
- [ActivitySlidingBackConsumer][ActivitySlidingBackConsumer] (Activity透明侧滑返回)

**更多侧滑效果等你来添加！**


[SwipeDistanceCalculator]: /pages/SwipeDistanceCalculator.md
[公共属性]: /pages/consumers/common_settings.md
[Release处理方式]: /pages/releaseMode.md


[ActivityDoorBackConsumer]: /pages/consumers/ActivityDoorBackConsumer.md
[ActivityShuttersBackConsumer]: /pages/consumers/ActivityShuttersBackConsumer.md
[ActivitySlidingBackConsumer]: /pages/consumers/ActivitySlidingBackConsumer.md
[BezierBackConsumer]: /pages/consumers/BezierBackConsumer.md
[DoorConsumer]: /pages/consumers/DoorConsumer.md
[DrawerConsumer]: /pages/consumers/DrawerConsumer.md
[ShuttersConsumer]: /pages/consumers/ShuttersConsumer.md
[SlidingConsumer]: /pages/consumers/SlidingConsumer.md
[SpaceConsumer]: /pages/consumers/SpaceConsumer.md
[StayConsumer]: /pages/consumers/StayConsumer.md
[StretchConsumer]: /pages/consumers/StretchConsumer.md
[TranslucentSlidingConsumer]: /pages/consumers/TranslucentSlidingConsumer.md




