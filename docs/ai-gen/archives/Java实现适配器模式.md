<think>
嗯，我现在得学习一下适配器模式，然后用Java来实现它。适配器模式是什么呢？好像是用来解决两个接口不兼容的问题，让它们能够合作。比如说，有一个旧的类，它的方法和新的需求不匹配，这时候就需要一个适配器来转换接口。

首先，我得理解适配器模式的结构。它有两种形式，一种是类适配器，使用继承；另一种是对象适配器，使用组合。我应该分别了解这两种实现方式。

那我先从类适配器开始吧。假设旧接口叫OldInterface，新接口叫NewInterface。旧的类可能有一个methodOld()方法，而新的需求需要methodNew()。这时候，我需要创建一个适配器类，它继承OldClass，并且实现NewInterface。这样，当调用methodNew()的时候，适配器内部会调用旧的methodOld()。

比如说，举个例子。假设有鸟和蝙蝠两种动物，鸟有飞行的方法，而蝙蝠虽然也能飞，但接口不一样。这时候，我可以创建一个适配器，让蝙蝠能够像鸟一样被调用飞行的方法。

那代码结构大概是这样的：
- Bird接口有fly()方法。
- Bat类有ownWings()和fly()方法，但fly()可能名称不同，比如叫flyBat()。
- 适配器BataAdapter继承Bat，并且实现Bird接口。这样，当调用adapter.fly()时，内部调用的就是bat的flyBat()。

那具体怎么写呢？比如说：

public interface Bird {
    void fly();
}

public class Bat {
    public void ownWings() { ... }
    public void flyBat() { ... }
}

public class BatAdapter extends Bat implements Bird {
    @Override
    public void fly() {
        this.flyBat();
    }
}

这样，当使用适配器的时候，就可以把Bat对象包装成Bird来使用。

接下来是对象适配器，它用组合的方式。也就是说，适配器类里包含一个旧的对象，而不是继承它。这在旧类不能被继承的情况下更有用，比如旧类是final的。

结构上，适配器实现新接口，并且持有旧对象的引用。在适配器的方法中，调用旧对象的对应方法。这样可以灵活地处理多个旧对象的适配情况。

举个例子，比如说旧的系统有某种数据读取方式，而新的需求需要不同的接口。这时候，可以创建一个适配器，内部持有旧的数据源，然后在新接口的方法中调用旧的数据获取方式。

那代码结构可能像这样：

public interface OldDataSource {
    void oldRead();
}

public class OldData implements OldDataSource {
    @Override
    public void oldRead() { ... }
}

然后适配器：

public class DataSourceAdapter implements NewInterface {
    private OldDataSource dataSource;

    public DataSourceAdapter(OldDataSource source) {
        this.dataSource = source;
    }

    @Override
    public void newRead() {
        dataSource.oldRead();
    }
}

这样，当使用适配器时，就能将旧的数据源包装成新的接口来用。

那现在我得把这些知识整合起来，写一个具体的Java示例。比如说，考虑一个实践中的场景，比如不同的支付方式。我可以创建一个旧的支付接口和一个新的支付接口，然后通过适配器让旧的支付方式适应新接口。

例如：

旧的支付接口可能有payByCredit()方法，而新的统一接口需要executePayment()。这时候，创建一个适配器，让旧系统能够使用新的支付接口。

那么具体步骤是什么呢？

首先定义旧接口和旧实现类，然后新接口，接着是适配器类。

好的，那我来写个例子：

// 旧的支付接口
interface OldPayment {
    void payByCredit();
}

// 旧的支付实现类
class CreditPayment implements OldPayment {
    @Override
    public void payByCredit() {
        System.out.println("支付通过信用卡完成。");
    }
}

// 新的统一支付接口
interface NewPayment {
    void executePayment();
}

然后是适配器：

// 类适配器
class PaymentAdapter extends CreditPayment implements NewPayment {
    @Override
    public void executePayment() {
        payByCredit(); // 调用旧方法
    }
}

这样，当使用PaymentAdapter的时候，就可以像NewPayment一样调用executePayment()，而内部执行的是payByCredit()。

或者，如果使用对象适配器：

// 对象适配器
class PaymentAdapterObj implements NewPayment {
    private OldPayment oldPayment;

