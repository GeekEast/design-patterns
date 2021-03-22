
- [Design Principles](#design-principles)
  - [Encapsulate what varies](#encapsulate-what-varies)
  - [Program to Interface, not an Implementation](#program-to-interface-not-an-implementation)
  - [Favor Composition Over Inheritance](#favor-composition-over-inheritance)

## Design Principles
核心：**隔离**`变化`；**解耦**`变化`；

| Principle    | Description                                              | Risk if not                                                |
| ------------ | -------------------------------------------------------- | ---------------------------------------------------------- |
| 封装变化     | 隔离程序中的可变成分，降低程序变动带来的时间成本         | 修改可变部分会引起其他部分的改动，时间成本上升             |
| 面向接口编程 | 类跟类之间的互动通过抽象类或者接口完成，而不是直接互动   | 写死的具体类的变动会可能引发依赖的频繁改动                       |
| 聚合优于继承 | 复杂的继承关系使得程序难以变动，尽量使用`组合`替代`继承` | 在复杂的继承关系下，一个类的改动可能会引起整个继承树的变动 |
### Encapsulate what varies

找到程序中的**变化内容**并将其与不变的内容区分开。该原则的主要目的是将**变更造成的影响最小化**

你可将程序的变化部分放入独立的模块中，保护其他代码不受负面影响。 最终，你只需花**较少时间**就能让程序恢复正常工作， 或是实现并测试修改的内容。 **你在修改程序上所花的时间越少，就会有更多时间来实现功能**。

这里代码的封装不是为了代码的复用, 而是**为了隔离可变部分, 降低程序变动带来的时间成本**。 例如税率计算部分变得异常庞大时，可以直接抽出来做成类。

- 修改前
```java
public Double getOrderTotal(Order order){
    double total = 0;

    // stable
    for (Item item: order.lineItems) {
        total += item.price * item.quantity;
    }

    // unstable
    if(order.country == "us")
        total += total * 0.07;

    if(order.country == "au")
        total += total * 0.06;

    return total;
}
```

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-18-36-50.png alt="no image found"></p>

- 抽离成`method`
```java
public double getOrderTotal(Order order){
    double total = 0;

    for (Item item: order.lineItems) {
        total += item.price * item.quantity;
    }

    return getOrderTotalAfterTax(total, order.country);
}

// 看似代码量增加了 但是风险降低了 因为unstable的部分我们单独提取出来了
double getOrderTotalAfterTax(Double total, String country) {
    double rate;
    switch (country){
        case "us":
            rate = 0.7;
        case "au":
            rate = 0.6;
        default:
            rate = 0.5;
    }
    return total * (1+rate);
}
```

- 抽离成`class` (业务逻辑更加复杂时)
```java
class Order  {
    Item[] lineItems;
    String country;
    double total;
    TaxCalculator taxCalculator;

    public Order(String country) {
        this.country = country;
        this.taxCalculator = new TaxCalculator(this.total, this.country);
    }

    double getTotal() {
        for (Item item: this.lineItems) {
            this.total += item.price * item.quantity;
        }
        return this.taxCalculator.getTotalAfterTax();
    }
}

class TaxCalculator {
    double total;
    String country;
    public TaxCalculator(double total, String country) {
        this.total = total;
        this.country = country;
    }

    double getTaxRate(String country) {
        return switch (country) {
            case "us" -> 0.07;
            case "au" -> 0.06;
            default -> 0.05;
        };
    }

    double getTotalAfterTax() {
        this.total += this.total * getTaxRate(this.country);
        return this.total;
    }
}
```


<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-18-37-09.png alt="no image found"></p>


### Program to Interface, not an Implementation
面向**接口**进行开发, 而不是面向**实现**。依赖于**抽象类型**, 而不是**具体类**。

当你需要两个类进行合作时， 可以让其中一个类依赖于另一个`接口`而不是`类`. 具体步骤为:

- 确定一个对象对另一对象的确切需求：它需执行哪些**方法**？(任何对象之间的交互都是通过`行为`, 不会直接交互任何的attribute)
- 在一个新的接口或抽象类中描述这些方法。
- 让被依赖的类实现该接口。
- 现在让有需求的类依赖于这个接口， 而不依赖于具体的类。 你仍可与原始类中的对象进行互动， 但现在其连接将会灵活得多。

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-18-57-21.png alt="no image found"></p>

一个对象依赖于另外一个对象时 仅关心对方**行为中自己需要的那部分** 并不**judge**对方是谁, 该对象的**合作范畴**和**合作灵活性**得以提高。

例2:
- 面向类的实现
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-19-03-00.png alt="no image found"></p>

- 面向接口的实现
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-19-15-17.png alt="no image found"></p>

- 进一步优化
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-23-02-45.png alt="no image found"></p>

基本原则: **一个类中在任何地方如果含有其他的类名，都要转换为接口或者抽象类来替代具体类。**


### Favor Composition Over Inheritance

继承是代码复用的一种常见方式, 但是`类的数量一旦非常多`，代码的修改就会变得特别困难。

继承有以下几个缺点:
- 子类无法减少父类接口数量。`子类必须实现父类中的所有抽象方法，即便在子类中该方法没有被使用过`。
- 子类重写父类方法时必须兼容父类中该方法的实现。需要传入`父类实例`为参数的方法中传入了一个`子类实例`，你需要保证code不报错。
- 继承打破了父类的封装。子类可以获取父类的实现细节，但是父类却无法获取子类的任何信息。
- 父类子类之间形成了强耦合。`任何父类的修改都有可能造成子类的崩溃`。
- 不同维度的类树组合会使得类的层次结构无比膨胀。(指数级增长)

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-19-23-57-46.png alt="no image found"></p>

组合是替代继承的一种方法。继承代表类之间为”是“的关系(汽车是交通工具)，而组合则代表类之间是”有“的关系(汽车有一个引擎)。

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-19-23-58-15.png alt="no image found"></p>

聚合与组合类似，但是**聚合是更松弛的组合的变体，一个对象可引用另外一个对象，但并不管理其生命周期**。例如: 一辆汽车上有司机，但是司机也可能会使用另外一辆汽车，或者选择步行而不使用汽车。




