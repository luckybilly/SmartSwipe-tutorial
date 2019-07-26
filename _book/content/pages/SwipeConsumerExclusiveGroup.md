## SwipeConsumerExclusiveGroup

管理一组{{book.baseName}},在这个组内的{{book.baseName}}打开状态是互斥的：**同时只能有0个或1个{{book.baseName}}处于打开状态，打开一个，其它的都将自动关闭**

### 构造方法

```java
/**
 * 创建一个SwipeConsumerExclusiveGroup，指定{{book.baseName}}自动关闭时是否平滑关闭
 * @param smooth 是否平滑关闭(true:平滑动画关闭, false: 立即关闭)
 */
public SwipeConsumerExclusiveGroup(boolean smooth) {
    this.smooth = smooth;
}
public SwipeConsumerExclusiveGroup() {
    this.smooth = true;
}
```

### 将{{book.baseName}}添加到SwipeConsumerExclusiveGroup组中
```java
consumer.addToExclusiveGroup(group);
//或者
group.add(consumer);
```
### 将一个{{book.baseName}}从组内移除
```java
group.remove(consumer);
```

### 手动将组内的所有{{book.baseName}}全部关闭
```java
group.markNoCurrent();
```
### 清除组内的所有{{book.baseName}}
```java
group.clear();
```
### 锁定其它的consumer
若当前某个consumer已打开，在关闭它之前，其它{{book.baseName}}无法开启
```java
group.setLockOther(true);
```