    public PaymentAdapterObj(OldPayment payment) {
        this.oldPayment = payment;
    }

    @Override
    public void executePayment() {
        oldPayment.payByCredit();
    }
}

然后使用的时候，可以传入任何旧的payment对象，比如：

OldPayment old = new CreditPayment();
NewPayment adapter = new PaymentAdapterObj(old);
adapter.executePayment();

这样就实现了适配器模式。

那我应该如何验证这个适配器是否正确工作呢？可以通过测试类来测试。当调用新接口的方法时，内部是否正确调用旧接口的方法。

比如：

public class Test {
    public static void main(String[] args) {
        // 使用类适配器
        NewPayment paymentAdapter = new PaymentAdapter();
        paymentAdapter.executePayment(); // 输出：支付通过信用卡完成。

        // 使用对象适配器
        OldPayment oldPayment = new CreditPayment();
        NewPayment objAdapter = new PaymentAdapterObj(oldPayment);
        objAdapter.executePayment(); // 同样输出：支付通过信用卡完成。
    }
}

这样测试后，应该能正确输出结果，证明适配器模式的实现是有效的。

那总结一下，适配器模式分为类和对象两种。类适配器使用继承，适用于可以修改旧类的情况；而对象适配器更灵活，可以处理多个旧类或无法被继承的旧类。

在实际开发中，应该根据具体情况选择合适的方式。当旧系统不能被修改或者不允许继承时，对象适配器是更好的选择。同时，如果需要扩展旧类的功能，也可以通过适配器来实现，而无需修改原有代码。

还有一个考虑点就是，这个模式的缺点是什么？可能会增加系统中类的数量，当过度使用时，反而让系统变得复杂。不过，在处理接口不兼容的问题时，它确实是一个有效的解决方案。
</think>

适配器模式是一种结构设计模式，它能使两个不兼容的对象协同工作。通过将一个类的接口转换为客户期望的另一个接口，适配器让原本由于接口不匹配而无法一起工作的类可以合作。

### Java实现适配器模式

以下是适配器模式在Java中的两种实现方式：类适配器和对象适配器。

#### 1. 类适配器（通过继承实现）

适用于需要扩展旧类并且旧类允许被继承的情况。

**代码示例：**

```java
// 旧接口
interface OldInterface {
    void oldMethod();
}

// 旧类
class OldClass implements OldInterface {
    @Override
    public void oldMethod() {
        System.out.println("调用旧的方法。");
    }
}

// 新接口
interface NewInterface {
    void newMethod();
}

// 类适配器：继承旧类并实现新接口
class Adapter extends OldClass implements NewInterface {
    @Override
    public void newMethod() {
        oldMethod(); // 调用旧类的方法
    }
}
```

**测试代码：**

```java
public class Test {
    public static void main(String[] args) {
        NewInterface adapter = new Adapter();
        adapter.newMethod(); // 输出：调用旧的方法。
    }
}
```

#### 2. 对象适配器（通过组合实现）

更灵活，允许在运行时改变被适配对象。

**代码示例：**

```java
// 旧接口
interface OldInterface {
    void oldMethod();
}

// 旧类
class OldClass implements OldInterface {
    @Override
    public void oldMethod() {
        System.out.println("调用旧的方法。");
    }
}

// 新接口
interface NewInterface {
    void newMethod();
}

// 对象适配器：持有旧对象并实现新接口
class Adapter implements NewInterface {
    private OldInterface oldObj;

    public Adapter(OldInterface oldObj) {
        this.oldObj = oldObj;
    }

    @Override
    public void newMethod() {
        oldObj.oldMethod(); // 调用旧方法
    }
}
```

**测试代码：**

```java
public class Test {
    public static void main(String[] args) {
        OldInterface old = new OldClass();
        NewInterface adapter = new Adapter(old);
        adapter.newMethod(); // 输出：调用旧的方法。
    }
}
```

### 总结

- **类适配器** 使用继承，适用于需要扩展旧类的情况，但对旧类有侵入性。
- **对象适配器** 使用组合，更灵活，适合运行时动态调整，被视为更好的实践。

选择哪种方式取决于具体需求和限制，如是否允许修改旧类或需要支持多个旧系统。