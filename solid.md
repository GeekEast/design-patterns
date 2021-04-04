- [SOLID Principle](#solid-principle)
  - [Single Responsibility Principle](#single-responsibility-principle)
  - [Open/closed Principle](#openclosed-principle)
  - [Liskov Substitution Principle](#liskov-substitution-principle)
  - [Interface Segregation Principle](#interface-segregation-principle)
  - [Dependency Inversion Principle](#dependency-inversion-principle)

## SOLID Principle

与生活中所有事情一样, 盲目遵守这些原则可能会**弊大于利**。 在程序架构中应用这些原则可能会使其变得过于复杂。 **我对于是否真的有能够同时应用所有这五条原则的成功软件产品表示怀疑**。 有原则是件好事， 但是也要时刻从**实用**的角度来考量， 不要把这里的每句话当作放之四海皆准的教条。

| Principle | Description                                                      | Risk If Not                                      | Category    | Solutions    |
| :-------: | ---------------------------------------------------------------- | ------------------------------------------------ | ----------- | ------------ |
| S         | 一个类中不要混跟本类无关的代码                                   | 无关部分的变动会引起该类改动                     | Class       | 隔离无关部分 |
| O         | 功能增加时，**写好的类不要改动**，通过拓展`继承`来增加新功能     | 类一旦修改, 依赖于该类的其他类可能就无法正常运行 | Class       | 隔离变动部分 |
| L         | 子类要兼容父类, 涉及到成员变量，方法参数，方法返回值和方法内异常 | 在要求传入父类的地方传入了子类可能会出错         | Inheritance | 子类兼容父类 **OR** 组合优于继承 |
| I         | 接口方法不要全写在一个接口内，要适当细分                         | 一些类要强制实现一些自己并不需要的接口类方法     | Interface   |      |
| D         | 高层次不要直接依赖低层次代码, 中间设置接口                       | 低层次类中的改动可能会引起高层次类的变动         | Interface   |  |


### Single Responsibility Principle

修改一个类的原因只能有一个。（代码分割的原则）


修改前: 修改打印时间表的方法会引起对雇员类的修改，实际上二者并无关联
<p align="center"><img style="display: block; width: 60%; margin: 0 auto;" src=img/2021-03-20-08-34-20.png alt="no image found"></p>


修改后： 将打印时间表的功能分割成另外一个类
<p align="center"><img style="display: block; width: 60%; margin: 0 auto;" src=img/2021-03-20-08-34-54.png alt="no image found"></p>

### Open/closed Principle

对于扩展，类应该是“开放”的；对于修改，类则应是“封闭”的。

本原则的主要理念是在`实现新功能时能保持已有代码不变`。

类对于拓展开放: 如果你可以对一个类进行扩展， 可以创建它的**子类**并对其做任何事情（如新增方法或成员变量、 重写基类行为等），那么它就是"开放"的。有些编程语言允许你通过特殊关键字（例如 `final`） 来限制对于类的进一步扩展， 这样类就不再是“开放”的了。

类对于修改封闭: 如果某个类已做好了`充分的准备`并可供其他`类`使用的话（即其`接口已明确定义且以后不会修改`）， 那么该类就是封闭（你可以称之为完整）的。

如果一个类已经完成开发、测试和审核工作， 而且属于某个框架或者可被其他类的代码直接使用的话， 对其代码进行修改就是有风险的。 你**可以创建一个子类并重写原始类的部分内容以完成不同的行为， 而不是直接对原始类的代码进行修改**。 这样你既可以达成自己的目标， 但同时又无需修改已有的原始类客户端。

通俗理解: 如果类已经写好，而且正在被其他类调用，不要直接修改已经写好的类，而是在原有基础上进行拓展。 但是如果是类本身出了问题，还是要对类的代码进行修改的。

修改前: shipping是hard-code进订单类的，如果要添加新的运输方式就要修改订单类
<p align="center"><img style="display: block; width: 60%; margin: 0 auto;" src=img/2021-03-20-09-32-43.png alt="no image found"></p>

修改后: 将shipping抽出来做成接口, 每次添加新的运输方式时，作为参数传入订单类的构造器即可
<p align="center"><img style="display: block; width: 60%; margin: 0 auto;" src=img/2021-03-20-09-32-57.png alt="no image found"></p>

总而言之，是为了减少对既有代码的改动的一个原则
### Liskov Substitution Principle

当你扩展一个类时，记住你应该要能在不修改客户端代码的情况下将`子类的对象`作为`父类对象`进行传递。

这意味着子类必须保持与父类行为的**兼容**。 在重写一个方法时， 你要对基类行为进行**扩展**， 而不是将其**完全替换**。

具体要求:
- 子类方法的`参数类型必须`与其超类的参数类型`相匹配`或`更加抽象`。
  - 子类不应该加强其前置条件。比如子类要求传入`正数`，而父类要求传入`int`。(✅ in TypeScript)
  - 子类不能削弱其后置条件。比如父类有方法在使用后会**关闭**所有活跃的数据库连接，子类中却**保持**了活跃的数据库连接。(❌ in TypeScript)
- 子类方法的`返回值类型`必须与超类方法的`返回值类型`或是其`返回值子类别`相匹配。(✅ in TypeScript)
- 子类方法中的`异常`必须与超类方法中`异常`匹配或者超类方法中`异常子类别`相匹配。(❌ in Typescript)
- 超类的不变量必须保留。超类中的常量或者固定成员变量继承到子类时，不要直接修改，而要保留，有需要再拓展。(❌ in TypeScript )
- 子类不能修改超类中私有成员变量的值。(✅ in TypeScript)

举例

修改前:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-10-55-20.png alt="no image found"></p>

只读文件`ReadOnlyDocuments`子类中的`save`保存方法会在被调用时抛出一个`异常`。 基础方法则没有这个限制。 这意味着如果我们没有在保存前检查文档类型， 客户端代码将会出错。

代码也将违反开闭原则， 因为客户端代码将依赖于**具体的**文档类。 如果你引入了新的文档子类， 则需要修改客户端代码才能对其进行支持。

修改后:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-11-34-43.png alt="no image found"></p>

你可以通过重新设计类层次结构来解决这个问题：一个子类必须扩展其超类的行为， 因此只读文档变成了层次结构中的基类。 可写文件现在变成了子类， 对基类进行扩展并添加了保存行为。

### Interface Segregation Principle

客户端不应被强迫依赖于其不使用的方法。尽量缩小接口的范围， 使得客户端的类不必实现其不需要的行为。

通俗理解: 接口要尽量细分，不要合起来。

修改前:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-12-17-41.png alt="no image found"></p>

修改后:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-12-17-58.png alt="no image found"></p>

### Dependency Inversion Principle

高层次的类不应该依赖于低层次的类。 两者都应该依赖于抽象接口。 抽象接口不应依赖于具体实现。 具体实现应该依赖于抽象接口。

- 低层次的类实现`基础操作`（例如磁盘操作、传输网络数据和连接数据库等）
- 高层次类包含`复杂业务逻辑`以指导低层次类执行特定操作

一般来说，人们会先设计低层次的类，然后才会开发高层次的类。如果采取这样的方式，业务逻辑类可能会依赖于低层次的类。

依赖倒置原则建议改变这种方式。
- 高层次的类依赖接口
- 低层次的类实现接口

修改前:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-12-26-35.png alt="no image found"></p>

修改后:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-12-28-04.png alt="no image found"></p>
