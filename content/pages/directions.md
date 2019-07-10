## Swipe Direction
---

{{book.name}}只处理上下左右4个方向的侧滑事件，在{{book.name}}中，对滑动方向的定义只能是下表方向的一种或者几种的组合


方向|常量名称|取值|备注
:---:|:---|:---:|:---
无|SwipeConsumer.DIRECTION_NONE|0x0|无方向
左|SwipeConsumer.DIRECTION_LEFT|0x1|从左向右的侧滑
右|SwipeConsumer.DIRECTION_RIGHT|0x2|从右向左的侧滑
上|SwipeConsumer.DIRECTION_TOP|0x4|从上向下的侧滑
下|SwipeConsumer.DIRECTION_BOTTOM|0x8|从下向上的侧滑

为了方便，已经内置了3种组合对应的常量

方向|常量名称|取值|备注
:---:|:---|:---:|:---
水平方向(左&右)|SwipeConsumer.DIRECTION_HORIZONTAL|0x3|左右2个方向的侧滑
垂直方向(上&下)|SwipeConsumer.DIRECTION_VERTICAL|0x12|上下2个方向的侧滑
全部(上&下&左&右)|SwipeConsumer.DIRECTION_ALL|0x15|上下左右4个方向的侧滑



