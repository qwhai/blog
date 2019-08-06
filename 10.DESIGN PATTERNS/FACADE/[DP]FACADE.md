## 概述
今天要说的外观模式是一个相对简单的设计模式，而且在日常的开发中，可能你也会时常使用它，只是你可能并未想过这是一个设计模式。本文会从一些实例着手，来对本文要说明的外观模式进行尽可能全面的讲解。希望于你有益。

### 引言
这里插入一条引言的目的是让你回想一下，在你日常开发中何时用到了外观模式。
可能你的 boss 会这样安排你一个任务。这可能是一个核心模块，模块会有它的一个功能，只是你的 boss 可能只想要你提供一个给他调用的接口。他会这么跟你说：嗨，小明，我们现在的这个系统里需要一个核心的功能 P0，它就交给你实现吧。你只要给我可以调用的接口就好了，你代码的内部逻辑我不需要知道的。去做吧。
如果你时常被安排这样的任务，我想你应该已经掌握外观模式了。

### 定义
> 外观模式（Faceade）提供了一个统一的接口，用来访问子系统中的一群接口。外观模式定义一个高层接口，让子系统更容易使用。

## 版权说明
<font color="#FF0000">
著作权归作者所有。<br>
商业转载请联系作者获得授权，非商业转载请注明出处。<br>
本文作者：[Q-WHai](https://qwhai.blog.csdn.net/)<br>
发表日期： 2016年6月6日<br>
本文链接：[https://qwhai.blog.csdn.net/article/details/51592617](https://qwhai.blog.csdn.net/article/details/51592617)<br>
来源：CSDN<br>
更多内容：[分类 >> 设计模式](http://blog.csdn.net/u013761665/article/category/3171981)
</font>

## 非外观模式
这里我就使用《大话设计模式》书中的例子了，感觉这个例子还是挺形象的。现在假设你是一个股民（只是博主不炒股，也不知道这里会不会有说得不对的地方，如果有你就当没看见吧。。^_^），你想做一些理财活动。你看中了两支股票、一支国债和一支房地产。
如果让你现在来编写这份代码，你的代码框架可能会像下面的这幅类图：

![这里写图片描述](http://img.blog.csdn.net/20160606000807672)

这里只列举了股票的代码，是因为其他理财活动的逻辑与股票的逻辑是一致的。冗余的代码除了占地方之外并没有再多的好处了。
*StockA.java*
```java
public class StockA {

    private int stockCount = 0;

    public void sell(int count){
        stockCount -= count;
        System.out.println("卖了" + count + "支 A 股票");
    }

    public void buy(int count){
        stockCount += count;
        System.out.println("买了" + count + "支 A 股票");
    }

    public int getStockCount() {
        return stockCount;
    }
}
```
下面的代码是处理理财者的，对于一个简单地买进买出逻辑，理财者都要花费这么多的代码处理，实在是折磨人嘛。
*Investors.java*
```java
public class Investors {

    public static void main(String[] args) {
        StockA stockA = new StockA();
        StockB stockB = new StockB();
        NationalDebt debt = new NationalDebt();
        RealEstate estate = new RealEstate();

        stockA.buy(100);
        stockB.buy(200);
        debt.buy(150);
        estate.buy(120);

        stockA.sell(100);
        stockB.sell(200);
        debt.sell(150);
        estate.sell(120);
    }
}
```
上面说的这些是在没有使用外观模式的情况下编写的代码。也就是说下面要说的外观模式可以让代码更加简单明了了。

## 外观模式
在上面的非外观模式中，我们看到了一些不是很友好的代码逻辑。而外观模式可以基于更高层次的封装，从而达到对调用者更加透明。下面是修改后的外观模式类图：

![这里写图片描述](http://img.blog.csdn.net/20160606000821019)

*FundFacade.java*
```java
public class FundFacade {

    private StockA stockA = null;
    private StockB stockB = null;
    private NationalDebt debt = null;
    private RealEstate estate = null;

    public FundFacade() {
        stockA = new StockA();
        stockB = new StockB();
        debt = new NationalDebt();
        estate = new RealEstate();
    }

    public void buyAll(int count) {
        stockA.buy(count);
        stockB.buy(count);
        debt.buy(count);
        estate.buy(count);
    }

    public void sellAll(int count) {
        stockA.sell(count);
        stockB.sell(count);
        debt.sell(count);
        estate.sell(count);
    }

    public void buyStockA(int count) {
        stockA.buy(count);
    }

    public void sellNationalDebt(int count) {
        debt.sell(count);
    }
}
```
上面的代码则是外观的核心类：FundFacade。在理财系统中的所有操作都可以通过这个类来实现。你通过这个类可以很方便地实现对股票、国债、房地产等理财项目的操作，而不用关心里面究竟是怎么处理的。这是对用户来说是一件好事，不是吗？
来看看用户的操作吧（当然上面图类已经反映了绝大部分效果了），这是用户的代码逻辑：
*Investors.java*
```java
public class Investors {

    public static void main(String[] args) {
        FundFacade facade = new FundFacade();
        facade.buyAll(120);
        facade.buyStockA(50);
        facade.sellAll(80);
    }
}
```
看看，用户只要告诉 FundFacade 类，买入什么、卖出什么、买多少、卖多少，就可以达到目的。实在是太方便了。
看看股票 A 的代码吧，其实它并什么实质性的变化。这也是外观模式的魅力所在，它不用你去修改原来子系统的代码，只要做一件事，构建更高层次的封装。当然，这里我做了一些简单地修改，StockA 的访问权限。既然要对用户透明，那么我的子系统就没有必要再对用户开放了，不是吗？因为我们已经有专业的外交官—— FundFacade。
*StockA.java*
```java
class StockA {

    private int stockCount = 0;

    void sell(int count){
        stockCount -= count;
        System.out.println("卖了" + count + "支 A 股票");
    }

    void buy(int count){
        stockCount += count;
        System.out.println("买了" + count + "支 A 股票");
    }

    int getStockCount() {
        return stockCount;
    }
}
```
外观模式是一个相对简单的设计模式，你可以很轻松地掌握并使用它。只是我想说，外观模式也会存在一定的局限性。相信你已经发现了。
由于我们把对于子系统所有的操作都交给了 FundFacade 类来处理，所以我们就受到了 FundFacade 类的约束了。比如上面的 FundFacade 类中并没有实现单独对 StockB 的操作，那么我们就不能单独对 StockB 进行操作了，除非你在 FundFacade 类中封装一个对 StockB 操作的接口。

## 外观模式的应用
上面的描述中我们不仅知道了如何使用外观模式，也了解了外观模式的局限，所以我们应该站在客观的立场，有选择性地使用它。这里说一个我在工作中使用外观模式的例子吧。
目前项目的老大让我去实现一个系统中的某一个模块，我想这应该是一个核心模块吧。这个模块的功能是，检查一个文件夹下的所有文件是否包含了敏感信息。而这个模块中会有很多小的子模块（当然老大并不会关心这些子模块做的事情），比如 AC 自动机的模式匹配、压缩文件的全自动解压、各种格式文件（doc/xls/ppt/zip/eml/rtf/pdf 等等，绝大部分的文件格式基本都在吧）、日志系统等等。
我不可能去跟老大说，你要完成的功能是要先去干嘛、再去干嘛、再去干嘛、再去干嘛... ...
哦，天啦。烦死了，你能对它封装一下吗？（当然，这些只是我的心理活动。事实上，我还没有让老大说明我的设计过程）
封装过后，我只要告诉老大，去调用这个类的这个方法就 ok 了。这样老大那边就不用操心里面的逻辑了，虽然如果出了错就是你的责任，可那也本该就 是你的责任啊。哈哈。。。
好了，扯蛋就到这里。不管是上面正儿八经地模式详解，还是下面的胡说八道，我都希望它可以让你充分了解本文这个设计模式，学习并合理使用它。

## Ref
- [http://blog.csdn.net/jason0539/article/details/22775311](http://blog.csdn.net/jason0539/article/details/22775311)
- 《大话设计模式》
