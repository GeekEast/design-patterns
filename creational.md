## Creational Patterns

### Factory Pattern
使用`工厂`来集中管理对象创建的`复杂逻辑`
结构:
<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-20-15-48-24.png alt="no image found"></p>

- case: 下pizza订单的时候回根据type生成不同类型的Pizza对象(复杂逻辑); 
- **所有代码都在client一端, Pizza类别的变动会引发client代码的改动， 违背单一职能原则，开闭原则**
```java
// client of creating Pizza objects
class PizzaStore {
    Pizza orderPizza(String type) {
        Pizza pizza;

        // code that will vary in the future
        if(type == "A") {
            pizza = new CheesePizza();     // extends class Pizza
        } else if ( type == "B") {
            pizza = new GreekPizza();      // extends class Pizza
        } else {
            pizza = new PepperoniPizza();  // extends class Pizza
        }

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}

abstract class Pizza {
    public abstract void prepare();
    public abstract void bake();
    public abstract void cut();
    public abstract void box();
}
```
- 封装orderPizza中会发生变动的部分: PizzaStore现在是close for modification, 提高了代码的复用性。
- 但是目前`SimplePizzaFactory`是写死在`PizzaStore`里的, 我无法在创建`PizzaStore`的时候指定具体的`SimplePizzaFactory`的具体类, 比如有一个类是 `SubSimplePizzaFactory` 继承了 `SimplePizzaFactory`, 我必须修改代码才可以使用`SubSimplePizzaFactory`
```java
// extract creating Pizza logic to another class or `Factory`
class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza;
        if(type == "A") {
            pizza = new CheesePizza();
        } else if (type == "B") {
            pizza = new GreekPizza();
        } else {
            pizza = new PepperoniPizza();
        }
        return pizza;
    }
}


class PizzaStore {
    Pizza orderPizza(String type) {
        Pizza pizza;

        SimplePizzaFactory spf = new SimplePizzaFactory();
        pizza = spf.createPizza("A");

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}

abstract class Pizza {
    public abstract void prepare();
    public abstract void bake();
    public abstract void cut();
    public abstract void box();
}
```
- 进一步优化, `PizzaStore(Client)` **Aggregate** `SimplePizzaFactory`, 这样我可以在创建`PizzaStore`对象时时指定`SimplePizzaFactory`或者其子类的实例
- 但是目前`SimplePizzaFactory`是个具体类, 违背了`依赖倒置原则`
```java
class PizzaStore() {
    // 将`SimplePizzaFactory`作为client的成员变量
    SimplePizzaFactory simplePizzaFactory;

    public PizzaStore(SimplePizzaFactory simplePizzaFactory) {
        this.simplePizzaFactory = simplePizzaFactory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;

        pizza = this.simplePizzaFactory().createPizza("A");
        
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}


class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza;
        if(type == "A") {
            pizza = new CheesePizza();
        } else if (type == "B") {
            pizza = new GreekPizza();
        } else {
            pizza = new PepperoniPizza();
        }
        return pizza;
    }
}

abstract class Pizza {
    public abstract void prepare();
    public abstract void bake();
    public abstract void cut();
    public abstract void box();
}
```
- 进一步优化: 只要实现了接口方法的类都可以作为`PizzaFactory`传入到`PizzaStore`当中
```java
interface PizzaFactory {
    Pizza createPizza(String type);
}

public class PizzaStore {
    PizzaFactory pf;

    // 面向接口
    public PizzaStore(SimplePizzaFactory pf) {
        this.pf = pf;
    }

    Pizza orderPizza(String type) {
        Pizza pizza = this.pf.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}

public class SimplePizzaFactory implements PizzaFactory {
    @Override
    public Pizza createPizza(String type) {
        Pizza pizza;

        // code that will vary in the future
        if(type.equals("A")) {
            pizza = new CheesePizza();     // extends class Pizza
        } else if (type.equals("B")) {
            pizza = new GreekPizza();      // extends class Pizza
        } else {
            pizza = new PepperoniPizza();  // extends class Pizza
        }

        return pizza;
    }
}
```

<p align="center"><img style="display: block; width: 600px; margin: 0 auto;" src=img/2021-03-22-18-55-50.png alt="no image found"></p>