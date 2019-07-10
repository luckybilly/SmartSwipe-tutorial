# 侧滑
上下左右4个方向的手势滑动

# 侧滑事件
触发侧滑的事件，整个过程从MotionEvent.ACTION_DOWN到MotionEvent.ACTION_UP（或MotionEvent.ACTION_CANCEL）的整个过程

# overSwipeFactor
float类型，打开SwipeUI后能越界继续拖动的系数

# progress
当前SwipeUI的打开完成度，float类型，取值范围为(0 ~ 1 + overSwipeFactor)，关闭状态为0，打开状态为1