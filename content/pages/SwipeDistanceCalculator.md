## SwipeDistanceCalculator
---

侧滑距离计算器

默认情况下，用户手指移动的距离就是UI需要呈现的侧滑距离。

某些情况下我们并不想这样，比如要实现阻力滑动效果，实际UI展示的移动距离占手指移动距离的60%。

这时我们就需要在侧滑事件触发移动后，在consumer消费这个侧滑距离之前，对它进行一次加工，并将加工后的距离交给consumer去消费：展示到UI上。

接口定义如下：

```java
public interface SwipeDistanceCalculator {
    /**
     * 计算侧滑距离
     * @param swipeDistance 侧滑的距离
     * @param progress 当前侧滑进度，其取值范围为 [0, 1 + consumer.getOverSwipeFactor())]
     * @return 展示在UI上的侧滑距离
     */
    int calculateSwipeDistance(int swipeDistance, float progress);

    /**
     * 作为calculateSwipeDistance的反向运算，这个方法是要计算使用这个计算器后，UI从关闭状态到打开状态，所需侧滑的距离
     * @param openDistance 不使用计算器时，打开状态所需侧滑的距离
     * @return 从关闭状态到打开状态所需侧滑的距离
     */
    int calculateSwipeOpenDistance(int openDistance);
}
```

{{book.name}}中内置了一个带阻力效果的计算器

```java
public class ScaledCalculator implements SwipeDistanceCalculator {

	//阻力系数
    private float mScale;

    public ScaledCalculator(float scale) {
        if (scale <= 0) {
            throw new IllegalArgumentException("scale must be positive");
        }
        this.mScale = scale;
    }

    @Override
    public int calculateSwipeDistance(int swipeDistance, float progress) {
    	//对实际侧滑的距离作阻力计算
        return (int) (swipeDistance * mScale);
    }

    @Override
    public int calculateSwipeOpenDistance(int openDistance) {
    	//计算从关闭状态到打开状态所需侧滑的距离
        return (int) (openDistance / mScale);
    }
}
```



