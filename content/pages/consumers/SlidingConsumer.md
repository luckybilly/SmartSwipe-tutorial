## SlidingConsumer
---

SlidingConsumer 继承自 [DrawerConsumer][DrawerConsumer]

抽屉效果，抽屉显示在被包裹的控件下层(即contentView覆盖在抽屉之上显示)

抽屉与contentView之间可以联动，联动系数可以在0至1之间设置


如下图所示：


<div align=center><img src="/images/slidingConsumer.gif"><br/><br/></div>

### 示例代码

```java
SmartSwipe.wrap(view)
    .addConsumer(new SlidingConsumer())
    .setHorizontalDrawerView(textView)
    .setScrimColor(0x2F000000)
    ;
```

### 联动系数
联动：指的是抽屉view在主体contentView侧滑时发生的关联移动

联动系数：指的是控制联动的系数

联动系数|联动效果
:---:|:---
0|抽屉view在下层保持不动，contentView移动后逐步显示下面的抽屉view，最终完全打开时完全显示抽屉view
0~1|抽屉view与contentView同步移动，<font color=blue>移动速率不一样</font>，最终完全打开时完全显示抽屉view
1|抽屉view与contentView同步移动，<font color=blue>移动速率完全相同</font>，最终完全打开时完全显示抽屉view


### 抽屉view的尺寸可扩展性

当[公共属性][公共属性]中的`mOverSwipeFactor`大于0时，侧滑的最大尺寸可能会大于抽屉view的尺寸，当拖动的尺寸大于抽屉view的尺寸时，出现的情况有以下几种：

mOverSwipeFactor|mDrawerExpandable|mEdgeAffinity|效果
:---:|:---|:---:|:---
0|无效|无效|正常拖动，无特殊效果
>0|false|true|抽屉view会一直呆在SmartSwipeWrapper的边缘不动，抽屉view与contentView之间留白
>0|false|false|抽屉view会跟着contentView一起被拖动，边缘留白
>0|true|无效|抽屉view会一直呆在SmartSwipeWrapper的边缘不动，抽屉view的layout尺寸将扩展填充满contentView与边缘的空间

### 属性设置

继承自 [DrawerConsumer][DrawerConsumer]，抽屉View等参数的设置与之相同

变量名称|类型|取值范围|默认值|备注
:---:|:---|:---:|:---:|:---
mRelativeMoveFactor|float|0F~1F|0.5F|联动系数
mDrawerExpandable|boolean|true/false|false|抽屉view的尺寸是否可扩展
mEdgeAffinity|boolean|true/false|false|是否边缘亲和














[公共属性]: /pages/consumers/common_settings.md
[DrawerConsumer]: /pages/consumers/DrawerConsumer.md