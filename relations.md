- [Relations Between Objects](#relations-between-objects)
  - [Dependency](#dependency)
  - [Association](#association)
  - [Aggregation](#aggregation)
  - [Composition](#composition)
- [Summary](#summary)

## Relations Between Objects
### Dependency
一个类的定义的修改会引起另外一个类的修改, 例如: `一个类的方法传入另外一个类的方法作为参数`

- [ ] 方法参数抽象成接口或者抽象类, 可以降低依赖程度

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-10-19-50.png alt="no image found"></p>


```java
public class Professor {
    // association: if you change remember() in the Student Class, Professor will also need to be updated.
    Student student;

    // dependency: if you change getKnowledge() in Course Class, Professor will also need to be updated.
    public void teach(Course course) {
        this.student.remember(course.getKnowledge());
    }
}
```

### Association
一个类会use或interact with另外一个类，是一种特殊的dependency. 

类跟类之间的关联是永久性的, 
例如：`classA中有成员变量ClassB`

- [ ] 成员变量抽象成接口或者接口类, 可以降低依赖程度


<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-10-25-03.png alt="no image found"></p>



```java
public class Professor {
    // association: if you change remember() in the Student Class, Professor will also need to be updated.
    Student student;

    // dependency: if you change getKnowledge() in Course Class, Professor will also need to be updated.
    public void teach(Course course) {
        this.student.remember(course.getKnowledge());
    }
}
```

### Aggregation
其中一个对象由一个或多个其他对象实例构成, **组件对象可以是独立存在的**。 是一种特殊的的Association

例如: 一个学院包含很多个教授

- [ ] 成员变量抽象成接口或者接口类, 可以降低依赖程度

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-13-47-23.png alt="no image found"></p>


### Composition

其中一个对象由一个或多个其他对象实例构成, **组件对象无法独立存在**。是一种特殊的的Aggregation

例如: 一本书有很多页

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-14-16-34.png alt="no image found"></p>

> 经常说的"组合优于继承"，其实应该是"聚合优于继承"

## Summary

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-18-14-32-13.png alt="no image found"></p>


- **Dependency**:  **class A** changes **affect** **class B**
  - **Association**: ObjectA knows ObjectB
    - **Aggregation**: ObjectA consists of ObjectB
      - **Composition**:  ObjectA manages ObjectB 

  - **Polymorphism**: **ObjectA** can be seen as **ObjectB**
    - **Implementation**: ClassA's behaviours are declared by **InterfaceB**
    - **Inheritance**: ClassA inherits ClassB and implements ClassB's interface. 