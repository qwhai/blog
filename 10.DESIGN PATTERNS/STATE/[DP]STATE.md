## 概述
很多人在说状态模式的时候总拿*策略模式*来进行对比，可能他们的类图会有一点类似，可我却不认为他们有多么相像。你可以阅读《[Java设计模式——策略模式](https://qwhai.blog.csdn.net/article/details/45894511)》这篇博客，并与本文对比，以找到蛛丝马迹。
他们最根本的差异在于策略模式是在求解同一个问题的多种解法，这些不同解法之间毫无关联；状态模式则不同，状态模式要求各个状态之间有所关联，以便实现状态转移。

## 定义
> 状态模式（State），当一个对象的内部状态改变时允许改变其行为，这个对象看起来像是改变了其类。

## 版权说明
<font color="#FF0000">
著作权归作者所有。<br>
商业转载请联系作者获得授权，非商业转载请注明出处。<br>
本文作者：[Q-WHai](https://qwhai.blog.csdn.net/)<br>
发表日期： 2016年6月6日<br>
本文链接：[https://qwhai.blog.csdn.net/article/details/51596556](https://qwhai.blog.csdn.net/article/details/51596556)<br>
来源：CSDN<br>
更多内容：[分类 >> 设计模式](http://blog.csdn.net/u013761665/article/category/3171981)
</font>

## 状态模式
我很喜欢具有状态转移的程序，总是感觉这里充满了无限的魅力。如果你也对状态转移的逻辑感兴趣，那么你可以阅读一下我之前的几篇博客。

-   [算法之动态规划初步（Java版）](https://qwhai.blog.csdn.net/article/details/47193671)
-   [算法：模式匹配之KMP算法](https://qwhai.blog.csdn.net/article/details/48488813)
-   [深入理解Aho-Corasick自动机算法](https://qwhai.blog.csdn.net/article/details/49335051)
-   [Trie树进阶：Double-Array Trie原理及状态转移过程详解](https://qwhai.blog.csdn.net/article/details/49281865)
-   [算法：关于生成抽样随机数的这些算法](https://qwhai.blog.csdn.net/article/details/50378102)


### 情境
看《Java 设计模式》的时候，我看到一个例子，感觉很好，拿来跟大家一起分享一下。
实体是电梯，这个大家一定不陌生。我们知道电梯主要有4种状态：电梯门关闭、电梯门打开、电梯上下运载、电梯停止。而且我们知道，电梯在门打开的时候，只能是关闭电梯门，不能是其他的任何操作。在学习状态模式之前，如果我们要编写这个逻辑，一定是长篇累读地 if ... else ... 。而且逻辑混乱，很难维护。当然，这里你可以使用 if ... else ...，因为电梯的这些状态基本是稳定的，不会有什么变动。而如果你的需求里，状态会不断更新，而你之前使用 if ... else ... 埋下的患根这时就会让你苦不堪言。
所以，你需要重构你的代码。

### 状态模式类图

![这里写图片描述](http://img.blog.csdn.net/20160606164019834)

### 逻辑实现
如果想要避免使用 if ... else ... 或是 switch ... case ...，那么我们就需要对这些条件进行封装。在学习状态模式之前，我很喜欢使用一个 Map 来解决 switch ... case ... 问题，而且屡试不爽。从使用 Map 来解决 switch ... case ... 问题中可以知道，这里的条件类必须去继承一个共同的类或是共同的接口。这里就是上面类图中的 LiftState。
*LiftState.java*
```java
public abstract class LiftState {

    protected Context context;

    public void setContext(Context _context) {
        this.context = _context;
    }

    public abstract void open();

    public abstract void close();

    public abstract void run();

    public abstract void stop();
}
```
可能你很奇怪，为什么这里 LiftState 类里面会有一个 Context 对象。它的作用是去调节状态的变化，它就是电梯，你的电梯状态肯定是针对电梯来说的，所以组合一个 Context 一点也不奇怪。

现在来看看 LiftState 的实现类吧，就拿 StoppingState 类来说吧，其他的实现跟这个类很像，就不多贴代码了。想要详细代码的朋友可以去我的 GitHub 上下载。
*StoppingState.java*
```java
public class StoppingState extends LiftState {

    @Override
    public void close() {
        // do nothing;
    }

    @Override
    public void open() {
        super.context.setLiftState(Context.openningState);
        super.context.getLiftState().open();
    }

    @Override
    public void run() {
        super.context.setLiftState(Context.runningState);
        super.context.getLiftState().run();
    }

    @Override
    public void stop() {
        System.out.println("电梯停止了...");
    }
}
```
在停下来的时候，我们不能让电梯关闭，因为它原本就是关闭的，我这里做法是不处理，当然你可以选择抛出异常。当电梯停下来的时候，电梯是可以打开的，所以在 open() 方法里可以将电梯的状态标识为打开状态；当然，也可以标识为运载状态。而究竟会转换成哪一种状态，就要依据实际乘客的使用情况了。
下面看看我们的关键实体 Context 是怎么实现的。
*Context.java*
```java
public class Context {

    // 定义出所有的电梯状态
    public final static OpenningState openningState = new OpenningState();
    public final static ClosingState closeingState = new ClosingState();
    public final static RunningState runningState = new RunningState();
    public final static StoppingState stoppingState = new StoppingState();

    // 定一个当前电梯状态
    private LiftState liftState;

    public LiftState getLiftState() {
        return liftState;
    }

    public void setLiftState(LiftState liftState) {
        this.liftState = liftState;
        // 把当前的环境通知到各个实现类中
        this.liftState.setContext(this);
    }

    public void open() {
        this.liftState.open();
    }

    public void close() {
        this.liftState.close();
    }

    public void run() {
        this.liftState.run();
    }

    public void stop() {
        this.liftState.stop();
    }
}
```
Context 组合了所有状态，这一点不奇怪，因为它是电梯嘛。在上面的代码中，你们可能很迷惑，这里 Context 都是去调用 LiftState 接口的相应方法，哪里体现了状态的转移呢？其实状态转移的逻辑是在各自的状态里面进行的，就像上面的 StoppingState 类。如果调用了 StoppingState 类，是不是说当前 Context 里的状态是 StoppingState 呢？而它却在 open() 方法里将 Context 的状态转换成了 OpenningState 。这样就完成了状态的转换了。Context 类的作用我想只是去触发状态的转换。
下面提供一张电梯的状态转移图：

![这里写图片描述](http://img.blog.csdn.net/20160606164128757)

## Ref
-   《Java 设计模式》

## GitHub 源码
https://github.com/qwhai/design-pattern

-----------------------------------
