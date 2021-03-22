- [OOP](#oop)
  - [Abstraction](#abstraction)
  - [Encapsulation](#encapsulation)
  - [Inheritance](#inheritance)
  - [Polymorphism](#polymorphism)

## OOP

| **Pillar**        | **Description**                       |
| :---------------: | ------------------------------------- |
| **Abstraction**   | 从现实世界中抽取当前context下`所需特征` |
| **Encapsulation** | 隐藏部分状态行为，暴露`有限`接口        |
| **Inheritance**   | `代码复用`的一种方式                    |
| **Polymorphism**  | 检测对象所属`实际类`, 调用其方法      |
### Abstraction
抽象是一种反映真实世界**对象**或**现象中**特定内容的**模型**, 它能高精度地反映所有与特定内容相关的详细信息, 同时**忽略其他内容**。
- 模型是对现实世界物象特性的提取
- 提取哪些特征是根据实际业务场景决定 会忽略掉现实中存在但是与业务场景无关的特征

例如: `飞行模拟器`和`航班预订`程序中都可能会包含一个**Airplane**类。 但是前者需包含与实际**飞行**相关的详细信息m, 而后者则只关心**座位图**和哪些座位可供预订。
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-17-23-19-19.png alt="no image found"></p>

### Encapsulation
封装是指一个对象对其他对象**隐藏**其部分状态和行为, 而仅向程序其他**部分**暴露`有限的接口`的能力。


<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-17-23-38-57.png alt="no image found"></p>


### Inheritance
继承是指在根据已有类创建新类的能力。 继承最主要的好处是`代码复用`。
- 父类实现一接口, 所有子类都必须实现该接口
- 子类必须实现父类的抽象方法
- 子类无法隐藏已经在父类中的具体方法，只能override
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-09-25-09.png alt="no image found"></p>

### Polymorphism
**多态**是指程序能够检测对象所属的`实际类`， 并在当前上下文不知道其真实类型的情况下调用其实现的能力。

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-09-30-27.png alt="no image found"></p>

```java
Bag bag = [new Cat(), new Dog()];

// 遍历每一个Animal而不是每一个不同类型的animal
// Animal是个统称
for (Animal a: bag) {
    a.makeSound()
}
```


