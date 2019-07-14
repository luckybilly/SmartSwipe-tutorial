## 公共属性
---

{{book.baseName}}作为所有consumer的基类，一些公共的设置属性将放在这个基类中进行设置及使用

### 1. 侧滑方向的启用与锁定

默认情况下，consumer未启用任何方向，必须至少启用一个方向consumer才会开始工作

默认情况下，consumer未锁定任何方向，被锁定的方向即使enable为true也不可被手势事件触发侧滑（用户无法拖动），但可通过代码执行侧滑事件。

变量名称|类型|取值范围|默认值|set方法|get方法|备注
:---:|:---|:---:|:---:|:---|:---|:---
mEnableDirection|int|0~15|0|enableDirection(dir)<br/>enableLeft()<br/>enableRight()<br/>enableTop()<br/>enableBottom()<br/>enableHorizontal()<br/>enableVertical()<br/>enableAllDirections()<br/><br/>disableDirection(dir)<br/>disableLeft()<br/>disableRight()<br/>disableTop()<br/>disableBottom()<br/>disableHorizontal()<br/>disableVertical()<br/>disableAllDirections()<br/>|isDirectionEnable(dir)<br/>isLeftEnable()<br/>isRightEnable()<br/>isTopEnable()<br/>isBottomEnable()<br/>isHorizontalEnable()<br/>isVerticalEnable()<br/>isAllDirectionsEnable()<br/>|表示可侧滑的方向<br/>若disable的方向包含当前侧滑的方向，则会自动close重置。<br/>参考：[侧滑方向][侧滑方向]
mLockDirection|int|0~15|0|lockDirection(dir)<br/>lockLeft()<br/>lockRight()<br/>lockTop()<br/>lockBottom()<br/>lockHorizontal()<br/>lockVertical()<br/>lockAllDirections()<br/><br/>unlockDirection(dir)<br/>unlockLeft()<br/>unlockRight()<br/>unlockTop()<br/>unlockBottom()<br/>unlockHorizontal()<br/>unlockVertical()<br/>unlockAllDirections()<br/>|isDirectionLocked(dir)<br/>isLeftLocked()<br/>isRightLocked()<br/>isTopLocked()<br/>isBottomLocked()<br/>isHorizontalLocked()<br/>isVerticalLocked()<br/>isAllDirectionsLocked()<br/>|表示锁定的方向<br/>被锁定的方向即使enable为true也不可被手势事件触发侧滑，但可通过代码执行侧滑事件。<br/>参考：[侧滑方向][侧滑方向]


### 2. 宽高

宽高属性指的是`SmartSwipeWrapper`的宽高尺寸

其初始值均为0，在`onMeasure`方法中分别被赋值为`mWrapper.getMeasuredWidth()`和`mWrapper.getMeasuredHeight()`

Tips: 

在`onMeasure`方法被调用前，通过`consumer.setLeftOpen()`、`consumer.smoothTopOpen()`等方法让{{book.name}}自动执行侧滑将不能生效。

如果`SmartSwipeWrapper`的宽高尺寸是确定值，可以提前为consumer设置宽高，从而实现在`onMeasure`方法被调用前执行自动侧滑的功能。**demo中的封面演示了这种用法。**


*getter/setter均一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mWidth|int|>=0|0|`SmartSwipeWrapper`的宽度
mHeight|int|>=0|0|`SmartSwipeWrapper`的高度

### 3. 位移相关属性

*getter/setter均一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mOpenDistance|int|>=0|20dp|从关闭状态到打开状态需要移动的像素值
mOverSwipeFactor|float|>=0F|0F|可以越界拖动的系数，越界的最大像素值为:<br/>mOpenDistance x mOverSwipeFactor<br/>可拖动的最大像素值为：<br/>mOpenDistance x (1 + mOverSwipeFactor)
mSwipeDistanceCalculator|[SwipeDistanceCalculator][SwipeDistanceCalculator]|-|null|侧滑距离计算器，对拖动的距离进行加工后返回UI需要呈现的距离，用以实现与手势拖动不同步的效果，如阻力效果
mEdgeSize|int|>=0|0|在控件边缘处一定范围内才能触发<br/>若为0，表示在整个控件区域都可触发
mSensitivity|float|>0|1F|灵敏度，数值越大越灵敏

### 4. 几个开关

*getter/setter都一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mDisableSwipeOnSettling|boolean|true/false|false|true表示在自动滑动过程中禁用手指触发侧滑
mDisableNestedScroll|boolean|true/false|false|true表示禁用手势拖动的NestedScroll触发侧滑
mDisableNestedFly|boolean|true/false|false|true表示禁用惯性滑动时的NestedScroll触发侧滑



### 5. 其它属性

*getter/setter都一一对应，不赘述*

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mInterpolator|Interpolator|-|SwipeHelper.sInterpolator|非手势事件拖动时动画侧滑的插值器
mReleaseMode|int|0~7|1|释放模式决定了在用户拖动触发侧滑后抬起手指时的处理策略<br/>参考：[Release处理方式][Release处理方式]
mTag|Object|所有值|null|如果有需要，可以为consumer设置一个标签


### 6. 可以获取但不能设置的属性


变量名称|类型|获取方式|取值范围|备注
:---:|:---|:---:|:---|:---
mWrapper|SmartSwipeWrapper|getWrapper()|-|当前consumer所添加到的wrapper
mSwipeHelper|SwipeHelper|getSwipeHelper()|-|为当前consumer捕获触发侧滑的touch事件管理工具
-|int|getDragState()|SwipeHelper.STATE_IDLE<br/>SwipeHelper.STATE_DRAGGING<br/>SwipeHelper.STATE_SETTLING<br/>SwipeHelper.STATE_NONE_TOUCH|获取当前consumer的侧滑状态
mProgress|float|getProgress()|0 ~ 1 + mOverSwipeFactor|当前侧滑的进度，0为关闭状态，1为打开状态
mDirection|int|getDirection()|0/1/2/4/8|当前侧滑的方向，只会是：无、左、右、上、下这5个值中的一种
mSwiping|boolean|isSwiping()|true/false|当前状态是否为：正在侧滑

### 7. 子类中可以使用的属性（不要修改）


变量名称|类型|备注
:---:|:---:|:---
mSwipeOpenDistance|int|从关闭状态到打开状态需要真实拖动的像素值(*此值是经过mSwipeDistanceCalculator计算后的mOpenDistance*)
mSwipeMaxDistance|int|最大拖动距离像素值，mSwipeMaxDistance = mSwipeOpenDistance * (1 + mOverSwipeFactor)
mOpenDistanceSpecified|boolean|true代表mOpenDistance是通过setOpenDistance(distance)手动设置的<br/>子类可以用此值来判断是否需要设置默认值
mCurDisplayDistanceX|int|当前UI需要呈现出X轴方向上移动的像素值（经过mSwipeDistanceCalculator计算后的值）
mCurDisplayDistanceY|int|当前UI需要呈现出Y轴方向上移动的像素值（经过mSwipeDistanceCalculator计算后的值）



[侧滑方向]: /pages/directions.md
[Release处理方式]: /pages/releaseMode.md
[SwipeDistanceCalculator]: /pages/SwipeDistanceCalculator.md
















