+++
title = "Java设计模式"
author = ["Cheng Xia"]
date = 2024-12-08
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">Table of Contents</div>

- [创建型模式](#创建型模式)
    - [单例模式（Singleton Pattern）](#单例模式-singleton-pattern)
    - [原型模式（Prototype Pattern）](#原型模式-prototype-pattern)
    - [工厂模式(工厂方法和抽象工厂)](#工厂模式--工厂方法和抽象工厂)
    - [建造者模式（Builder Pattern）](#建造者模式-builder-pattern)
- [结构型模式](#结构型模式)
    - [代理模式（Proxy Pattern）](#代理模式-proxy-pattern)
    - [适配器模式（Adapter Pattern）](#适配器模式-adapter-pattern)
    - [桥接模式（Bridge Pattern）](#桥接模式-bridge-pattern)
    - [装饰器模式（Decorator Pattern）](#装饰器模式-decorator-pattern)
    - [外观模式（Facade Pattern）](#外观模式-facade-pattern)
    - [享元模式（Flyweight Pattern）](#享元模式-flyweight-pattern)
    - [组合模式（Composite Pattern）](#组合模式-composite-pattern)
- [行为型模式](#行为型模式)
    - [模板方法模式（Template Method Pattern）](#模板方法模式-template-method-pattern)
    - [策略模式（Strategy Pattern）](#策略模式-strategy-pattern)
    - [命令模式（Command Pattern）](#命令模式-command-pattern)
    - [职责链模式（Chain of Responsibility Pattern）](#职责链模式-chain-of-responsibility-pattern)
    - [状态模式（State Pattern）](#状态模式-state-pattern)
    - [观察者模式（Observer Pattern）](#观察者模式-observer-pattern)
    - [中介者模式（Mediator Pattern）](#中介者模式-mediator-pattern)
    - [迭代器模式（Iterator Pattern）](#迭代器模式-iterator-pattern)
    - [访问者模式（Visitor Pattern）](#访问者模式-visitor-pattern)
    - [备忘录模式（Memento Pattern）](#备忘录模式-memento-pattern)
    - [解释器模式（Interpreter Pattern）](#解释器模式-interpreter-pattern)

</div>
<!--endtoc-->

说明： <br/>
本文内容通过gpt生成。 <br/>

设计模式分类： <br/>

-   创建型模式 <br/>
    用于描述“怎样创建对象”，它的主要特点是“将对象的创建与使用分离”。GoF（四人组）书中提供了单例、原型、工厂方法、抽象工厂、建造者等 5 种创建型模式。 <br/>
-   结构型模式 <br/>
    用于描述如何将类或对象按某种布局组成更大的结构，GoF（四人组）书中提供了代理、适配器、桥接、装饰、外观、享元、组合等 7 种结构型模式。 <br/>
-   行为型模式 <br/>
    用于描述类或对象之间怎样相互协作共同完成单个对象无法单独完成的任务，以及怎样分配职责。GoF（四人组）书中提供了模板方法、策略、命令、职责链、状态、观察者、中介者、迭代器、访问者、备忘录、解释器等 11 种行为型模式。 <br/>

原文链接：[GOF设计模式](https://blog.csdn.net/weixin_66400215/article/details/136081852) <br/>


## 创建型模式 {#创建型模式}


### 单例模式（Singleton Pattern） {#单例模式-singleton-pattern}

单例模式（Singleton Pattern）是一种创建型设计模式，其核心目的是确保一个类在整个应用程序中只有一个实例，并提供一个全局访问点。 <br/>

-   单例模式的核心思想 <br/>
    1.  唯一性：保证一个类只有一个实例存在。 <br/>
    2.  全局访问点：提供一个全局访问该实例的方法。 <br/>

-   单例模式的实现要点 <br/>
    实现单例模式需要解决以下几个关键问题： <br/>
    1.  唯一实例的创建：通过私有构造函数防止外部实例化。 <br/>
    2.  线程安全性：确保多线程环境下的单例对象唯一性。 <br/>
    3.  延迟初始化（可选）：只有在第一次使用时创建实例，节省资源。 <br/>

-   单例模式的常见实现方式 <br/>
    1.  饿汉式单例 <br/>
        特点：类加载时就初始化单例实例，线程安全。 <br/>
        ```java
        public class Singleton {
            // 提前初始化单例实例
            private static final Singleton INSTANCE = new Singleton();
        
            // 私有构造函数，防止外部实例化
            private Singleton() {}
        
            // 提供全局访问点
            public static Singleton getInstance() {
                return INSTANCE;
            }
        }
        ```
        优点：简单，容易实现。线程安全。 <br/>
        缺点：即使不使用实例，也会初始化，可能浪费资源。 <br/>
    2.  懒汉式单例（线程不安全） <br/>
        特点：实例在首次调用时创建，非线程安全。 <br/>
        ```java
        public class Singleton {
            private static Singleton instance;
        
            private Singleton() {}
        
            public static Singleton getInstance() {
                if (instance == null) {
                    instance = new Singleton(); // 多线程环境可能出现问题
                }
                return instance;
            }
        }
        ```
        优点：延迟初始化，只有在需要时才创建实例。 <br/>
        缺点：在多线程环境中可能会创建多个实例（线程不安全）。 <br/>
    3.  懒汉式单例（线程安全，使用同步方法） <br/>
        特点：通过同步方法解决多线程问题，效率较低。 <br/>
        ```java
        public class Singleton {
            private static Singleton instance;
        
            private Singleton() {}
        
            public static synchronized Singleton getInstance() {
                if (instance == null) {
                    instance = new Singleton();
                }
                return instance;
            }
        }
        ```
        优点：延迟初始化。线程安全。 <br/>
        缺点：同步方法会导致性能开销较大。 <br/>
    4.  双重检查锁（推荐） <br/>
        特点：通过双重检查锁机制，既实现了延迟初始化，又保证了线程安全和效率。 <br/>
        ```java
        public class Singleton {
            private static volatile Singleton instance; // 使用 volatile 确保可见性
        
            private Singleton() {}
        
            public static Singleton getInstance() {
                if (instance == null) { // 第一次检查
                    synchronized (Singleton.class) {
                        if (instance == null) { // 第二次检查
                            instance = new Singleton();
                        }
                    }
                }
                return instance;
            }
        }
        ```
        优点：延迟初始化。线程安全，效率高。 <br/>
        缺点：代码稍微复杂。 <br/>
    5.  静态内部类（推荐） <br/>
        特点：利用 Java 的类加载机制，既保证线程安全，又实现延迟加载。 <br/>
        ```java
        public class Singleton {
            private Singleton() {}
        
            // 静态内部类，只有在第一次调用 getInstance() 时才会加载
            private static class SingletonHolder {
                private static final Singleton INSTANCE = new Singleton();
            }
        
            public static Singleton getInstance() {
                return SingletonHolder.INSTANCE;
            }
        }
        ```
        优点：延迟初始化。线程安全。实现简单，性能高。 <br/>
    6.  枚举实现（最安全） <br/>
        特点：利用 Java 的枚举特性，防止反序列化和反射破坏单例。 <br/>
        ```java
        public enum Singleton {
            INSTANCE;
        
            public void doSomething() {
                System.out.println("Do something...");
            }
        }
        ```
        优点：简单易用。天然线程安全。防止反射和序列化破坏。 <br/>
        缺点：不支持延迟加载。 <br/>

-   单例模式的优缺点 <br/>
    优点： <br/>
    
    1.  控制实例数量：确保系统中只有一个实例存在，节省内存。 <br/>
    2.  全局访问：提供全局访问点，便于统一管理实例。 <br/>
    3.  灵活性：在需要共享资源或集中控制时非常有用。 <br/>
    
    缺点： <br/>
    
    1.  违背单一职责原则：单例类需要负责自己的创建和生命周期管理。 <br/>
    2.  不易测试：由于全局实例的存在，可能难以模拟不同的运行环境。 <br/>
    3.  资源回收问题：全局实例在程序生命周期中一直存在，可能导致资源浪费。 <br/>

-   单例模式的应用场景 <br/>
    1.  资源管理：线程池、数据库连接池。 <br/>
    2.  全局配置管理：日志管理器（如 LogManager）。 <br/>
    3.  设备或外部资源访问：打印机、文件系统、网络连接。 <br/>
    4.  缓存：系统中缓存数据的管理对象。 <br/>
    5.  其他场景：游戏中的全局游戏设置管理、操作系统中的窗口管理器。 <br/>

-   单例模式的注意事项 <br/>
    1.  防止反射攻击： <br/>
        通过私有构造函数和枚举可以避免反射破坏单例。 <br/>
        在构造函数中检查是否已实例化。 <br/>
    2.  防止序列化破坏： <br/>
        如果需要序列化单例类，重写 readResolve 方法返回单例实例： <br/>
        ```java
        private Object readResolve() {
            return getInstance();
        }
        ```
    3.  并发问题： <br/>
        在多线程环境中需要使用线程安全的实现，如双重检查锁或静态内部类。 <br/>

-   总结 <br/>
    单例模式是一种简单但实用的设计模式，广泛应用于需要全局唯一性的场景。 <br/>
    推荐使用双重检查锁和静态内部类的实现方式，它们既简单又高效。 <br/>
    在现代开发中，使用枚举单例是最安全和推荐的做法，尤其是在需要防止反序列化和反射攻击时。 <br/>


### 原型模式（Prototype Pattern） {#原型模式-prototype-pattern}

原型模式（Prototype Pattern）是一种创建型设计模式，它通过复制（克隆）一个已有的对象来创建新对象，而不是通过 new 关键字实例化对象。 <br/>
这种模式允许对象在运行时动态地创建新的对象，并可以显著提高对象创建的效率。 <br/>

-   原型模式的核心思想 <br/>
    用一个现有的对象作为原型，通过复制（克隆）来生成新的对象。这种方式可以避免复杂对象的多次创建，并提高性能。 <br/>

-   原型模式的结构 <br/>
    原型模式的结构包括以下角色： <br/>
    -   Prototype（抽象原型）：声明一个 clone() 方法，用于克隆对象。 <br/>
    -   ConcretePrototype（具体原型）：实现 clone() 方法，返回对象的副本。 <br/>
    -   Client（客户端）：使用 clone() 方法来复制已有的对象。 <br/>

-   Java 中的实现 <br/>
    Java 中的原型模式通常通过实现 Cloneable 接口和重写 Object 类的 clone() 方法来实现。 <br/>
    示例：克隆一份文档对象 <br/>
    步骤 1：定义抽象原型类 <br/>
    ```java
    // 抽象原型类
    abstract class Document implements Cloneable {
        private String title;
        private String content;
    
        public Document(String title, String content) {
            this.title = title;
            this.content = content;
        }
    
        public String getTitle() {
            return title;
        }
    
        public String getContent() {
            return content;
        }
    
        // 克隆方法
        @Override
        public Document clone() {
            try {
                return (Document) super.clone();  // 浅拷贝
            } catch (CloneNotSupportedException e) {
                e.printStackTrace();
                return null;
            }
        }
    
        // 显示文档信息
        public abstract void display();
    }
    ```
    步骤 2：实现具体原型类 <br/>
    ```java
    // 具体原型类
    class WordDocument extends Document {
        public WordDocument(String title, String content) {
            super(title, content);
        }
    
        @Override
        public void display() {
            System.out.println("Word Document: " + getTitle());
            System.out.println("Content: " + getContent());
        }
    }
    ```
    步骤 3：客户端使用克隆 <br/>
    ```java
    // 客户端代码
    public class PrototypePatternDemo {
        public static void main(String[] args) {
            // 创建一个文档对象
            WordDocument originalDocument = new WordDocument("Report", "This is the original document content.");
    
            // 克隆文档对象
            WordDocument clonedDocument = originalDocument.clone();
    
            // 修改克隆对象的内容
            System.out.println("Original Document:");
            originalDocument.display();  // 输出原始文档内容
    
            System.out.println("\nCloned Document:");
            clonedDocument.display();  // 输出克隆文档内容
        }
    }
    ```
    输出结果： <br/>
    ```text
    Original Document:
    Word Document: Report
    Content: This is the original document content.
    
    Cloned Document:
    Word Document: Report
    Content: This is the original document content.
    ```
-   深拷贝与浅拷贝 <br/>
    在 Java 的原型模式中，克隆可以分为两种方式： <br/>
    1.  浅拷贝（Shallow Copy） <br/>
        浅拷贝只复制对象的基本数据类型和对象的引用，不复制引用对象本身。修改引用对象时，原型对象与克隆对象共享同一个引用。 <br/>
        浅拷贝示例： <br/>
        ```java
        @Override
        public Document clone() {
            try {
                return (Document) super.clone();  // 仅复制对象本身，不复制内部的引用对象
            } catch (CloneNotSupportedException e) {
                e.printStackTrace();
                return null;
            }
        }
        ```
    2.  深拷贝（Deep Copy） <br/>
        深拷贝不仅复制对象本身，还会递归地复制对象内部的引用对象，使得克隆对象与原型对象完全独立。 <br/>
        深拷贝示例： <br/>
        ```java
        @Override
        public Document clone() {
            Document cloned = (Document) super.clone();  // 浅拷贝对象本身
            cloned.setContent(new String(this.getContent()));  // 深拷贝引用类型的字段
            return cloned;
        }
        ```
-   原型模式的优缺点 <br/>
    优点： <br/>
    
    -   提高对象创建效率：通过克隆现有对象，避免了使用 new 创建对象的复杂过程。 <br/>
    -   简化对象创建：适用于复杂对象的创建场景，尤其是创建成本较高的对象。 <br/>
    -   减少子类的数量：通过克隆对象，可以减少需要创建的子类数量。 <br/>
    
    缺点： <br/>
    
    -   克隆的复杂性：对象的深拷贝可能较为复杂，特别是当对象中包含大量嵌套引用时。 <br/>
    -   依赖于 Cloneable 接口：需要正确实现 Cloneable 接口和 clone() 方法，可能会引入错误。 <br/>

-   原型模式的应用场景 <br/>
    -   对象创建成本较高：当对象的创建过程复杂或需要消耗大量资源时，通过克隆可以提高效率。 <br/>
    -   需要大量相似对象：当需要创建一组相似对象时，可以通过克隆原型对象来避免重复创建。 <br/>
    -   需要动态生成对象：例如，需要在运行时动态生成某些配置或状态的对象。 <br/>

-   原型模式的现实生活类比 <br/>
    -   文档复制：从现有的 Word 文档模板中复制一个新的文档并进行修改。 <br/>
    -   对象复制：如游戏中的角色克隆或 UI 组件的克隆。 <br/>

-   原型模式与其他创建型模式的比较 <br/>
    
    | 模式  | 目的          | 实现方式    | 优点             |
    |-----|-------------|---------|----------------|
    | 原型模式 | 通过克隆现有对象创建新对象 | 克隆（浅拷贝或深拷贝） | 提高对象创建效率，减少子类数量。 |
    | 工厂模式 | 创建不同类型的对象 | 工厂方法或抽象工厂 | 隐藏对象的创建逻辑。 |
    | 单例模式 | 确保一个类只有一个实例 | 静态方法返回实例 | 全局唯一实例，节省资源。 |
    | 建造者模式 | 逐步构建复杂对象 | 链式调用或建造者类 | 逐步构建复杂对象，易读性高。 |

-   总结 <br/>
    -   原型模式是一种高效的对象创建方式，通过克隆现有对象来创建新对象。 <br/>
    -   适用于对象创建成本较高或需要大量相似对象的场景。 <br/>
    -   在实现原型模式时，需要正确处理浅拷贝和深拷贝，以确保对象的独立性。 <br/>


### 工厂模式(工厂方法和抽象工厂) {#工厂模式--工厂方法和抽象工厂}

工厂模式（Factory Pattern）是一种创建型设计模式，它通过定义一个工厂类来创建对象，而不是直接在代码中使用 new 关键字实例化对象。这样可以将对象创建的细节封装起来，提供灵活的对象生成方式。 <br/>

-   工厂模式的核心思想 <br/>
    将对象的创建与使用分离，让工厂类负责对象的实例化，从而使代码更加灵活、可扩展、易维护。 <br/>

-   工厂模式的分类 <br/>
    1.  简单工厂模式（Simple Factory Pattern） <br/>
        也称为静态工厂方法模式，由一个工厂类根据输入的条件来决定创建哪种具体类的对象。这是一种不属于 GoF（四人帮）设计模式的模式。 <br/>
        示例：创建不同类型的图形（如圆形、矩形）。 <br/>
        ```java
        // 接口或抽象类
        interface Shape {
            void draw();
        }
        
        // 具体类
        class Circle implements Shape {
            public void draw() {
                System.out.println("Drawing a Circle.");
            }
        }
        
        class Rectangle implements Shape {
            public void draw() {
                System.out.println("Drawing a Rectangle.");
            }
        }
        
        // 简单工厂类
        class ShapeFactory {
            public static Shape getShape(String shapeType) {
                if (shapeType == null) {
                    return null;
                }
                if (shapeType.equalsIgnoreCase("CIRCLE")) {
                    return new Circle();
                } else if (shapeType.equalsIgnoreCase("RECTANGLE")) {
                    return new Rectangle();
                }
                return null;
            }
        }
        
        // 使用工厂创建对象
        public class SimpleFactoryDemo {
            public static void main(String[] args) {
                Shape shape1 = ShapeFactory.getShape("CIRCLE");
                shape1.draw();  // 输出: Drawing a Circle.
        
                Shape shape2 = ShapeFactory.getShape("RECTANGLE");
                shape2.draw();  // 输出: Drawing a Rectangle.
            }
        }
        ```
    2.  工厂方法模式（Factory Method Pattern） <br/>
        定义一个创建对象的接口，但将实例化的工作推迟到子类中，由子类决定要实例化的具体类。这是 GoF 设计模式中的创建型模式。 <br/>
        结构： <br/>
        
        -   抽象产品（Product）：定义产品的接口。 <br/>
        -   具体产品（ConcreteProduct）：实现产品接口。 <br/>
        -   抽象工厂（Factory）：定义一个创建产品对象的接口。 <br/>
        -   具体工厂（ConcreteFactory）：实现创建具体产品对象的逻辑。 <br/>
        
        示例：创建不同类型的日志记录器（如文件日志、控制台日志）。 <br/>
        ```java
        // 抽象产品
        interface Logger {
            void log(String message);
        }
        
        // 具体产品
        class ConsoleLogger implements Logger {
            public void log(String message) {
                System.out.println("ConsoleLogger: " + message);
            }
        }
        
        class FileLogger implements Logger {
            public void log(String message) {
                System.out.println("FileLogger: " + message);
            }
        }
        
        // 抽象工厂
        interface LoggerFactory {
            Logger createLogger();
        }
        
        // 具体工厂
        class ConsoleLoggerFactory implements LoggerFactory {
            public Logger createLogger() {
                return new ConsoleLogger();
            }
        }
        
        class FileLoggerFactory implements LoggerFactory {
            public Logger createLogger() {
                return new FileLogger();
            }
        }
        
        // 使用工厂方法创建对象
        public class FactoryMethodDemo {
            public static void main(String[] args) {
                LoggerFactory factory = new ConsoleLoggerFactory(); // 可替换为 FileLoggerFactory
                Logger logger = factory.createLogger();
                logger.log("This is a message.");  // 输出: ConsoleLogger: This is a message.
            }
        }
        ```
    3.  抽象工厂模式（Abstract Factory Pattern） <br/>
        提供一个接口，用于创建一族相关或互相依赖的对象，而无需指定具体的类。不同于工厂方法模式，它生产的是一系列相关的对象，而不仅仅是一个对象。 <br/>
        结构： <br/>
        
        -   抽象产品（ProductA、ProductB）：定义产品的接口。 <br/>
        -   具体产品（ConcreteProductA、ConcreteProductB）：实现产品接口。 <br/>
        -   抽象工厂（AbstractFactory）：定义创建一族产品的接口。 <br/>
        -   具体工厂（ConcreteFactory）：实现创建具体产品的逻辑。 <br/>
        
        示例：创建不同风格的 UI 组件（如按钮和文本框）。 <br/>
        ```java
        // 抽象产品
        interface Button {
            void click();
        }
        
        interface TextField {
            void type(String text);
        }
        
        // 具体产品：Windows 风格
        class WindowsButton implements Button {
            public void click() {
                System.out.println("Windows Button clicked.");
            }
        }
        
        class WindowsTextField implements TextField {
            public void type(String text) {
                System.out.println("Windows TextField: " + text);
            }
        }
        
        // 具体产品：Mac 风格
        class MacButton implements Button {
            public void click() {
                System.out.println("Mac Button clicked.");
            }
        }
        
        class MacTextField implements TextField {
            public void type(String text) {
                System.out.println("Mac TextField: " + text);
            }
        }
        
        // 抽象工厂
        interface UIFactory {
            Button createButton();
            TextField createTextField();
        }
        
        // 具体工厂：Windows 工厂
        class WindowsFactory implements UIFactory {
            public Button createButton() {
                return new WindowsButton();
            }
            public TextField createTextField() {
                return new WindowsTextField();
            }
        }
        
        // 具体工厂：Mac 工厂
        class MacFactory implements UIFactory {
            public Button createButton() {
                return new MacButton();
            }
            public TextField createTextField() {
                return new MacTextField();
            }
        }
        
        // 使用抽象工厂创建对象
        public class AbstractFactoryDemo {
            public static void main(String[] args) {
                UIFactory factory = new WindowsFactory(); // 可替换为 MacFactory
                Button button = factory.createButton();
                TextField textField = factory.createTextField();
        
                button.click();  // 输出: Windows Button clicked.
                textField.type("Hello!");  // 输出: Windows TextField: Hello!
            }
        }
        ```

-   工厂模式的优缺点 <br/>
    优点： <br/>
    
    -   解耦对象的创建与使用：使用者无需了解对象创建的细节，只需通过工厂来获取对象。 <br/>
    -   提高代码的灵活性和可维护性：新增产品时只需增加具体产品类和对应的工厂类，不需要修改已有代码。 <br/>
    -   符合开闭原则（OCP）：对扩展开放，对修改关闭。 <br/>
    
    缺点： <br/>
    
    -   增加了系统的复杂度：引入工厂模式会增加类的数量和代码的复杂性。 <br/>
    -   适用性有限：如果产品种类不多或变化不大，使用工厂模式可能显得过于复杂。 <br/>

-   工厂模式的应用场景 <br/>
    -   当对象的创建过程较复杂，且存在多种类型时。 <br/>
    -   当对象的创建需要依赖于具体环境或配置时。 <br/>
    -   需要解耦对象的创建与使用代码时。 <br/>
    -   在开发框架或类库时，为了提供灵活的扩展能力。 <br/>

-   总结 <br/>
    
    | 模式   | 特点          | 适用场景              |
    |------|-------------|-------------------|
    | 简单工厂模式 | 一个工厂类创建多个产品 | 产品数量较少，变化较少的场景。 |
    | 工厂方法模式 | 每个产品由对应的工厂类创建 | 产品种类较多，可能扩展新的产品类型。 |
    | 抽象工厂模式 | 创建一族相关的产品 | 需要创建一组相关对象时（如 UI 组件）。 |


### 建造者模式（Builder Pattern） {#建造者模式-builder-pattern}

建造者模式（Builder Pattern）是一种创建型设计模式，用于分步骤构建复杂对象。通过将一个对象的构建过程与它的表示分离，可以让同样的构建过程创建不同的表示。 <br/>

-   建造者模式的核心思想 <br/>
    将复杂对象的构建过程与其表示分离，从而允许相同的构建过程创建不同的对象。逐步构建对象，将创建逻辑分成多个步骤。 <br/>

-   建造者模式的结构 <br/>
    建造者模式包括以下角色： <br/>
    -   Builder（抽象建造者）： <br/>
        定义创建产品的步骤接口，如 buildPartA()、buildPartB() 等。 <br/>
    -   ConcreteBuilder（具体建造者）： <br/>
        实现 Builder 接口，负责具体的构建逻辑。 <br/>
    -   Product（产品类）： <br/>
        表示最终构建的复杂对象。 <br/>
    -   Director（指挥者）： <br/>
        负责按照特定顺序调用建造者对象的构建步骤，完成产品的创建。 <br/>

-   建造者模式的示例：构建房屋 <br/>
    1.  产品类 <br/>
        ```java
        // 产品类，表示复杂对象
        public class House {
            private String foundation;
            private String structure;
            private String roof;
        
            public void setFoundation(String foundation) {
                this.foundation = foundation;
            }
        
            public void setStructure(String structure) {
                this.structure = structure;
            }
        
            public void setRoof(String roof) {
                this.roof = roof;
            }
        
            @Override
            public String toString() {
                return "House [foundation=" + foundation + ", structure=" + structure + ", roof=" + roof + "]";
            }
        }
        ```
    2.  抽象建造者 <br/>
        ```java
        // 抽象建造者，定义建造步骤
        public interface HouseBuilder {
            void buildFoundation();
            void buildStructure();
            void buildRoof();
            House getHouse();
        }
        ```
    3.  具体建造者 <br/>
        ```java
        // 具体建造者，实现建造步骤
        public class ConcreteHouseBuilder implements HouseBuilder {
            private House house;
        
            public ConcreteHouseBuilder() {
                this.house = new House();
            }
        
            @Override
            public void buildFoundation() {
                house.setFoundation("Concrete Foundation");
                System.out.println("Foundation is built.");
            }
        
            @Override
            public void buildStructure() {
                house.setStructure("Concrete Structure");
                System.out.println("Structure is built.");
            }
        
            @Override
            public void buildRoof() {
                house.setRoof("Concrete Roof");
                System.out.println("Roof is built.");
            }
        
            @Override
            public House getHouse() {
                return this.house;
            }
        }
        ```
    4.  指挥者 <br/>
        ```java
        // 指挥者，负责按步骤建造产品
        public class HouseDirector {
            private HouseBuilder houseBuilder;
        
            public HouseDirector(HouseBuilder houseBuilder) {
                this.houseBuilder = houseBuilder;
            }
        
            public House constructHouse() {
                houseBuilder.buildFoundation();
                houseBuilder.buildStructure();
                houseBuilder.buildRoof();
                return houseBuilder.getHouse();
            }
        }
        ```
    5.  客户端使用建造者模式 <br/>
        ```java
        public class BuilderPatternDemo {
            public static void main(String[] args) {
                // 创建具体建造者
                HouseBuilder builder = new ConcreteHouseBuilder();
        
                // 指挥者控制建造过程
                HouseDirector director = new HouseDirector(builder);
        
                // 构建房屋
                House house = director.constructHouse();
        
                // 输出建造的房屋
                System.out.println("Constructed House: " + house);
            }
        }
        ```
        输出结果： <br/>
        ```text
        Foundation is built.
        Structure is built.
        Roof is built.
        Constructed House: House [foundation=Concrete Foundation, structure=Concrete Structure, roof=Concrete Roof]
        ```

-   建造者模式的优缺点 <br/>
    优点： <br/>
    
    1.  封装性好：将复杂对象的构建逻辑封装在建造者中，客户端无需关心具体的构建细节。 <br/>
    2.  灵活性高：可以通过不同的建造者创建不同的对象表示。 <br/>
    3.  符合单一职责原则：指挥者和建造者的职责分离。 <br/>
    4.  易于扩展：新增产品类型或建造步骤时，对原有代码影响较小。 <br/>
    
    缺点： <br/>
    
    1.  实现较复杂：需要定义多个类（抽象建造者、具体建造者、指挥者等）。 <br/>
    2.  不适合简单对象：如果对象结构简单，使用建造者模式可能显得臃肿。 <br/>

-   建造者模式的应用场景 <br/>
    1.  构建复杂对象： <br/>
        需要多个步骤来构建对象，且构建步骤顺序可能不同。例如：构建车辆、房屋、计算机等复杂对象。 <br/>
    2.  创建对象的多种表示： <br/>
        对象的构建过程是相同的，但最终的表示可以不同。例如：HTML 文件生成器、JSON 数据生成器。 <br/>
    3.  与指挥者协作： <br/>
        在构建对象时需要将逻辑控制权交给指挥者。 <br/>

-   建造者模式的现实生活类比 <br/>
    套餐定制：快餐店点餐时，选择基础套餐（主餐、饮料、甜点），套餐中的内容可以定制。 <br/>
    房屋建造：按照步骤完成地基、框架、屋顶的建造。 <br/>

-   建造者模式与工厂模式的对比 <br/>
    
    | 对比点 | 建造者模式        | 工厂模式         |
    |-----|--------------|--------------|
    | 对象复杂性 | 适合构建复杂对象，需要多个步骤完成 | 适合构建结构简单的对象 |
    | 对象创建过程 | 关注对象的构建过程和细节 | 关注对象的创建逻辑，直接返回对象 |
    | 产品多样性 | 可以生成多个不同的对象表示 | 通常生成一类对象的实例 |
    | 扩展性 | 扩展建造步骤较灵活 | 增加新产品需要新工厂实现 |

-   总结 <br/>
    建造者模式适用于构建复杂对象，尤其是对象的构建步骤和表示需要分离的场景。 <br/>
    推荐在构建复杂配置类、复杂对象时使用，能够提高代码的可维护性和可扩展性。 <br/>


## 结构型模式 {#结构型模式}


### 代理模式（Proxy Pattern） {#代理模式-proxy-pattern}

代理模式（Proxy Pattern）是一种结构型设计模式，它为某个对象提供一个代理对象，用来控制对目标对象的访问。代理对象可以在访问目标对象时，添加额外的功能（如权限控制、延迟加载等）。 <br/>

-   代理模式的核心思想 <br/>
    控制访问：通过代理对象间接访问目标对象。 <br/>
    附加功能：代理对象可以在访问目标对象时执行额外的操作（如日志记录、性能监控、权限检查等）。 <br/>
    解耦与扩展：客户端与目标对象解耦，方便扩展功能。 <br/>

-   代理模式的结构 <br/>
    代理模式通常包括以下几个角色： <br/>
    -   Subject（抽象主题）： <br/>
        定义目标对象和代理对象的共同接口。 <br/>
    -   RealSubject（真实主题）： <br/>
        被代理的实际对象，包含具体的业务逻辑。 <br/>
    -   Proxy（代理）： <br/>
        代理对象，持有对真实主题的引用，负责控制访问。 <br/>

-   代理模式的分类 <br/>
    1.  静态代理 <br/>
        代理类在编译时已经确定。需要为每个目标对象手动创建代理类。 <br/>
    2.  动态代理 <br/>
        代理类在运行时动态生成。Java 提供两种动态代理方式：基于接口的动态代理（通过 java.lang.reflect.Proxy 实现）；基于类的动态代理（通过 cglib 实现）。 <br/>

-   代理模式的实现示例 <br/>
    1.  静态代理示例 <br/>
        接口（Subject）： <br/>
        ```java
        public interface Service {
            void request();
        }
        ```
        真实主题（RealSubject）： <br/>
        ```java
        public class RealService implements Service {
            @Override
            public void request() {
                System.out.println("Executing RealService request...");
            }
        }
        ```
        代理类（Proxy）： <br/>
        ```java
        public class ServiceProxy implements Service {
            private RealService realService;
        
            public ServiceProxy(RealService realService) {
                this.realService = realService;
            }
        
            @Override
            public void request() {
                System.out.println("Proxy: Pre-processing...");
                realService.request();
                System.out.println("Proxy: Post-processing...");
            }
        }
        ```
        客户端（Client）： <br/>
        ```java
        public class StaticProxyDemo {
            public static void main(String[] args) {
                RealService realService = new RealService();
                Service proxy = new ServiceProxy(realService);
                proxy.request();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Proxy: Pre-processing...
        Executing RealService request...
        Proxy: Post-processing...
        ```
    2.  动态代理示例（基于接口） <br/>
        真实主题（RealSubject）： <br/>
        ```java
        public class RealService implements Service {
            @Override
            public void request() {
                System.out.println("Executing RealService request...");
            }
        }
        ```
        动态代理类： <br/>
        ```java
        import java.lang.reflect.InvocationHandler;
        import java.lang.reflect.Method;
        import java.lang.reflect.Proxy;
        
        public class DynamicProxy implements InvocationHandler {
            private Object target;
        
            public DynamicProxy(Object target) {
                this.target = target;
            }
        
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("Proxy: Pre-processing...");
                Object result = method.invoke(target, args);
                System.out.println("Proxy: Post-processing...");
                return result;
            }
        
            public static <T> T createProxy(T target) {
                return (T) Proxy.newProxyInstance(
                        target.getClass().getClassLoader(),
                        target.getClass().getInterfaces(),
                        new DynamicProxy(target)
                );
            }
        }
        ```
        客户端（Client）： <br/>
        ```java
        public class DynamicProxyDemo {
            public static void main(String[] args) {
                RealService realService = new RealService();
                Service proxy = DynamicProxy.createProxy(realService);
                proxy.request();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Proxy: Pre-processing...
        Executing RealService request...
        Proxy: Post-processing...
        ```
    3.  动态代理示例（基于类，使用 CGLIB） <br/>
        如果目标类没有实现接口，可以使用 CGLIB 动态代理。 <br/>
        引入依赖（Maven）： <br/>
        ```xml
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.3.0</version>
        </dependency>
        ```
        真实主题（没有接口）： <br/>
        ```java
        public class RealService {
            public void request() {
                System.out.println("Executing RealService request...");
            }
        }
        ```
        动态代理类（CGLIB 实现）： <br/>
        ```java
        import net.sf.cglib.proxy.Enhancer;
        import net.sf.cglib.proxy.MethodInterceptor;
        import net.sf.cglib.proxy.MethodProxy;
        
        import java.lang.reflect.Method;
        
        public class CglibProxy implements MethodInterceptor {
            private Object target;
        
            public CglibProxy(Object target) {
                this.target = target;
            }
        
            @Override
            public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
                System.out.println("Proxy: Pre-processing...");
                Object result = proxy.invoke(target, args);
                System.out.println("Proxy: Post-processing...");
                return result;
            }
        
            public static <T> T createProxy(T target) {
                Enhancer enhancer = new Enhancer();
                enhancer.setSuperclass(target.getClass());
                enhancer.setCallback(new CglibProxy(target));
                return (T) enhancer.create();
            }
        }
        ```
        客户端（Client）： <br/>
        ```java
        public class CglibProxyDemo {
            public static void main(String[] args) {
                RealService realService = new RealService();
                RealService proxy = CglibProxy.createProxy(realService);
                proxy.request();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Proxy: Pre-processing...
        Executing RealService request...
        Proxy: Post-processing...
        ```

-   代理模式的优缺点 <br/>
    优点： <br/>
    增强功能：可以在目标对象前后添加额外逻辑（如日志、权限检查）。 <br/>
    解耦性：客户端无需直接与目标对象交互，提升代码的灵活性和可维护性。 <br/>
    透明性：客户端不需要感知到代理的存在。 <br/>
    缺点： <br/>
    性能开销：由于增加了代理层，可能会有一定的性能损耗。 <br/>
    实现复杂：特别是动态代理，涉及反射或第三方库，增加了开发难度。 <br/>

-   代理模式的应用场景 <br/>
    权限控制：在访问资源前，检查用户是否有访问权限。 <br/>
          远程代理：通过代理对象访问远程对象（如 RMI）。 <br/>
    缓存代理：缓存目标对象的调用结果，减少系统开销。 <br/>
          延迟加载：在需要时才初始化目标对象。 <br/>
          监控与日志：在方法调用时记录日志或监控性能。 <br/>

-   总结 <br/>
    代理模式是一种非常灵活的设计模式，在实际开发中应用广泛。根据场景需要，可以选择静态代理或动态代理。现代 Java 开发中，动态代理更加流行，尤其是在框架（如 Spring AOP）中被大量使用，是实现横切关注点功能的核心技术。 <br/>


### 适配器模式（Adapter Pattern） {#适配器模式-adapter-pattern}

适配器模式（Adapter Pattern）是一种结构型设计模式，它的主要作用是将一个类的接口转换为客户希望的另一个接口。 <br/>
配器模式使得原本由于接口不兼容而不能一起工作的类可以协同工作。 <br/>

-   适配器模式的核心思想 <br/>
    将已有的类或接口通过适配器封装，使其与客户端期望的接口兼容，从而实现客户端可以使用不同的类而无需修改代码。 <br/>

-   适配器模式的结构 <br/>
    适配器模式的 UML 类图如下： <br/>
    ```text
     Client
       |
       v
    Target(目标接口) <------- Adapter(适配器) ------- Adaptee(被适配者)
    ```
    Target（目标接口）：定义客户端所期望的接口。 <br/>
    Adaptee（被适配者）：现有的接口或类，它需要被适配以满足目标接口的需求。 <br/>
    Adapter（适配器）：将 Adaptee 转换为 Target 接口的实现，使 Adaptee 可以与客户端代码兼容。 <br/>

-   适配器模式的实现方式 <br/>
    适配器模式有两种主要实现方式：对象适配器（使用组合实现）和类适配器（使用继承实现）。 <br/>
    1.  对象适配器模式（推荐） <br/>
        通过组合的方式，适配器内部持有一个 Adaptee 的实例，将其适配为 Target。 <br/>
        示例：将 Adaptee 接口转换为 Target。场景：假设我们有一个老的音频播放器 VlcPlayer，但客户端需要使用新的 MediaPlayer 接口。 <br/>
        ```java
        // 目标接口 (Target)
        interface MediaPlayer {
            void play(String audioType, String fileName);
        }
        
        // 被适配者类 (Adaptee)
        class VlcPlayer {
            public void playVlc(String fileName) {
                System.out.println("Playing VLC file: " + fileName);
            }
        }
        
        // 适配器类 (Adapter)
        class MediaAdapter implements MediaPlayer {
            private VlcPlayer vlcPlayer;
        
            public MediaAdapter() {
                this.vlcPlayer = new VlcPlayer();  // 组合关系
            }
        
            @Override
            public void play(String audioType, String fileName) {
                if (audioType.equalsIgnoreCase("vlc")) {
                    vlcPlayer.playVlc(fileName);  // 调用 Adaptee 的方法
                } else {
                    System.out.println("Unsupported audio format: " + audioType);
                }
            }
        }
        
        // 客户端代码 (Client)
        public class AdapterPatternDemo {
            public static void main(String[] args) {
                MediaPlayer mediaPlayer = new MediaAdapter();
                mediaPlayer.play("vlc", "song.vlc");  // 输出: Playing VLC file: song.vlc
                mediaPlayer.play("mp3", "song.mp3");  // 输出: Unsupported audio format: mp3
            }
        }
        ```
        对象适配器的特点： <br/>
        
        -   使用组合来适配接口，适配器可以适配多个不同的类。 <br/>
        -   更灵活，因为可以适配多个不同的 Adaptee 实例。 <br/>
    2.  类适配器模式 <br/>
        通过继承的方式实现适配器，适配器既继承 Adaptee，又实现 Target 接口。 <br/>
        示例： <br/>
        ```java
        // 目标接口 (Target)
        interface MediaPlayer {
            void play(String audioType, String fileName);
        }
        
        // 被适配者类 (Adaptee)
        class VlcPlayer {
            public void playVlc(String fileName) {
                System.out.println("Playing VLC file: " + fileName);
            }
        }
        
        // 适配器类 (Adapter)，继承 Adaptee 并实现 Target 接口
        class MediaAdapter extends VlcPlayer implements MediaPlayer {
            @Override
            public void play(String audioType, String fileName) {
                if (audioType.equalsIgnoreCase("vlc")) {
                    super.playVlc(fileName);  // 直接调用父类的方法
                } else {
                    System.out.println("Unsupported audio format: " + audioType);
                }
            }
        }
        
        // 客户端代码 (Client)
        public class ClassAdapterDemo {
            public static void main(String[] args) {
                MediaPlayer mediaPlayer = new MediaAdapter();
                mediaPlayer.play("vlc", "song.vlc");  // 输出: Playing VLC file: song.vlc
                mediaPlayer.play("mp3", "song.mp3");  // 输出: Unsupported audio format: mp3
            }
        }
        ```
        类适配器的特点： <br/>
        
        -   使用继承来适配接口。 <br/>
        -   因为 Java 不支持多继承，类适配器模式在适配多个类时存在限制。 <br/>

-   适配器模式的优缺点 <br/>
    优点： <br/>
    
    -   提高代码的复用性：可以复用已有的类，避免修改原有代码。 <br/>
    -   提高系统的灵活性和扩展性：适配器使得原本不兼容的类可以一起工作。 <br/>
    -   符合开闭原则：不需要修改已有的类，只需新增适配器类。 <br/>
    
    缺点： <br/>
    
    -   增加代码的复杂性：引入适配器可能增加系统的复杂性，尤其是在多个适配器同时存在时。 <br/>
    -   类适配器的局限性：由于 Java 不支持多继承，类适配器模式无法适配多个类。 <br/>

-   适配器模式的应用场景 <br/>
    -   已有的类接口不符合新的需求，但又无法修改该类时。 <br/>
    -   希望复用一些现有的类，但类的接口与目标接口不兼容时。 <br/>
    -   在开发过程中，需要统一多个不同类的接口以适应新的系统时。 <br/>

-   适配器模式的现实生活类比 <br/>
    电源适配器：将不同国家的电源插头适配为本地插座的标准。 <br/>
    USB 转接头：将旧式 USB 接口设备适配为新的 USB-C 接口。 <br/>

-   适配器模式与其他模式的区别 <br/>
    
    | 模式  | 目的                        | 实现方式 |
    |-----|---------------------------|------|
    | 适配器模式 | 将一个接口转换为另一个接口，使不兼容的类可以一起工作。 | 组合或继承。 |
    | 装饰器模式 | 动态地为对象添加新的功能。  | 组合。 |
    | 代理模式 | 为对象提供一个代理，以控制对对象的访问。 | 组合。 |
    | 桥接模式 | 将抽象部分与实现部分分离，以便独立变化。 | 组合。 |

-   总结 <br/>
    -   适配器模式主要用于接口转换，使不兼容的类可以协同工作。 <br/>
    -   在实际开发中，适配器模式非常适合在重构和兼容性维护中使用，使代码更加灵活和可扩展。 <br/>


### 桥接模式（Bridge Pattern） {#桥接模式-bridge-pattern}

桥接模式是一种结构型设计模式，其核心思想是将抽象部分与其实现部分分离，使它们可以独立变化。通过使用桥接模式，可以在不修改抽象和实现代码的情况下扩展系统功能，从而实现更好的灵活性和可维护性。 <br/>

-   桥接模式的核心思想 <br/>
    分离抽象和实现：将抽象部分（接口或抽象类）和实现部分（具体实现类）独立出来，使它们之间通过组合而非继承来协作。 <br/>
    使用“桥”连接抽象和实现：抽象部分维护对实现部分的引用（桥），抽象与实现解耦后，二者可以独立扩展。 <br/>

-   桥接模式的结构 <br/>
    桥接模式由以下几个核心角色组成： <br/>
    
    -   Abstraction（抽象类） <br/>
    
    定义抽象接口，包含实现部分的引用。可以包含抽象方法，也可以实现一些通用行为。 <br/>
    
    -   Implementor（实现接口） <br/>
    
    定义实现部分的接口，通常是一些底层操作。 <br/>
    
    -   RefinedAbstraction（扩展抽象类） <br/>
    
    继承自抽象类，定义更具体的功能。 <br/>
    
    -   ConcreteImplementor（具体实现类） <br/>
    
    实现实现接口的具体逻辑。 <br/>

-   桥接模式的示例：绘图工具 <br/>
    假设我们有一个绘图工具，支持绘制不同形状（如圆形和矩形），并且这些形状可以用不同的颜色实现（如红色和蓝色）。通过桥接模式，可以将形状与颜色解耦，从而实现两者的独立扩展。 <br/>
    1.  实现接口 <br/>
        定义颜色的实现接口： <br/>
        ```java
        // 实现接口
        public interface Color {
            void applyColor();
        }
        ```
    2.  具体实现类 <br/>
        实现颜色的具体逻辑： <br/>
        ```java
        // 红色实现类
        public class RedColor implements Color {
            @Override
            public void applyColor() {
                System.out.println("Applying Red Color");
            }
        }
        
        // 蓝色实现类
        public class BlueColor implements Color {
            @Override
            public void applyColor() {
                System.out.println("Applying Blue Color");
            }
        }
        ```
    3.  抽象类 <br/>
        定义形状的抽象类，包含颜色的引用： <br/>
        ```java
        // 抽象类
        public abstract class Shape {
            protected Color color; // 组合颜色实现
        
            public Shape(Color color) {
                this.color = color;
            }
        
            public abstract void draw();
        }
        ```
    4.  扩展抽象类 <br/>
        具体的形状类继承自抽象类，并定义自己的绘制逻辑： <br/>
        ```java
        // 圆形类
        public class Circle extends Shape {
            public Circle(Color color) {
                super(color);
            }
        
            @Override
            public void draw() {
                System.out.print("Drawing Circle with ");
                color.applyColor();
            }
        }
        
        // 矩形类
        public class Rectangle extends Shape {
            public Rectangle(Color color) {
                super(color);
            }
        
            @Override
            public void draw() {
                System.out.print("Drawing Rectangle with ");
                color.applyColor();
            }
        }
        ```
    5.  客户端代码 <br/>
        通过组合不同的形状和颜色，创建具体的实例： <br/>
        ```java
        public class BridgePatternDemo {
            public static void main(String[] args) {
                // 创建红色圆形
                Shape redCircle = new Circle(new RedColor());
                redCircle.draw();
        
                // 创建蓝色矩形
                Shape blueRectangle = new Rectangle(new BlueColor());
                blueRectangle.draw();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Drawing Circle with Applying Red Color
        Drawing Rectangle with Applying Blue Color
        ```

-   桥接模式的优缺点 <br/>
    优点： <br/>
    解耦抽象与实现：抽象和实现独立扩展，互不影响。 <br/>
          更好的扩展性：新增抽象或实现类时无需修改已有代码。 <br/>
          遵循开闭原则：系统更灵活，易于维护。 <br/>
          提高代码可读性：通过清晰的层次关系组织代码逻辑。 <br/>
    缺点： <br/>
    增加复杂性：系统引入更多类和接口，可能导致代码结构复杂。 <br/>
          不适合小型系统：对简单功能的开发来说可能显得多余。 <br/>

-   桥接模式的应用场景 <br/>
    需要解耦抽象和实现的场景：比如形状与颜色、设备与操作系统。 <br/>
          多维度变化的系统：系统需要在多个维度扩展时，比如数据库操作（SQL vs NoSQL）与数据库驱动。 <br/>
          需要避免继承层次过多：继承导致类爆炸时，可以使用桥接模式简化结构。 <br/>

-   桥接模式与其他模式的对比 <br/>
    
    | 模式   | 特点                        | 适用场景           |
    |------|---------------------------|----------------|
    | 桥接模式 | 将抽象与实现分离，通过组合实现解耦 | 抽象和实现需要独立变化的场景 |
    | 装饰器模式 | 动态地为对象添加额外功能，不改变对象接口 | 需要增强对象功能但不希望修改对象本身 |
    | 策略模式 | 定义一组算法，将其封装起来，可以互换 | 需要动态选择行为算法的场景 |
    | 抽象工厂模式 | 提供一系列相关或相互依赖对象的创建接口，不指定具体实现 | 需要创建一系列相关对象的场景 |

-   总结 <br/>
    桥接模式是一种结构清晰且高效的设计模式，适合用于多维度扩展和解耦复杂系统的场景。在实际开发中，通过桥接模式，可以让代码更具灵活性和可维护性，同时避免继承层次的过度扩展。 <br/>


### 装饰器模式（Decorator Pattern） {#装饰器模式-decorator-pattern}

装饰器模式是一种结构型设计模式，用于动态地为对象添加额外的职责或行为，而无需修改原始类或使用子类扩展。 <br/>

-   装饰器模式的核心思想 <br/>
    通过将对象嵌套在一系列的装饰器对象中，每个装饰器负责为对象添加新的行为。装饰器和被装饰对象都实现相同的接口，因此可以替代被装饰对象。动态组合行为，避免了大量继承导致的类爆炸问题。 <br/>

-   装饰器模式的结构 <br/>
    装饰器模式包含以下角色： <br/>
    
    -   Component（抽象组件） <br/>
    
    定义对象的接口，可以被动态添加行为。 <br/>
    
    -   ConcreteComponent（具体组件） <br/>
        实现 Component 接口，表示被装饰的核心对象。 <br/>
        
        -   Decorator（抽象装饰器） <br/>
        
        实现 Component 接口，并持有一个 Component 的引用。通过组合的方式，将行为传递给被装饰对象。 <br/>
        
        -   ConcreteDecorator（具体装饰器） <br/>
        
        继承 Decorator，实现具体的增强行为。 <br/>

-   装饰器模式的示例：咖啡订单系统 <br/>
    场景：实现一个咖啡订单系统，可以为基础咖啡动态添加附加项，如牛奶、糖浆等。 <br/>
    1.  抽象组件 <br/>
        ```java
        // 抽象组件，定义咖啡接口
        public interface Coffee {
            String getDescription();
            double getCost();
        }
        ```
    2.  具体组件 <br/>
        ```java
        // 具体组件，基本咖啡实现
        public class BasicCoffee implements Coffee {
            @Override
            public String getDescription() {
                return "Basic Coffee";
            }
        
            @Override
            public double getCost() {
                return 5.0;
            }
        }
        ```
    3.  抽象装饰器 <br/>
        ```java
        // 抽象装饰器，实现 Coffee 接口
        public abstract class CoffeeDecorator implements Coffee {
            protected Coffee coffee;
        
            public CoffeeDecorator(Coffee coffee) {
                this.coffee = coffee;
            }
        
            @Override
            public String getDescription() {
                return coffee.getDescription();
            }
        
            @Override
            public double getCost() {
                return coffee.getCost();
            }
        }
        ```
    4.  具体装饰器 <br/>
        ```java
        // 牛奶装饰器
        public class MilkDecorator extends CoffeeDecorator {
            public MilkDecorator(Coffee coffee) {
                super(coffee);
            }
        
            @Override
            public String getDescription() {
                return coffee.getDescription() + ", Milk";
            }
        
            @Override
            public double getCost() {
                return coffee.getCost() + 1.5;
            }
        }
        
        // 糖装饰器
        public class SugarDecorator extends CoffeeDecorator {
            public SugarDecorator(Coffee coffee) {
                super(coffee);
            }
        
            @Override
            public String getDescription() {
                return coffee.getDescription() + ", Sugar";
            }
        
            @Override
            public double getCost() {
                return coffee.getCost() + 0.5;
            }
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class DecoratorPatternDemo {
            public static void main(String[] args) {
                // 创建基础咖啡
                Coffee coffee = new BasicCoffee();
                System.out.println(coffee.getDescription() + " Cost: $" + coffee.getCost());
        
                // 添加牛奶
                coffee = new MilkDecorator(coffee);
                System.out.println(coffee.getDescription() + " Cost: $" + coffee.getCost());
        
                // 添加糖
                coffee = new SugarDecorator(coffee);
                System.out.println(coffee.getDescription() + " Cost: $" + coffee.getCost());
            }
        }
        ```
        输出结果： <br/>
        ```text
        Basic Coffee Cost: $5.0
        Basic Coffee, Milk Cost: $6.5
        Basic Coffee, Milk, Sugar Cost: $7.0
        ```

-   装饰器模式的优缺点 <br/>
    优点： <br/>
    灵活性高：可以动态添加或移除功能，而无需修改原始类。 <br/>
          遵循开闭原则：对扩展开放，对修改封闭。 <br/>
          可组合性强：多个装饰器可以任意组合，形成新的行为。 <br/>
          简化继承结构：避免通过继承来扩展功能，减少类的数量。 <br/>
    缺点： <br/>
    增加复杂性：过多的装饰器可能导致系统变得难以理解和维护。 <br/>
          调试困难：多层装饰器嵌套可能使问题定位变得复杂。 <br/>

-   装饰器模式的应用场景 <br/>
    动态扩展对象的功能：需要在运行时为对象添加功能，而非通过继承。 <br/>
          功能可组合：需要多个功能模块可以自由组合，比如 GUI 组件中的边框、滚动条等。 <br/>
          避免类爆炸：如果通过继承来扩展功能会导致类数目过多。 <br/>

-   装饰器模式的现实生活类比 <br/>
    服装搭配：基础服装可以穿外套、围巾等，不同的搭配形成新的风格。 <br/>
    文本编辑器：可以动态添加拼写检查、语法高亮等功能。 <br/>
          饮品加料：一杯基本饮品（如奶茶）可以按需添加珍珠、椰果、布丁等配料。 <br/>

-   装饰器模式与其他模式的对比 <br/>
    
    | 模式  | 特点                | 适用场景        |
    |-----|-------------------|-------------|
    | 装饰器模式 | 动态添加行为，扩展对象功能 | 功能可组合的场景 |
    | 代理模式 | 控制对象访问，通过代理类实现间接访问 | 需要对对象访问进行控制或增强时 |
    | 桥接模式 | 分离抽象与实现，通过组合实现解耦 | 抽象与实现独立扩展时 |
    | 适配器模式 | 将一个接口转换为客户端期望的另一个接口 | 现有类的接口与需求不匹配时 |

-   总结 <br/>
    装饰器模式是一种灵活、强大且高效的设计模式，特别适合在功能需求动态变化的场景下使用。通过动态组合行为，装饰器模式在保证代码扩展性和可维护性方面发挥了重要作用。 <br/>


### 外观模式（Facade Pattern） {#外观模式-facade-pattern}

外观模式是一种结构型设计模式，用于为子系统中的一组复杂接口提供一个统一的、高层的接口，从而让客户端能够更容易地与子系统交互。 <br/>

-   外观模式的核心思想 <br/>
    将一个复杂的系统划分为多个子系统。提供一个外观类（Facade），简化子系统的使用方式。客户端通过外观类访问子系统，隐藏子系统的实现细节。 <br/>

-   外观模式的结构 <br/>
    外观模式的 UML 结构包含以下角色： <br/>
    
    -   Facade（外观类） <br/>
    
    提供简化的接口，调用子系统的功能。 <br/>
    
    -   Subsystem（子系统） <br/>
    
    实现系统的具体功能，多个子系统可以独立工作。子系统不知道外观类的存在。 <br/>
    
    -   Client（客户端）： <br/>
    
    通过外观类访问子系统，而无需直接调用子系统的接口。 <br/>

-   外观模式的实现示例：家庭影院系统 <br/>
    场景：设计一个家庭影院系统，它包含多个组件：电视、音响、DVD 播放器。用户希望通过一个简单的接口来控制整个系统。 <br/>
    1.  子系统 <br/>
        定义家庭影院系统的各个子系统： <br/>
        ```java
        // 电视
        public class Television {
            public void turnOn() {
                System.out.println("Television is ON");
            }
        
            public void turnOff() {
                System.out.println("Television is OFF");
            }
        }
        
        // 音响
        public class SoundSystem {
            public void turnOn() {
                System.out.println("Sound System is ON");
            }
        
            public void turnOff() {
                System.out.println("Sound System is OFF");
            }
        
            public void setVolume(int level) {
                System.out.println("Sound System volume set to " + level);
            }
        }
        
        // DVD 播放器
        public class DVDPlayer {
            public void turnOn() {
                System.out.println("DVD Player is ON");
            }
        
            public void turnOff() {
                System.out.println("DVD Player is OFF");
            }
        
            public void play() {
                System.out.println("DVD is playing");
            }
        }
        ```
    2.  外观类 <br/>
        创建一个外观类，提供简化的接口： <br/>
        ```java
        // 外观类
        public class HomeTheaterFacade {
            private Television television;
            private SoundSystem soundSystem;
            private DVDPlayer dvdPlayer;
        
            public HomeTheaterFacade(Television television, SoundSystem soundSystem, DVDPlayer dvdPlayer) {
                this.television = television;
                this.soundSystem = soundSystem;
                this.dvdPlayer = dvdPlayer;
            }
        
            public void watchMovie() {
                System.out.println("Preparing to watch a movie...");
                television.turnOn();
                soundSystem.turnOn();
                soundSystem.setVolume(5);
                dvdPlayer.turnOn();
                dvdPlayer.play();
            }
        
            public void endMovie() {
                System.out.println("Shutting down the home theater...");
                dvdPlayer.turnOff();
                soundSystem.turnOff();
                television.turnOff();
            }
        }
        ```
    3.  客户端代码 <br/>
        客户端通过外观类与子系统交互： <br/>
        ```java
        public class FacadePatternDemo {
            public static void main(String[] args) {
                // 创建子系统组件
                Television television = new Television();
                SoundSystem soundSystem = new SoundSystem();
                DVDPlayer dvdPlayer = new DVDPlayer();
        
                // 创建外观类
                HomeTheaterFacade homeTheater = new HomeTheaterFacade(television, soundSystem, dvdPlayer);
        
                // 使用外观类
                homeTheater.watchMovie();
                System.out.println();
                homeTheater.endMovie();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Preparing to watch a movie...
        Television is ON
        Sound System is ON
        Sound System volume set to 5
        DVD Player is ON
        DVD is playing
        
        Shutting down the home theater...
        DVD Player is OFF
        Sound System is OFF
        Television is OFF
        ```

-   外观模式的优缺点 <br/>
    优点： <br/>
    简化接口使用：客户端通过统一接口与复杂系统交互，减少了学习成本。 <br/>
          降低客户端与子系统的耦合性：客户端与子系统之间的直接依赖被外观类隔离。 <br/>
          更好的分层设计：外观类充当接口层，子系统专注于具体实现，结构清晰。 <br/>
    缺点： <br/>
    可能掩盖系统的复杂性：如果外观类过于简化，可能导致客户端无法使用系统的高级功能。 <br/>
          可能引入过多的依赖：外观类与多个子系统耦合，可能需要频繁更新。 <br/>

-   外观模式的应用场景 <br/>
    需要为复杂子系统提供简化接口时：如数据库连接池的管理、日志系统的封装。 <br/>
          多个子系统需要协调工作时：如企业级应用中的模块通信。 <br/>
          构建分层架构时：外观类充当接口层，屏蔽底层实现细节。 <br/>

-   外观模式与其他模式的对比 <br/>
    
    | 模式  | 特点               | 适用场景           |
    |-----|------------------|----------------|
    | 外观模式 | 提供统一接口，简化复杂系统的使用 | 子系统复杂，需要提供高层接口的场景 |
    | 代理模式 | 控制对象访问，通过代理类实现间接访问 | 需要控制对象访问或提供额外功能时 |
    | 装饰器模式 | 动态为对象添加功能 | 对象功能需要扩展且不影响原始类的场景 |
    | 桥接模式 | 分离抽象与实现，通过组合实现解耦 | 抽象和实现需要独立扩展的场景 |

-   总结 <br/>
    外观模式是一种非常实用的模式，适合用于简化系统使用、降低耦合性和提升代码的可维护性。在实际开发中，它常被用于设计大型系统中的接口层。通过外观模式，开发者可以专注于核心功能实现，而客户端只需面对简单的接口即可完成复杂操作。 <br/>


### 享元模式（Flyweight Pattern） {#享元模式-flyweight-pattern}

享元模式是一种结构型设计模式，用于通过共享对象来减少内存使用量和对象创建开销，从而提高系统性能。它适合于需要大量创建相似对象的场景。 <br/>

-   享元模式的核心思想 <br/>
    1.  共享重复数据： <br/>
        将对象分为内蕴状态（不变的共享部分）和外蕴状态（变化的非共享部分）。内蕴状态存储在享元对象中，供多个上下文共享。外蕴状态由客户端负责维护。 <br/>
        
        1.  对象池： <br/>
        
        使用工厂类（享元工厂）创建和管理共享对象。当请求对象时，返回已有的共享对象（如果存在），否则创建新对象。 <br/>

-   享元模式的结构 <br/>
    享元模式由以下角色组成： <br/>
    
    -   Flyweight（抽象享元角色） <br/>
    
    定义共享对象的接口，供外部访问。 <br/>
    
    -   ConcreteFlyweight（具体享元角色） <br/>
    
    实现共享对象的具体内容。 <br/>
    
    -   UnsharedFlyweight（非共享享元角色） <br/>
    
    不需要共享的对象，一般结合享元对象使用。 <br/>
    
    -   FlyweightFactory（享元工厂角色） <br/>
    
    创建和管理享元对象，负责实现享元对象的共享。 <br/>
    
    -   Client（客户端） <br/>
    
    维护外蕴状态，并通过享元工厂获取共享对象。 <br/>

-   享元模式的实现示例：文本编辑器中的字符管理 <br/>
    场景：设计一个文本编辑器，它需要管理大量的字符，每个字符可以有不同的字体、大小和颜色。共享相同的字符数据可以有效减少内存开销。 <br/>
    1.  抽象享元角色 <br/>
        ```java
        // 抽象享元角色
        public interface Flyweight {
            void display(Character extrinsicState); // 接收外蕴状态
        }
        ```
    2.  具体享元角色 <br/>
        ```java
        // 具体享元角色
        public class ConcreteFlyweight implements Flyweight {
            private final String intrinsicState; // 内蕴状态
        
            public ConcreteFlyweight(String intrinsicState) {
                this.intrinsicState = intrinsicState;
            }
        
            @Override
            public void display(Character extrinsicState) {
                System.out.println("Intrinsic State: " + intrinsicState + ", Extrinsic State: " + extrinsicState);
            }
        }
        ```
    3.  享元工厂角色 <br/>
        ```java
        import java.util.HashMap;
        import java.util.Map;
        
        // 享元工厂
        public class FlyweightFactory {
            private final Map<String, Flyweight> flyweightPool = new HashMap<>();
        
            public Flyweight getFlyweight(String key) {
                if (!flyweightPool.containsKey(key)) {
                    flyweightPool.put(key, new ConcreteFlyweight(key));
                    System.out.println("Creating new Flyweight for key: " + key);
                }
                return flyweightPool.get(key);
            }
        
            public int getFlyweightCount() {
                return flyweightPool.size();
            }
        }
        ```
    4.  客户端代码 <br/>
        ```java
        public class FlyweightPatternDemo {
            public static void main(String[] args) {
                FlyweightFactory factory = new FlyweightFactory();
        
                Flyweight flyweightA = factory.getFlyweight("A");
                flyweightA.display('1');
        
                Flyweight flyweightB = factory.getFlyweight("B");
                flyweightB.display('2');
        
                Flyweight flyweightA2 = factory.getFlyweight("A");
                flyweightA2.display('3');
        
                System.out.println("Total Flyweight Objects: " + factory.getFlyweightCount());
            }
        }
        ```
        输出结果： <br/>
        ```text
        Creating new Flyweight for key: A
        Intrinsic State: A, Extrinsic State: 1
        Creating new Flyweight for key: B
        Intrinsic State: B, Extrinsic State: 2
        Intrinsic State: A, Extrinsic State: 3
        Total Flyweight Objects: 2
        ```

-   享元模式的优缺点 <br/>
    优点： <br/>
    减少内存占用：通过共享对象，避免创建大量重复对象。 <br/>
          性能优化：降低对象创建的开销，提升系统运行效率。 <br/>
          解耦状态：分离内蕴状态和外蕴状态，提升系统的灵活性。 <br/>
    缺点： <br/>
    增加复杂性：引入了享元工厂和状态分离，代码复杂度增加。 <br/>
          非共享对象的管理：需要客户端维护外蕴状态，可能导致使用不当。 <br/>

-   享元模式的应用场景 <br/>
    需要大量相似对象：如游戏中的粒子系统、字符处理等。 <br/>
          对象的状态可以分离：当对象的部分状态可以共享时，如图形绘制中的图元。 <br/>
          内存限制场景：系统对内存使用敏感，需优化对象存储。 <br/>

-   享元模式的现实生活类比 <br/>
    停车位管理：一个停车场的每个车位可以被视为享元对象，共享车位的编号等属性，外部维护当前停的车辆信息。 <br/>
          字符字体渲染：文本编辑器将相同的字体数据作为共享对象，不为每个字符创建独立的字体样式。 <br/>

-   享元模式与其他模式的对比 <br/>
    
    | 模式 | 特点          | 适用场景                  |
    |----|-------------|-----------------------|
    | 享元模式 | 通过共享对象减少内存使用 | 对象数量庞大且部分状态可共享的场景 |
    | 单例模式 | 确保一个类只有一个实例 | 全局唯一性需求的对象      |
    | 原型模式 | 通过克隆现有对象创建新对象 | 创建对象成本高或复杂时    |
    | 工厂模式 | 通过工厂方法创建对象 | 对象的创建逻辑复杂，或者需要灵活扩展实例类型的场景 |

-   总结 <br/>
    享元模式是一种专注于优化内存使用的设计模式，尤其适用于需要大量相似对象的场景。通过将状态分离并共享不变部分，享元模式能够显著降低系统开销。然而，在实现时需要权衡系统复杂性和性能收益，适当管理外蕴状态。 <br/>


### 组合模式（Composite Pattern） {#组合模式-composite-pattern}

组合模式是一种结构型设计模式，用于将对象组合成树形结构，以表示“部分-整体”的层次结构。它让客户端可以以一致的方式处理单个对象和对象的组合。 <br/>

-   组合模式的核心思想 <br/>
    统一接口：定义一个接口（或抽象类），用于表示叶子节点和组合对象。 <br/>
    递归组合：组合对象（容器）可以包含叶子节点或其他组合对象，实现递归结构。 <br/>
          透明性：客户端无需关心是单个对象还是组合对象，只需通过统一接口操作。 <br/>

-   组合模式的结构 <br/>
    组合模式的 UML 结构包含以下角色： <br/>
    
    -   Component（组件） <br/>
    
    定义组合对象和叶子对象的共同接口，可以是抽象类或接口。 <br/>
    
    -   Leaf（叶子节点） <br/>
    
    表示树的叶子节点，没有子节点。 <br/>
    
    -   Composite（组合节点） <br/>
    
    表示树的非叶子节点，可以包含其他叶子节点或组合节点。 <br/>
    
    -   Client（客户端） <br/>
    
    通过组件接口与树结构进行交互。 <br/>

-   组合模式的实现示例：文件系统 <br/>
    场景：设计一个文件系统，可以包含文件和文件夹。文件是叶子节点，文件夹是组合节点，客户端希望能够统一操作文件和文件夹。 <br/>
    1.  组件接口 <br/>
        ```java
        // 抽象组件
        public abstract class FileSystemComponent {
            protected String name;
        
            public FileSystemComponent(String name) {
                this.name = name;
            }
        
            public abstract void display(); // 显示组件信息
        
            // 默认实现的可选方法（供组合节点重写）
            public void add(FileSystemComponent component) {
                throw new UnsupportedOperationException("Add operation is not supported");
            }
        
            public void remove(FileSystemComponent component) {
                throw new UnsupportedOperationException("Remove operation is not supported");
            }
        }
        ```
    2.  叶子节点 <br/>
        ```java
        // 叶子节点：文件
        public class File extends FileSystemComponent {
        
            public File(String name) {
                super(name);
            }
        
            @Override
            public void display() {
                System.out.println("File: " + name);
            }
        }
        ```
    3.  组合节点 <br/>
        ```java
        import java.util.ArrayList;
        import java.util.List;
        
        // 组合节点：文件夹
        public class Folder extends FileSystemComponent {
            private List<FileSystemComponent> components = new ArrayList<>();
        
            public Folder(String name) {
                super(name);
            }
        
            @Override
            public void add(FileSystemComponent component) {
                components.add(component);
            }
        
            @Override
            public void remove(FileSystemComponent component) {
                components.remove(component);
            }
        
            @Override
            public void display() {
                System.out.println("Folder: " + name);
                for (FileSystemComponent component : components) {
                    component.display(); // 递归调用子节点的 display 方法
                }
            }
        }
        ```
    4.  客户端代码 <br/>
        ```java
        public class CompositePatternDemo {
            public static void main(String[] args) {
                // 创建文件
                File file1 = new File("File1.txt");
                File file2 = new File("File2.txt");
                File file3 = new File("File3.txt");
        
                // 创建文件夹
                Folder folder1 = new Folder("Folder1");
                Folder folder2 = new Folder("Folder2");
        
                // 组装树形结构
                folder1.add(file1);
                folder1.add(file2);
        
                folder2.add(file3);
                folder2.add(folder1);
        
                // 显示文件系统结构
                folder2.display();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Folder: Folder2
        File: File3.txt
        Folder: Folder1
        File: File1.txt
        File: File2.txt
        ```
-   组合模式的优缺点 <br/>
    优点： <br/>
    清晰的层次结构：使用树形结构表示“部分-整体”关系。 <br/>
          统一接口：客户端以一致方式操作单个对象和组合对象，增强透明性。 <br/>
          可扩展性强：添加新的叶子节点或组合节点不需要修改现有代码，符合开闭原则。 <br/>
    缺点： <br/>
    设计复杂性增加：如果系统中的对象关系较简单，使用组合模式可能导致过度设计。 <br/>
          对安全性要求较高：客户端需要确保正确使用 add 和 remove 等方法，否则可能出现运行时异常。 <br/>

-   组合模式的应用场景 <br/>
    树形结构的场景：文件系统、菜单管理、组织结构等。 <br/>
          希望统一操作单个对象和组合对象：例如绘图系统中的几何图形。 <br/>
          需要递归处理的对象结构：例如处理 XML 或 JSON 树。 <br/>

-   现实生活类比 <br/>
    公司组织架构：一个公司包含部门（组合节点）和员工（叶子节点），可以递归表示整个组织。 <br/>
    菜单管理：一个菜单系统包含子菜单（组合节点）和菜单项（叶子节点）。 <br/>

-   组合模式与其他模式的对比 <br/>
    
    | 模式  | 特点           | 适用场景       |
    |-----|--------------|------------|
    | 组合模式 | 统一处理单个对象和组合对象 | 树形结构，部分-整体关系 |
    | 装饰器模式 | 动态扩展对象功能 | 需要动态添加功能的场景 |
    | 桥接模式 | 分离抽象和实现，通过组合解耦 | 抽象和实现需要独立扩展的场景 |
    | 责任链模式 | 对象间以链式结构处理请求 | 请求需要在多个对象中传递时 |

-   总结 <br/>
    组合模式是处理复杂层次结构问题的强大工具，它将树形结构和递归处理的优势结合起来。通过统一接口，客户端可以透明地操作单个对象或组合对象，简化了复杂系统的使用和扩展。在需要表达“部分-整体”关系的系统中，组合模式是一种常见的设计模式。 <br/>


## 行为型模式 {#行为型模式}


### 模板方法模式（Template Method Pattern） {#模板方法模式-template-method-pattern}

模板方法模式是一种行为型设计模式，通过定义一个算法的骨架，将算法的某些步骤延迟到子类中实现，从而使得子类可以在不改变算法结构的情况下重新定义某些特定步骤。 <br/>

-   模板方法模式的核心思想 <br/>
    算法框架固定：定义一个模板方法，提供算法的固定步骤。 <br/>
          部分步骤可变：将某些具体的步骤通过抽象方法延迟到子类中实现。 <br/>
          代码复用：父类中实现通用的逻辑，子类专注于实现个性化的部分。 <br/>

-   模板方法模式的结构 <br/>
    核心角色： <br/>
    -   AbstractClass（抽象类） <br/>
        定义算法的骨架，包含一个模板方法和若干抽象方法。模板方法定义算法的固定流程，调用抽象方法或具体方法。 <br/>
        
        -   ConcreteClass（具体子类） <br/>
        
        实现抽象方法，定义特定步骤的具体行为。 <br/>
        
        -   Client（客户端） <br/>
        
        调用模板方法执行算法。 <br/>

-   模板方法模式的实现示例：制作饮料 <br/>
    场景：制作饮料的过程可以分为以下固定步骤： <br/>
    
    -   烧水。 <br/>
    -   放置主料（如茶叶或咖啡）。 <br/>
    -   冲泡主料。 <br/>
    -   加配料（如糖或牛奶）。 <br/>
    
    烧水和冲泡主料的逻辑是固定的，而放置主料和加配料的方式由具体饮料决定。 <br/>
    
    1.  抽象类 <br/>
        ```java
        // 抽象类：饮料制作模板
        public abstract class Beverage {
            // 模板方法，定义制作流程
            public final void prepare() {
                boilWater();
                addMainIngredient();
                brew();
                addCondiments();
                System.out.println("Beverage is ready!\n");
            }
        
            // 具体方法：烧水
            private void boilWater() {
                System.out.println("Boiling water...");
            }
        
            // 抽象方法：添加主料
            protected abstract void addMainIngredient();
        
            // 具体方法：冲泡主料
            private void brew() {
                System.out.println("Brewing...");
            }
        
            // 抽象方法：添加配料
            protected abstract void addCondiments();
        }
        ```
    2.  具体子类 <br/>
        制作咖啡 <br/>
        ```java
        // 具体子类：咖啡
        public class Coffee extends Beverage {
            @Override
            protected void addMainIngredient() {
                System.out.println("Adding coffee powder...");
            }
        
            @Override
            protected void addCondiments() {
                System.out.println("Adding sugar and milk...");
            }
        }
        ```
        制作茶 <br/>
        ```java
        // 具体子类：茶
        public class Tea extends Beverage {
            @Override
            protected void addMainIngredient() {
                System.out.println("Adding tea leaves...");
            }
        
            @Override
            protected void addCondiments() {
                System.out.println("Adding lemon...");
            }
        }
        ```
    3.  客户端代码 <br/>
        ```java
        public class TemplateMethodDemo {
            public static void main(String[] args) {
                // 制作咖啡
                Beverage coffee = new Coffee();
                coffee.prepare();
        
                // 制作茶
                Beverage tea = new Tea();
                tea.prepare();
            }
        }
        ```
        输出结果： <br/>
        ```text
        Boiling water...
        Adding coffee powder...
        Brewing...
        Adding sugar and milk...
        Beverage is ready!
        
        Boiling water...
        Adding tea leaves...
        Brewing...
        Adding lemon...
        Beverage is ready!
        ```

-   模板方法模式的优缺点 <br/>
    优点： <br/>
    代码复用：提取了公共逻辑到抽象类，避免重复代码。 <br/>
          灵活扩展：子类可以定制算法的特定步骤，而无需改变算法结构。 <br/>
          封装变化：将容易变化的部分封装到子类，符合开闭原则。 <br/>
    缺点： <br/>
    增加类的复杂度：每种具体实现需要一个子类，导致类数量增加。 <br/>
          限制灵活性：如果算法框架需要修改，可能需要调整所有子类。 <br/>

-   模板方法模式的应用场景 <br/>
    流程固定但部分步骤可变的场景：如制作流程、数据处理流程等。 <br/>
          需要复用通用逻辑的场景：如文件操作（打开、读取、关闭）。 <br/>
          具有稳定算法框架的场景：如 Web 框架中请求的处理流程。 <br/>

-   模板方法模式与其他模式的对比 <br/>
    
    | 模式   | 特点              | 适用场景             |
    |------|-----------------|------------------|
    | 模板方法模式 | 定义算法骨架，部分步骤由子类实现 | 固定流程的场景，需要不同实现部分步骤 |
    | 策略模式 | 动态选择算法，算法实现相互独立 | 算法或行为需要灵活切换的场景 |
    | 工厂方法模式 | 延迟对象实例化，由子类决定具体实现 | 需要动态生成对象且不确定其具体类型的场景 |

-   现实生活类比 <br/>
    制作模板：一个蛋糕制作过程固定：准备材料、混合、烘焙等，但不同种类蛋糕的配料和烘焙时间可以不同。 <br/>
          教育课程：上课流程（签到、讲课、讨论、复习）固定，但不同课程的内容和方法各有特色。 <br/>

-   总结 <br/>
    模板方法模式通过固定算法框架并将具体实现延迟到子类，为复杂算法的复用和扩展提供了灵活性。它特别适用于具有稳定流程框架但部分步骤可能变化的场景，在许多系统中（如框架设计、数据处理）有广泛应用。 <br/>


### 策略模式（Strategy Pattern） {#策略模式-strategy-pattern}

策略模式是一种行为型设计模式，用于定义一系列算法（或行为），将每个算法封装到独立的类中，并使它们可以互相替换，从而让算法的变化独立于使用算法的客户端。 <br/>

-   策略模式的核心思想 <br/>
    封装变化：将不同的算法或行为封装到独立的策略类中，避免硬编码在客户端代码中。 <br/>
          面向接口编程：客户端通过接口与策略交互，而不关心策略的具体实现。 <br/>
          动态替换：客户端可以在运行时选择或切换不同的策略。 <br/>

-   策略模式的结构 <br/>
    核心角色： <br/>
    -   Context（上下文类） <br/>
        持有对策略对象的引用，负责与客户端交互。将具体的算法委托给策略对象执行。 <br/>
        
        -   Strategy（策略接口） <br/>
        
        定义算法或行为的通用接口。 <br/>
        
        -   ConcreteStrategy（具体策略类） <br/>
        
        实现策略接口，定义具体的算法或行为。 <br/>

-   策略模式的实现示例：支付系统 <br/>
    场景：一个在线支付系统支持多种支付方式（如信用卡、PayPal、比特币）。根据用户选择的支付方式执行相应的支付逻辑。 <br/>
    1.  策略接口 <br/>
        ```java
        // 策略接口
        public interface PaymentStrategy {
            void pay(double amount);
        }
        ```
-   具体策略类 <br/>
    信用卡支付 <br/>
    ```java
    // 具体策略：信用卡支付
    public class CreditCardPayment implements PaymentStrategy {
        private String cardNumber;
    
        public CreditCardPayment(String cardNumber) {
            this.cardNumber = cardNumber;
        }
    
        @Override
        public void pay(double amount) {
            System.out.println("Paid " + amount + " using Credit Card: " + cardNumber);
        }
    }
    ```
    PayPal支付 <br/>
    ```java
    // 具体策略：PayPal支付
    public class PayPalPayment implements PaymentStrategy {
        private String email;
    
        public PayPalPayment(String email) {
            this.email = email;
        }
    
        @Override
        public void pay(double amount) {
            System.out.println("Paid " + amount + " using PayPal account: " + email);
        }
    }
    ```
    比特币支付 <br/>
    ```java
    // 具体策略：比特币支付
    public class BitcoinPayment implements PaymentStrategy {
        private String walletAddress;
    
        public BitcoinPayment(String walletAddress) {
            this.walletAddress = walletAddress;
        }
    
        @Override
        public void pay(double amount) {
            System.out.println("Paid " + amount + " using Bitcoin wallet: " + walletAddress);
        }
    }
    ```
-   上下文类 <br/>
    ```java
    // 上下文类
    public class PaymentContext {
        private PaymentStrategy paymentStrategy;
    
        // 设置策略
        public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
            this.paymentStrategy = paymentStrategy;
        }
    
        // 执行支付
        public void executePayment(double amount) {
            if (paymentStrategy == null) {
                throw new IllegalStateException("Payment strategy is not set");
            }
            paymentStrategy.pay(amount);
        }
    }
    ```
-   客户端代码 <br/>
    ```java
    public class StrategyPatternDemo {
        public static void main(String[] args) {
            PaymentContext context = new PaymentContext();
    
            // 使用信用卡支付
            context.setPaymentStrategy(new CreditCardPayment("1234-5678-9876-5432"));
            context.executePayment(100.50);
    
            // 使用PayPal支付
            context.setPaymentStrategy(new PayPalPayment("user@example.com"));
            context.executePayment(200.75);
    
            // 使用比特币支付
            context.setPaymentStrategy(new BitcoinPayment("1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa"));
            context.executePayment(300.00);
        }
    }
    ```
    输出结果： <br/>
    ```text
    Paid 100.5 using Credit Card: 1234-5678-9876-5432
    Paid 200.75 using PayPal account: user@example.com
    Paid 300.0 using Bitcoin wallet: 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa
    ```

-   策略模式的优缺点 <br/>
    优点 <br/>
    开闭原则：可以自由添加新的策略类，而不影响已有代码。 <br/>
          代码复用：提取算法逻辑到独立类中，避免代码重复。 <br/>
          灵活扩展：客户端可以动态选择或切换不同的策略。 <br/>
    缺点 <br/>
    增加类的数量：每种策略都需要定义一个新的类，可能导致类数量增加。 <br/>
          上下文依赖：上下文类需要知道所有的策略，并负责设置策略对象。 <br/>

-   策略模式的应用场景 <br/>
    算法需要灵活切换的场景：如支付方式、排序算法等。 <br/>
          行为或功能多样但逻辑相似的场景：如日志记录（不同格式）、文件处理（不同编码）。 <br/>
          需要避免使用多重条件语句（如 if-else 或 switch-case）的场景：策略模式可以将这些条件替换为多态。 <br/>

-   策略模式与其他模式的对比 <br/>
    
    | 模式   | 特点             | 适用场景               |
    |------|----------------|--------------------|
    | 策略模式 | 动态选择算法，算法独立实现 | 算法或行为需要灵活切换的场景 |
    | 模板方法模式 | 固定算法框架，部分步骤由子类实现 | 算法框架固定但部分步骤可变的场景 |
    | 状态模式 | 通过状态对象改变上下文行为 | 对象的行为依赖于其状态，且状态切换频繁的场景 |

-   现实生活类比 <br/>
    导航应用：根据用户选择（步行、驾车、骑行）提供不同的路线规划。 <br/>
          支付场景：支持多种支付方式，如支付宝、微信、信用卡。 <br/>

-   总结 <br/>
    策略模式通过将算法或行为封装为独立的策略类，为客户端提供了灵活性和扩展性。它在需要动态选择算法、避免多重条件语句时特别有用，是实现面向对象设计中开闭原则的重要工具之一。 <br/>


### 命令模式（Command Pattern） {#命令模式-command-pattern}

命令模式是一种行为型设计模式，它将请求封装为对象，使得你可以用不同的请求、队列或日志来参数化对象，并支持撤销和恢复操作。 <br/>

-   命令模式的核心思想 <br/>
    封装请求：将请求和执行者解耦，通过将请求封装为对象来实现。 <br/>
          请求参数化：请求可以在运行时传递给不同的调用者。 <br/>
          支持扩展：可以方便地实现请求的记录、撤销和重做等操作。 <br/>

-   命令模式的结构 <br/>
    核心角色 <br/>
    Command（命令接口）：声明执行命令的接口。 <br/>
          ConcreteCommand（具体命令类）：实现 Command 接口，封装请求的实际操作。 <br/>
          Receiver（接收者）：执行命令的具体逻辑。 <br/>
          Invoker（调用者）：保存命令对象并通过调用命令对象的方法来触发请求。 <br/>
          Client（客户端）：创建具体命令对象，并将其与接收者绑定。 <br/>

-   命令模式的实现示例：家电遥控器 <br/>
    场景：实现一个遥控器，可以控制电灯和风扇的开关操作。用户可以按下按钮执行操作，也可以撤销上次的操作。 <br/>
    1.  命令接口 <br/>
        ```java
        // 命令接口
        public interface Command {
            void execute(); // 执行命令
            void undo();    // 撤销命令
        }
        ```
    2.  接收者 <br/>
        电灯 <br/>
        ```java
        // 接收者：电灯
        public class Light {
            public void turnOn() {
                System.out.println("Light is ON");
            }
        
            public void turnOff() {
                System.out.println("Light is OFF");
            }
        }
        ```
        风扇 <br/>
        ```java
        // 接收者：风扇
        public class Fan {
            public void start() {
                System.out.println("Fan is running");
            }
        
            public void stop() {
                System.out.println("Fan is stopped");
            }
        }
        ```
    3.  具体命令类 <br/>
        电灯命令 <br/>
        ```java
        // 具体命令：打开电灯
        public class LightOnCommand implements Command {
            private Light light;
        
            public LightOnCommand(Light light) {
                this.light = light;
            }
        
            @Override
            public void execute() {
                light.turnOn();
            }
        
            @Override
            public void undo() {
                light.turnOff();
            }
        }
        
        // 具体命令：关闭电灯
        public class LightOffCommand implements Command {
            private Light light;
        
            public LightOffCommand(Light light) {
                this.light = light;
            }
        
            @Override
            public void execute() {
                light.turnOff();
            }
        
            @Override
            public void undo() {
                light.turnOn();
            }
        }
        ```
        风扇命令 <br/>
        ```java
        // 具体命令：启动风扇
        public class FanOnCommand implements Command {
            private Fan fan;
        
            public FanOnCommand(Fan fan) {
                this.fan = fan;
            }
        
            @Override
            public void execute() {
                fan.start();
            }
        
            @Override
            public void undo() {
                fan.stop();
            }
        }
        
        // 具体命令：停止风扇
        public class FanOffCommand implements Command {
            private Fan fan;
        
            public FanOffCommand(Fan fan) {
                this.fan = fan;
            }
        
            @Override
            public void execute() {
                fan.stop();
            }
        
            @Override
            public void undo() {
                fan.start();
            }
        }
        ```
    4.  调用者 <br/>
        ```java
        // 调用者：遥控器
        public class RemoteControl {
            private Command command;   // 当前命令
            private Command lastCommand; // 上一个执行的命令，用于撤销
        
            public void setCommand(Command command) {
                this.command = command;
            }
        
            public void pressButton() {
                if (command != null) {
                    command.execute();
                    lastCommand = command;
                }
            }
        
            public void pressUndo() {
                if (lastCommand != null) {
                    lastCommand.undo();
                }
            }
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class CommandPatternDemo {
            public static void main(String[] args) {
                // 创建接收者
                Light light = new Light();
                Fan fan = new Fan();
        
                // 创建命令对象
                Command lightOn = new LightOnCommand(light);
                Command lightOff = new LightOffCommand(light);
                Command fanOn = new FanOnCommand(fan);
                Command fanOff = new FanOffCommand(fan);
        
                // 创建调用者
                RemoteControl remote = new RemoteControl();
        
                // 控制电灯
                remote.setCommand(lightOn);
                remote.pressButton();  // 输出：Light is ON
                remote.pressUndo();    // 输出：Light is OFF
        
                // 控制风扇
                remote.setCommand(fanOn);
                remote.pressButton();  // 输出：Fan is running
                remote.pressUndo();    // 输出：Fan is stopped
            }
        }
        ```
-   命令模式的优缺点 <br/>
    优点 <br/>
    解耦请求发送者和接收者：调用者不直接调用接收者的方法，双方独立变化。 <br/>
          扩展性强：可以很方便地增加新命令，而不需要修改已有代码。 <br/>
          支持撤销和重做：通过 undo 方法可以方便地实现撤销操作。 <br/>
          请求的记录和排队：命令对象可以被序列化，用于日志记录和请求队列。 <br/>
    缺点 <br/>
    类数量增加：每个具体命令类需要实现一个独立的类，可能导致类数量增加。 <br/>
          过于灵活：如果命令和接收者的关系较为简单，可能导致过度设计。 <br/>

-   命令模式的应用场景 <br/>
    需要封装请求的场景：如事务型操作、操作日志。 <br/>
          需要支持撤销和重做的场景：如文本编辑器的操作历史。 <br/>
          请求排队和延迟执行：如打印队列、线程池任务。 <br/>
          宏命令（组合命令）：一次执行多个命令的场景，如批处理任务。 <br/>

-   命令模式与其他模式的对比 <br/>
    
    | 模式  | 特点            | 适用场景         |
    |-----|---------------|--------------|
    | 命令模式 | 封装请求，支持撤销、重做、排队 | 请求解耦、操作日志、事务处理 |
    | 策略模式 | 动态选择算法    | 多种算法或行为需要灵活切换的场景 |
    | 责任链模式 | 请求沿着链传递，直到被处理 | 多个处理者动态确定请求的处理方式 |

-   总结 <br/>
    命令模式通过封装请求，实现了调用者与接收者的解耦，同时提供了灵活的扩展性。它适用于需要请求封装、操作撤销、事务管理等复杂场景，是设计模式中灵活且实用的一种模式。 <br/>


### 职责链模式（Chain of Responsibility Pattern） {#职责链模式-chain-of-responsibility-pattern}

职责链模式是一种行为型设计模式，用于将请求沿着一条处理链传递，直到有一个处理者处理该请求。这种模式可以动态地组合处理者，并简化请求的处理逻辑。 <br/>

-   职责链模式的核心思想 <br/>
    链式传递：将多个处理者串成一条链，每个处理者尝试处理请求，如果无法处理，则将请求传递给下一个处理者。 <br/>
          请求与处理解耦：请求的发送者无需了解哪个处理者会处理请求，处理者也无需知道请求的来源。 <br/>
          灵活性：通过调整链的结构或处理者的顺序，动态改变请求的处理逻辑。 <br/>

-   职责链模式的结构 <br/>
    核心角色 <br/>
    Handler（抽象处理者）：定义处理请求的接口，包含处理请求的方法和对下一个处理者的引用。 <br/>
          ConcreteHandler（具体处理者）：实现 Handler 接口，处理请求或将请求传递给下一个处理者。 <br/>
          Client（客户端）：创建请求并将其发送到链的起点。 <br/>

-   职责链模式的实现示例：请求处理系统 <br/>
    场景：一个简单的日志系统，支持多种日志级别（INFO、DEBUG、ERROR）。日志消息需要被不同的处理器处理（如控制台输出、文件输出、邮件通知等）。 <br/>
    1.  抽象处理者 <br/>
        ```java
        // 抽象处理者
        public abstract class Logger {
            public static final int INFO = 1;
            public static final int DEBUG = 2;
            public static final int ERROR = 3;
        
            protected int level;            // 当前处理者能处理的日志级别
            protected Logger nextLogger;    // 下一个处理者
        
            // 设置下一个处理者
            public void setNextLogger(Logger nextLogger) {
                this.nextLogger = nextLogger;
            }
        
            // 处理请求
            public void logMessage(int level, String message) {
                if (this.level <= level) {
                    write(message);
                }
                if (nextLogger != null) {
                    nextLogger.logMessage(level, message);
                }
            }
        
            // 抽象方法，由具体处理者实现
            protected abstract void write(String message);
        }
        ```
    2.  具体处理者 <br/>
        控制台日志处理器 <br/>
        ```java
        // 具体处理者：控制台日志
        public class ConsoleLogger extends Logger {
            public ConsoleLogger(int level) {
                this.level = level;
            }
        
            @Override
            protected void write(String message) {
                System.out.println("ConsoleLogger: " + message);
            }
        }
        ```
        文件日志处理器 <br/>
        ```java
        // 具体处理者：文件日志
        public class FileLogger extends Logger {
            public FileLogger(int level) {
                this.level = level;
            }
        
            @Override
            protected void write(String message) {
                System.out.println("FileLogger: " + message);
            }
        }
        ```
        邮件日志处理器 <br/>
        ```java
        // 具体处理者：邮件日志
        public class EmailLogger extends Logger {
            public EmailLogger(int level) {
                this.level = level;
            }
        
            @Override
            protected void write(String message) {
                System.out.println("EmailLogger: " + message);
            }
        }
        ```
    3.  客户端代码 <br/>
        ```java
        public class ChainOfResponsibilityDemo {
            public static void main(String[] args) {
                // 创建具体处理者
                Logger consoleLogger = new ConsoleLogger(Logger.INFO);
                Logger fileLogger = new FileLogger(Logger.DEBUG);
                Logger emailLogger = new EmailLogger(Logger.ERROR);
        
                // 构建职责链
                consoleLogger.setNextLogger(fileLogger);
                fileLogger.setNextLogger(emailLogger);
        
                // 测试职责链
                System.out.println("INFO level log:");
                consoleLogger.logMessage(Logger.INFO, "This is an information.");
        
                System.out.println("\nDEBUG level log:");
                consoleLogger.logMessage(Logger.DEBUG, "This is a debug level message.");
        
                System.out.println("\nERROR level log:");
                consoleLogger.logMessage(Logger.ERROR, "This is an error message.");
            }
        }
        ```
        输出结果 <br/>
        ```java
        INFO level log:
        ConsoleLogger: This is an information.
        
        DEBUG level log:
        ConsoleLogger: This is a debug level message.
        FileLogger: This is a debug level message.
        
        ERROR level log:
        ConsoleLogger: This is an error message.
        FileLogger: This is an error message.
        EmailLogger: This is an error message.
        ```

-   职责链模式的优缺点 <br/>
    优点 <br/>
    解耦请求和处理：请求的发送者和接收者解耦，发送者无需关心具体的处理者。 <br/>
          动态组合：可以灵活调整链的顺序或结构以满足新的需求。 <br/>
          单一职责原则：每个处理者只需专注于自己的职责。 <br/>
    缺点 <br/>
    请求未必被处理：如果链中没有合适的处理者，可能导致请求丢失。 <br/>
          调试困难：由于请求沿着链传递，可能难以确定实际的处理者。 <br/>
          性能问题：如果链过长，可能导致性能下降。 <br/>

-   职责链模式的应用场景 <br/>
    日志系统：日志按级别分层次处理，如控制台输出、文件保存、远程发送。 <br/>
          权限校验：用户请求需要经过多个校验步骤，如身份验证、权限验证、数据验证。 <br/>
          事件处理：图形界面事件从最内层组件向外层组件传递（如按钮点击事件）。 <br/>
          责任分级：如客服系统，将客户问题从普通客服逐级传递到高级客服。 <br/>

-   职责链模式与其他模式的对比 <br/>
    
    | 模式  | 特点               | 适用场景            |
    |-----|------------------|-----------------|
    | 职责链模式 | 请求沿着链传递，直到被某个处理者处理 | 请求有多个可能的处理者且需要动态分配时 |
    | 命令模式 | 封装请求，支持撤销、排队等 | 需要将请求封装为对象并动态执行的场景 |
    | 观察者模式 | 多个观察者订阅一个主题 | 事件通知机制，多对多依赖关系 |

-   总结 <br/>
    职责链模式通过将请求与处理者解耦，实现了处理逻辑的灵活性和可扩展性。它特别适合处理分层结构或请求需要动态分配的场景，是一种重要的设计模式。 <br/>


### 状态模式（State Pattern） {#状态模式-state-pattern}

状态模式是一种行为型设计模式，允许一个对象在其内部状态改变时改变其行为。这种模式将对象的行为封装在不同的状态类中，外部无需知道对象的具体状态，能够实现更灵活和可维护的状态切换。 <br/>

-   状态模式的核心思想 <br/>
    将状态封装为类：每种状态对应一个独立的类，实现了特定状态下的行为。 <br/>
          状态切换：对象的行为依赖于其内部状态的变化，可以动态切换状态。 <br/>
          解耦状态与逻辑：状态与具体的行为实现解耦，使代码更加清晰和易于扩展。 <br/>

-   状态模式的结构 <br/>
    核心角色 <br/>
    Context（上下文类）：维护当前的状态对象，并定义一个状态接口供外部调用；负责在不同状态间切换。 <br/>
          State（抽象状态）：定义一个接口，封装与上下文的一个特定状态相关的行为。 <br/>
          ConcreteState（具体状态类）：实现状态接口，定义该状态下的具体行为。 <br/>

-   状态模式的实现示例：电梯状态 <br/>
    场景：模拟电梯的工作状态，包括 开启、关闭、运行 和 停止 四种状态，电梯的行为会根据当前状态而不同。 <br/>
    1.  抽象状态类 <br/>
        ```java
        // 抽象状态类
        public interface ElevatorState {
            void openDoor();     // 开门
            void closeDoor();    // 关门
            void run();          // 运行
            void stop();         // 停止
        }
        ```
    2.  具体状态类 <br/>
        开门状态 <br/>
        ```java
        // 开门状态
        public class OpenState implements ElevatorState {
            private ElevatorContext context;
        
            public OpenState(ElevatorContext context) {
                this.context = context;
            }
        
            @Override
            public void openDoor() {
                System.out.println("The door is already open.");
            }
        
            @Override
            public void closeDoor() {
                System.out.println("Closing the door...");
                context.setState(context.getClosedState());
            }
        
            @Override
            public void run() {
                System.out.println("Cannot run while the door is open!");
            }
        
            @Override
            public void stop() {
                System.out.println("Elevator is already stopped.");
            }
        }
        ```
        关门状态 <br/>
        ```java
        // 关门状态
        public class ClosedState implements ElevatorState {
            private ElevatorContext context;
        
            public ClosedState(ElevatorContext context) {
                this.context = context;
            }
        
            @Override
            public void openDoor() {
                System.out.println("Opening the door...");
                context.setState(context.getOpenState());
            }
        
            @Override
            public void closeDoor() {
                System.out.println("The door is already closed.");
            }
        
            @Override
            public void run() {
                System.out.println("Elevator starts running...");
                context.setState(context.getRunningState());
            }
        
            @Override
            public void stop() {
                System.out.println("Elevator is already stopped.");
            }
        }
        ```
        运行状态 <br/>
        ```java
        // 运行状态
        public class RunningState implements ElevatorState {
            private ElevatorContext context;
        
            public RunningState(ElevatorContext context) {
                this.context = context;
            }
        
            @Override
            public void openDoor() {
                System.out.println("Cannot open the door while running!");
            }
        
            @Override
            public void closeDoor() {
                System.out.println("The door is already closed.");
            }
        
            @Override
            public void run() {
                System.out.println("Elevator is already running.");
            }
        
            @Override
            public void stop() {
                System.out.println("Stopping the elevator...");
                context.setState(context.getStoppedState());
            }
        }
        ```
        停止状态 <br/>
        ```java
        // 停止状态
        public class StoppedState implements ElevatorState {
            private ElevatorContext context;
        
            public StoppedState(ElevatorContext context) {
                this.context = context;
            }
        
            @Override
            public void openDoor() {
                System.out.println("Opening the door...");
                context.setState(context.getOpenState());
            }
        
            @Override
            public void closeDoor() {
                System.out.println("Closing the door...");
                context.setState(context.getClosedState());
            }
        
            @Override
            public void run() {
                System.out.println("Elevator starts running...");
                context.setState(context.getRunningState());
            }
        
            @Override
            public void stop() {
                System.out.println("Elevator is already stopped.");
            }
        }
        ```
    3.  上下文类 <br/>
        ```java
        // 上下文类
        public class ElevatorContext {
            private ElevatorState openState;
            private ElevatorState closedState;
            private ElevatorState runningState;
            private ElevatorState stoppedState;
        
            private ElevatorState currentState;
        
            public ElevatorContext() {
                openState = new OpenState(this);
                closedState = new ClosedState(this);
                runningState = new RunningState(this);
                stoppedState = new StoppedState(this);
        
                currentState = stoppedState; // 默认状态为停止
            }
        
            public void setState(ElevatorState state) {
                this.currentState = state;
            }
        
            public ElevatorState getOpenState() {
                return openState;
            }
        
            public ElevatorState getClosedState() {
                return closedState;
            }
        
            public ElevatorState getRunningState() {
                return runningState;
            }
        
            public ElevatorState getStoppedState() {
                return stoppedState;
            }
        
            // 操作接口
            public void openDoor() {
                currentState.openDoor();
            }
        
            public void closeDoor() {
                currentState.closeDoor();
            }
        
            public void run() {
                currentState.run();
            }
        
            public void stop() {
                currentState.stop();
            }
        }
        ```
    4.  客户端代码 <br/>
        ```java
        public class StatePatternDemo {
            public static void main(String[] args) {
                ElevatorContext elevator = new ElevatorContext();
        
                elevator.openDoor();  // 输出：Opening the door...
                elevator.closeDoor(); // 输出：Closing the door...
                elevator.run();       // 输出：Elevator starts running...
                elevator.openDoor();  // 输出：Cannot open the door while running!
                elevator.stop();      // 输出：Stopping the elevator...
                elevator.openDoor();  // 输出：Opening the door...
            }
        }
        ```

-   状态模式的优缺点 <br/>
    优点 <br/>
    状态和行为解耦：状态和行为的逻辑被封装到具体状态类中，易于扩展和维护。 <br/>
          代码更清晰：使用状态类取代复杂的条件判断语句。 <br/>
          可扩展性强：新增状态或行为时，只需增加新的状态类，无需修改原有代码。 <br/>
    缺点 <br/>
    类数量增加：每个状态对应一个类，可能导致类数量过多。 <br/>
          状态切换可能复杂：如果状态之间的依赖关系复杂，可能增加实现难度。 <br/>

-   状态模式的应用场景 <br/>
    对象行为随状态变化而变化：如电梯的运行逻辑、订单的状态变化。 <br/>
          消除大量的条件语句：状态的切换逻辑可通过多态替代复杂的 if-else 或 switch。 <br/>
          允许状态动态切换：游戏中的角色状态（如站立、跑动、攻击）或文件传输的状态。 <br/>

-   总结 <br/>
    状态模式通过将状态的行为封装到不同的类中，使得对象的行为可以随状态的变化而变化。这种模式在处理状态复杂、切换频繁的场景中非常实用，可以显著提高代码的可维护性和扩展性。 <br/>


### 观察者模式（Observer Pattern） {#观察者模式-observer-pattern}

观察者模式是一种行为型设计模式，它定义了一种一对多的依赖关系：当被观察对象（主题）发生变化时，所有依赖它的对象（观察者）都会自动收到通知并更新。这种模式广泛用于实现事件驱动系统，例如订阅-发布机制。 <br/>

-   观察者模式的核心思想 <br/>
    一对多关系：一个主题可以有多个观察者，当主题发生变化时，它会通知所有观察者。 <br/>
          解耦：观察者和主题之间是松散耦合的，观察者只需订阅感兴趣的主题即可，不需要知道其他观察者的存在。 <br/>
          灵活性：可以动态添加或移除观察者，适应变化的需求。 <br/>

-   观察者模式的结构 <br/>
    核心角色 <br/>
    Subject（主题）：被观察的对象，维护观察者列表，并提供添加、删除观察者的方法；当状态发生变化时，通知所有观察者。 <br/>
          Observer（观察者）：定义一个更新接口，当主题状态发生变化时，主题会调用该接口通知观察者。 <br/>
          ConcreteSubject（具体主题）：实现主题的具体行为，保存主题的状态。 <br/>
          ConcreteObserver（具体观察者）：实现观察者接口，定义在接收到通知后应采取的具体操作。 <br/>

-   观察者模式的实现示例：天气预报系统 <br/>
    场景：一个天气预报系统需要通知多个观察者（如手机APP、网页、广播系统）天气变化的情况。 <br/>
    1.  定义观察者接口 <br/>
        ```java
        // 观察者接口
        public interface Observer {
            void update(float temperature, float humidity, float pressure);
        }
        ```
    2.  定义主题接口 <br/>
        ```java
        // 主题接口
        public interface Subject {
            void registerObserver(Observer observer);  // 注册观察者
            void removeObserver(Observer observer);    // 移除观察者
            void notifyObservers();                    // 通知所有观察者
        }
        ```
    3.  具体主题实现 <br/>
        ```java
        // 具体主题：天气数据
        import java.util.ArrayList;
        import java.util.List;
        
        public class WeatherData implements Subject {
            private List<Observer> observers;  // 保存观察者列表
            private float temperature;         // 温度
            private float humidity;            // 湿度
            private float pressure;            // 气压
        
            public WeatherData() {
                observers = new ArrayList<>();
            }
        
            @Override
            public void registerObserver(Observer observer) {
                observers.add(observer);
            }
        
            @Override
            public void removeObserver(Observer observer) {
                observers.remove(observer);
            }
        
            @Override
            public void notifyObservers() {
                for (Observer observer : observers) {
                    observer.update(temperature, humidity, pressure);
                }
            }
        
            // 模拟天气数据发生变化
            public void setMeasurements(float temperature, float humidity, float pressure) {
                this.temperature = temperature;
                this.humidity = humidity;
                this.pressure = pressure;
                notifyObservers(); // 数据变化时通知所有观察者
            }
        }
        ```
    4.  具体观察者实现 <br/>
        手机APP观察者 <br/>
        ```java
        // 具体观察者：手机APP
        public class MobileAppDisplay implements Observer {
            @Override
            public void update(float temperature, float humidity, float pressure) {
                System.out.println("MobileAppDisplay: Temperature = " + temperature 
        ​            + ", Humidity = " + humidity + ", Pressure = " + pressure);
            }
        }
        ```
        网页观察者 <br/>
        ```java
        // 具体观察者：网页
        public class WebDisplay implements Observer {
            @Override
            public void update(float temperature, float humidity, float pressure) {
                System.out.println("WebDisplay: Temperature = " + temperature 
        ​            + ", Humidity = " + humidity + ", Pressure = " + pressure);
            }
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class ObserverPatternDemo {
            public static void main(String[] args) {
                // 创建主题（天气数据）
                WeatherData weatherData = new WeatherData();
        
                // 创建观察者
                Observer mobileApp = new MobileAppDisplay();
                Observer webDisplay = new WebDisplay();
        
                // 注册观察者
                weatherData.registerObserver(mobileApp);
                weatherData.registerObserver(webDisplay);
        
                // 模拟天气数据变化
                System.out.println("Weather Update #1:");
                weatherData.setMeasurements(25.5f, 65.0f, 1013.0f);
        
                System.out.println("\nWeather Update #2:");
                weatherData.setMeasurements(27.0f, 70.0f, 1012.5f);
            }
        }
        ```
    6.  输出结果 <br/>
        ```text
        Weather Update #1:
        MobileAppDisplay: Temperature = 25.5, Humidity = 65.0, Pressure = 1013.0
        WebDisplay: Temperature = 25.5, Humidity = 65.0, Pressure = 1013.0
        
        Weather Update #2:
        MobileAppDisplay: Temperature = 27.0, Humidity = 70.0, Pressure = 1012.5
        WebDisplay: Temperature = 27.0, Humidity = 70.0, Pressure = 1012.5
        ```

-   观察者模式的优缺点 <br/>
    优点 <br/>
    解耦：主题和观察者独立开发，降低了系统的耦合性。 <br/>
          动态扩展：可以动态添加或移除观察者，满足灵活的需求。 <br/>
          支持广播机制：一个主题可以同时通知多个观察者。 <br/>
    缺点 <br/>
    性能问题：如果观察者数量过多，通知可能会影响性能。 <br/>
          通知顺序不确定：观察者的通知顺序不保证一致，可能引发不确定性。 <br/>
          循环依赖：如果观察者和主题之间存在循环依赖，可能引发死循环。 <br/>

-   观察者模式的应用场景 <br/>
    事件订阅-发布系统：GUI事件处理系统（按钮点击、输入框变化）。 <br/>
          消息推送系统：社交媒体通知机制、新闻订阅推送。 <br/>
          模型-视图更新：MVC架构中，视图订阅模型的变化并动态更新。 <br/>
          监听机制：文件系统的目录变化监听（如Java的 FileWatcher）。 <br/>

-   观察者模式与其他模式对比 <br/>
    
    | 模式    | 特点         | 适用场景           |
    |-------|------------|----------------|
    | 观察者模式 | 一对多通知，动态订阅 | 需要事件驱动机制或状态变化通知的场景 |
    | 发布-订阅模式 | 一对多通知，带中介者 | 更复杂的事件系统，解耦发布者和订阅者 |
    | 职责链模式 | 请求沿链传递，按顺序处理 | 多个处理者按顺序处理请求 |

-   总结 <br/>
    观察者模式通过维护观察者列表，实现了被观察者状态变化时的通知机制，在事件驱动系统中非常实用。它提供了解耦和动态扩展的能力，但需注意性能和循环依赖问题。 <br/>


### 中介者模式（Mediator Pattern） {#中介者模式-mediator-pattern}

中介者模式是一种行为型设计模式，用于降低多个对象之间的复杂耦合关系。通过引入一个中介者对象，各个对象不直接相互引用，而是通过中介者进行交互，从而实现松散耦合。 <br/>

-   中介者模式的核心思想 <br/>
    核心角色：用一个中介者来封装对象之间的交互逻辑，各个对象只需与中介者交互，而无需直接引用其他对象。 <br/>
          解耦：将多个对象之间的复杂关系简化为中介者和对象的一对多关系，减少系统的耦合度。 <br/>
          集中控制：中介者统一管理和协调对象之间的通信，便于扩展和维护。 <br/>

-   中介者模式的结构 <br/>
    核心角色 <br/>
    Mediator（中介者接口）：定义与同事对象（Colleague）的交互接口，协调各同事类的行为。 <br/>
          ConcreteMediator（具体中介者）：实现中介者接口，具体协调各同事类之间的交互行为。 <br/>
    Colleague（同事类抽象接口）：定义依赖于中介者的对象接口，同事类通过中介者进行通信。 <br/>
          ConcreteColleague（具体同事类）：实现同事类接口，与中介者交互实现自身逻辑。 <br/>

-   中介者模式的实现示例：聊天室 <br/>
    场景：设计一个聊天室系统，多个用户可以发送消息给其他用户，但用户之间的通信通过聊天室中介者协调实现。 <br/>
    1.  定义中介者接口 <br/>
        ```java
        // 中介者接口
        public interface ChatMediator {
            void sendMessage(String message, User user);  // 发送消息
            void addUser(User user);                     // 添加用户
        }
        ```
    2.  具体中介者实现 <br/>
        ```java
        // 具体中介者：聊天室
        import java.util.ArrayList;
        import java.util.List;
        
        public class ChatRoom implements ChatMediator {
            private List<User> users; // 用户列表
        
            public ChatRoom() {
                this.users = new ArrayList<>();
            }
        
            @Override
            public void addUser(User user) {
                users.add(user);
            }
        
            @Override
            public void sendMessage(String message, User sender) {
                for (User user : users) {
                    if (user != sender) { // 消息不发给发送者自己
                        user.receiveMessage(message);
                    }
                }
            }
        }
        ```
    3.  定义同事类抽象 <br/>
        ```java
        // 同事类接口
        public abstract class User {
            protected ChatMediator mediator;
            protected String name;
        
            public User(ChatMediator mediator, String name) {
                this.mediator = mediator;
                this.name = name;
            }
        
            public abstract void sendMessage(String message);    // 发送消息
            public abstract void receiveMessage(String message); // 接收消息
        }
        ```
    4.  具体同事类实现 <br/>
        ```java
        // 具体同事类：普通用户
        public class ChatUser extends User {
            public ChatUser(ChatMediator mediator, String name) {
                super(mediator, name);
            }
        
            @Override
            public void sendMessage(String message) {
                System.out.println(this.name + " sends: " + message);
                mediator.sendMessage(message, this);
            }
        
            @Override
            public void receiveMessage(String message) {
                System.out.println(this.name + " receives: " + message);
            }
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class MediatorPatternDemo {
            public static void main(String[] args) {
                // 创建聊天室中介者
                ChatMediator chatRoom = new ChatRoom();
        
                // 创建用户
                User user1 = new ChatUser(chatRoom, "Alice");
                User user2 = new ChatUser(chatRoom, "Bob");
                User user3 = new ChatUser(chatRoom, "Charlie");
        
                // 添加用户到聊天室
                chatRoom.addUser(user1);
                chatRoom.addUser(user2);
                chatRoom.addUser(user3);
        
                // 用户发送消息
                user1.sendMessage("Hi everyone!");
                user2.sendMessage("Hello Alice!");
            }
        }
        ```
    6.  输出结果 <br/>
        ```text
        Alice sends: Hi everyone!
        Bob receives: Hi everyone!
        Charlie receives: Hi everyone!
        
        Bob sends: Hello Alice!
        Alice receives: Hello Alice!
        Charlie receives: Hello Alice!
        ```

-   中介者模式的优缺点 <br/>
    优点 <br/>
    降低耦合性：对象之间不再直接引用，减少了类与类之间的依赖。 <br/>
          集中控制：交互逻辑集中在中介者中，更易于维护和扩展。 <br/>
          灵活性：可以动态地添加或移除同事类，扩展中介者的行为。 <br/>
    缺点 <br/>
    中介者复杂化：中介者集中管理所有的交互逻辑，可能会导致代码复杂度增加。 <br/>
          潜在性能问题：如果同事类过多或交互过于复杂，可能导致中介者性能瓶颈。 <br/>

-   中介者模式的应用场景 <br/>
    GUI组件交互：在图形界面开发中，各组件（如按钮、文本框）通过中介者协调交互。 <br/>
          系统模块间通信：微服务架构中，各服务之间通过消息中介（如RabbitMQ、Kafka）进行通信。 <br/>
          聊天系统：用户之间通过聊天室服务器进行消息传递。 <br/>
          航班调度系统：飞机通过控制塔（中介者）协调起降。 <br/>

-   中介者模式与其他模式对比 <br/>
    
    | 模式  | 特点                    | 适用场景         |
    |-----|-----------------------|--------------|
    | 中介者模式 | 将对象之间的交互逻辑集中到中介者，解耦对象关系 | 需要集中管理复杂的交互逻辑时 |
    | 观察者模式 | 一对多的通知机制        | 需要动态订阅和通知的事件驱动场景 |
    | 职责链模式 | 请求沿责任链传递，由某个节点处理 | 需要动态确定请求的处理者时 |

-   总结 <br/>
    中介者模式通过引入一个中介者对象，简化了多个对象之间的交互关系，适用于需要集中管理交互逻辑的复杂系统。虽然可能增加中介者的复杂度，但它极大地降低了耦合度，提高了系统的可维护性和扩展性。 <br/>


### 迭代器模式（Iterator Pattern） {#迭代器模式-iterator-pattern}

迭代器模式是一种行为型设计模式，用于提供一种顺序访问集合对象中元素的方法，而无需暴露其内部表示。它将遍历集合的职责交给迭代器对象，从而使集合类和迭代器实现解耦。 <br/>

-   迭代器模式的核心思想 <br/>
    将集合对象的遍历逻辑封装到迭代器中，使集合对象专注于自身的数据存储和管理。 <br/>
          通过迭代器访问集合，用户无需了解集合的具体实现。 <br/>
          提供统一的遍历接口，支持不同类型集合的一致性访问。 <br/>

-   迭代器模式的结构 <br/>
    核心角色 <br/>
    Iterator（迭代器接口）：定义访问和遍历元素的方法（如 next() 和 hasNext()）。 <br/>
          ConcreteIterator（具体迭代器）：实现迭代器接口，负责遍历集合中的元素。 <br/>
          Aggregate（聚合接口）：定义创建迭代器的方法（如 createIterator()）。 <br/>
          ConcreteAggregate（具体聚合类）：实现聚合接口，提供存储元素和创建具体迭代器的功能。 <br/>

-   迭代器模式的实现示例 <br/>
    场景：实现一个自定义的集合类，让用户能够通过迭代器顺序访问集合中的元素。 <br/>
    1.  定义迭代器接口 <br/>
        ```java
        // 迭代器接口
        public interface Iterator<T> {
            boolean hasNext(); // 判断是否有下一个元素
            T next();          // 获取下一个元素
        }
        ```
    2.  定义聚合接口 <br/>
        ```java
        // 聚合接口
        public interface Aggregate<T> {
            Iterator<T> createIterator(); // 创建迭代器
        }
        ```
    3.  实现具体聚合类 <br/>
        ```java
        // 具体聚合类
        import java.util.ArrayList;
        import java.util.List;
        
        public class CustomCollection<T> implements Aggregate<T> {
            private List<T> items = new ArrayList<>(); // 存储集合元素
        
            public void add(T item) {
                items.add(item);
            }
        
            public T get(int index) {
                return items.get(index);
            }
        
            public int size() {
                return items.size();
            }
        
            @Override
            public Iterator<T> createIterator() {
                return new CustomIterator<>(this);
            }
        }
        ```
    4.  实现具体迭代器类 <br/>
        ```java
        // 具体迭代器类
        public class CustomIterator<T> implements Iterator<T> {
            private CustomCollection<T> collection;
            private int currentIndex = 0;
        
            public CustomIterator(CustomCollection<T> collection) {
                this.collection = collection;
            }
        
            @Override
            public boolean hasNext() {
                return currentIndex < collection.size();
            }
        
            @Override
            public T next() {
                return collection.get(currentIndex++);
            }
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class IteratorPatternDemo {
            public static void main(String[] args) {
                // 创建自定义集合
                CustomCollection<String> collection = new CustomCollection<>();
                collection.add("Apple");
                collection.add("Banana");
                collection.add("Cherry");
        
                // 创建迭代器
                Iterator<String> iterator = collection.createIterator();
        
                // 使用迭代器访问集合元素
                while (iterator.hasNext()) {
                    System.out.println(iterator.next());
                }
            }
        }
        ```
    6.  输出结果 <br/>
        ```text
        Apple
        Banana
        Cherry
        ```

-   迭代器模式的优缺点 <br/>
    优点 <br/>
    统一接口：通过统一的接口遍历不同类型的集合，提高代码的通用性和可扩展性。 <br/>
          解耦：将集合的遍历逻辑与集合本身解耦，集合可以专注于存储数据，而迭代器负责遍历。 <br/>
          灵活性：支持不同类型的遍历算法（如正向遍历、反向遍历）。 <br/>
    缺点 <br/>
    开销增加：为每种集合实现具体迭代器类，可能增加代码复杂性和系统开销。 <br/>
          可能破坏封装性：如果迭代器暴露了集合的内部结构，可能导致集合封装性受损。 <br/>

-   迭代器模式的应用场景 <br/>
    标准库集合类：Java的 Iterator 接口和集合框架（如 List、Set 等）就是迭代器模式的经典实现。 <br/>
          需要隐藏集合内部结构：用户无需关心集合的具体实现，只需通过迭代器访问元素。 <br/>
          需要多种遍历方式：对集合数据支持多种遍历逻辑，如双向遍历、跳跃遍历等。 <br/>

-   Java标准库中的迭代器模式 <br/>
    Java中的 java.util.Iterator 是迭代器模式的经典实现。以下是示例代码： <br/>
    使用标准库的迭代器 <br/>
    ```java
    import java.util.ArrayList;
    import java.util.Iterator;
    import java.util.List;
    
    public class JavaIteratorExample {
        public static void main(String[] args) {
            // 使用Java集合框架
            List<String> list = new ArrayList<>();
            list.add("Dog");
            list.add("Cat");
            list.add("Rabbit");
    
            // 获取迭代器
            Iterator<String> iterator = list.iterator();
    
            // 遍历集合
            while (iterator.hasNext()) {
                System.out.println(iterator.next());
            }
        }
    }
    ```
    输出结果 <br/>
    ```text
    Dog
    Cat
    Rabbit
    ```

-   迭代器模式与其他模式对比 <br/>
    
    | 模式  | 特点               | 适用场景         |
    |-----|------------------|--------------|
    | 迭代器模式 | 提供统一的方式访问集合中的元素 | 需要遍历不同类型的集合数据 |
    | 组合模式 | 通过树形结构管理对象 | 需要以树形结构组织和访问对象 |
    | 访问者模式 | 为对象结构增加操作，而不改变对象本身 | 需要对复杂对象结构执行不同操作时 |

-   总结 <br/>
    迭代器模式通过将遍历逻辑与集合解耦，为用户提供了一种统一的访问方式，在各种集合操作中得到了广泛应用。无论是自定义集合还是Java标准集合框架，迭代器模式都发挥着重要作用。 <br/>


### 访问者模式（Visitor Pattern） {#访问者模式-visitor-pattern}

访问者模式是一种行为型设计模式，用于将作用于某种对象结构中的操作与对象结构本身进行解耦。通过访问者模式，可以在不改变对象结构的前提下，定义作用于这些对象的新操作。 <br/>

-   访问者模式的核心思想 <br/>
    双重分派：将操作和数据分开。数据结构负责管理和提供接口，而操作由访问者封装。 <br/>
          开放封闭原则：通过新增访问者，扩展对象的功能，而无需修改对象本身。 <br/>

-   访问者模式的结构 <br/>
    核心角色 <br/>
    Visitor（访问者接口）：定义访问对象结构中各类具体元素的接口。 <br/>
          ConcreteVisitor（具体访问者）：实现访问者接口，定义作用于元素的具体操作。 <br/>
          Element（元素接口）：定义一个 accept 方法，接收访问者对象。 <br/>
          ConcreteElement（具体元素）：实现元素接口，定义自己的数据和行为。 <br/>
          ObjectStructure（对象结构）：包含一组元素对象，提供遍历元素的方法，并允许访问者访问这些元素。 <br/>

-   访问者模式的实现示例 <br/>
    场景：设计一个系统，包含不同类型的员工（工程师、经理），我们希望通过访问者模式来实现对员工的不同操作，比如计算工资总额或生成绩效报告。 <br/>
    1.  定义访问者接口 <br/>
        ```java
        // 访问者接口
        public interface Visitor {
            void visit(Engineer engineer); // 访问工程师
            void visit(Manager manager);   // 访问经理
        }
        ```
    2.  定义元素接口 <br/>
        ```java
        // 元素接口
        public interface Employee {
            void accept(Visitor visitor); // 接收访问者
        }
        ```
    3.  实现具体元素 <br/>
        ```java
        // 工程师类
        public class Engineer implements Employee {
            private String name;
            private int codeLines; // 每月代码行数
        
            public Engineer(String name, int codeLines) {
                this.name = name;
                this.codeLines = codeLines;
            }
        
            public String getName() {
                return name;
            }
        
            public int getCodeLines() {
                return codeLines;
            }
        
            @Override
            public void accept(Visitor visitor) {
                visitor.visit(this);
            }
        }
        
        // 经理类
        public class Manager implements Employee {
            private String name;
            private int performance; // 绩效分数
        
            public Manager(String name, int performance) {
                this.name = name;
                this.performance = performance;
            }
        
            public String getName() {
                return name;
            }
        
            public int getPerformance() {
                return performance;
            }
        
            @Override
            public void accept(Visitor visitor) {
                visitor.visit(this);
            }
        }
        ```
    4.  实现具体访问者 <br/>
        ```java
        // 具体访问者：工资计算
        public class SalaryCalculator implements Visitor {
            @Override
            public void visit(Engineer engineer) {
                System.out.println("Calculating salary for Engineer: " + engineer.getName());
                System.out.println("Salary based on code lines: " + engineer.getCodeLines() * 10);
            }
        
            @Override
            public void visit(Manager manager) {
                System.out.println("Calculating salary for Manager: " + manager.getName());
                System.out.println("Salary based on performance: " + manager.getPerformance() * 100);
            }
        }
        
        // 具体访问者：绩效报告生成
        public class PerformanceReporter implements Visitor {
            @Override
            public void visit(Engineer engineer) {
                System.out.println("Generating performance report for Engineer: " + engineer.getName());
                System.out.println("Code lines this month: " + engineer.getCodeLines());
            }
        
            @Override
            public void visit(Manager manager) {
                System.out.println("Generating performance report for Manager: " + manager.getName());
                System.out.println("Performance score: " + manager.getPerformance());
            }
        }
        ```
    5.  创建对象结构 <br/>
        ```java
        import java.util.ArrayList;
        import java.util.List;
        
        // 对象结构
        public class Company {
            private List<Employee> employees = new ArrayList<>();
        
            public void addEmployee(Employee employee) {
                employees.add(employee);
            }
        
            public void accept(Visitor visitor) {
                for (Employee employee : employees) {
                    employee.accept(visitor);
                }
            }
        }
        ```
    6.  客户端代码 <br/>
        ```java
        public class VisitorPatternDemo {
            public static void main(String[] args) {
                // 创建公司对象结构
                Company company = new Company();
        
                // 添加员工
                company.addEmployee(new Engineer("Alice", 5000));
                company.addEmployee(new Manager("Bob", 85));
        
                // 创建访问者
                Visitor salaryCalculator = new SalaryCalculator();
                Visitor performanceReporter = new PerformanceReporter();
        
                // 执行操作
                System.out.println("=== Salary Calculation ===");
                company.accept(salaryCalculator);
        
                System.out.println("\n=== Performance Reporting ===");
                company.accept(performanceReporter);
            }
        }
        ```
    7.  输出结果 <br/>
        ```text
        === Salary Calculation ===
        Calculating salary for Engineer: Alice
        Salary based on code lines: 50000
        Calculating salary for Manager: Bob
        Salary based on performance: 8500
        
        === Performance Reporting ===
        Generating performance report for Engineer: Alice
        Code lines this month: 5000
        Generating performance report for Manager: Bob
        Performance score: 85
        ```

-   访问者模式的优缺点 <br/>
    优点 <br/>
    符合开放封闭原则：可以新增访问者实现，而无需修改原有的元素类。 <br/>
          功能扩展方便：适合为复杂对象结构添加多种操作。 <br/>
          分离数据和行为：元素只负责数据存储，访问者负责操作，职责清晰。 <br/>
    缺点 <br/>
    违反单一职责原则：如果访问者需要访问多种元素，会将多个操作耦合到一个访问者中。 <br/>
          对象结构固定：如果对象结构经常变化，维护访问者模式会非常困难。 <br/>
          双向依赖：访问者依赖于元素的接口，元素也依赖于访问者。 <br/>

-   访问者模式的应用场景 <br/>
    复杂对象结构：系统中包含多种类型的对象，且需要对这些对象执行多种操作。 <br/>
          需要扩展操作而不修改对象结构：需要经常新增操作，但不希望修改已有的类。 <br/>
          编译器设计：如语法树的操作（遍历、优化、代码生成等）使用访问者模式。 <br/>
          报表生成：对不同类型的报表元素生成不同的格式和内容。 <br/>

-   访问者模式与其他模式对比 <br/>
    
    | 模式   | 特点                   | 适用场景                |
    |------|----------------------|---------------------|
    | 访问者模式 | 将操作封装在访问者中，解耦对象结构与操作逻辑 | 需要对对象结构定义多种操作，并保持数据结构不变 |
    | 迭代器模式 | 提供统一的方式遍历对象集合 | 遍历对象集合中的元素    |
    | 模板方法模式 | 固定算法框架，允许子类覆盖部分实现 | 算法步骤固定，但部分实现可变化 |

-   总结 <br/>
    访问者模式适合在稳定的数据结构和频繁变化的操作逻辑之间切换。虽然可能增加系统复杂度，但它提供了灵活扩展功能的能力，在实际开发中非常有用。 <br/>


### 备忘录模式（Memento Pattern） {#备忘录模式-memento-pattern}

备忘录模式是一种行为型设计模式，用于在不破坏对象封装性的前提下，捕获并保存对象的状态，以便以后恢复到该状态。 <br/>

-   备忘录模式的核心思想 <br/>
    封装性：对象的状态信息存储在备忘录中，对外不暴露对象的内部细节。 <br/>
          回滚操作：允许在需要时将对象恢复到先前的状态。 <br/>

-   备忘录模式的结构 <br/>
    核心角色 <br/>
    Originator（发起者）：负责创建和恢复自身状态。 <br/>
          Memento（备忘录）：存储发起者的内部状态。对发起者以外的其他对象是不可见的。 <br/>
          Caretaker（负责人）：负责保存备忘录，但不能对备忘录的内容进行操作或修改。 <br/>

-   备忘录模式的实现示例 <br/>
    场景：模拟一个文本编辑器，它可以保存文本的历史状态并在需要时撤销。 <br/>
    1.  定义发起者类 <br/>
        ```java
        // 发起者类
        public class TextEditor {
            private String text;
        
            public void setText(String text) {
                this.text = text;
            }
        
            public String getText() {
                return text;
            }
        
            // 创建备忘录
            public Memento save() {
                return new Memento(text);
            }
        
            // 从备忘录恢复
            public void restore(Memento memento) {
                this.text = memento.getText();
            }
        
            // 备忘录内部类
            public static class Memento {
                private final String text;
        
                private Memento(String text) {
                    this.text = text;
                }
        
                private String getText() {
                    return text;
                }
            }
        }
        ```
    2.  定义负责人类 <br/>
        ```java
        // 负责人类
        import java.util.Stack;
        
        public class Caretaker {
            private final Stack<TextEditor.Memento> history = new Stack<>();
        
            // 保存备忘录
            public void save(TextEditor.Memento memento) {
                history.push(memento);
            }
        
            // 获取最近的备忘录
            public TextEditor.Memento undo() {
                if (!history.isEmpty()) {
                    return history.pop();
                }
                return null;
            }
        }
        ```
    3.  客户端代码 <br/>
        ```java
        public class MementoPatternDemo {
            public static void main(String[] args) {
                // 创建发起者和负责人
                TextEditor editor = new TextEditor();
                Caretaker caretaker = new Caretaker();
        
                // 设置初始文本
                editor.setText("Hello, World!");
                System.out.println("Initial Text: " + editor.getText());
        
                // 保存状态
                caretaker.save(editor.save());
        
                // 修改文本
                editor.setText("Hello, Design Patterns!");
                System.out.println("Modified Text: " + editor.getText());
        
                // 撤销到之前的状态
                editor.restore(caretaker.undo());
                System.out.println("After Undo: " + editor.getText());
            }
        }
        ```
    4.  输出结果 <br/>
        ```text
        Initial Text: Hello, World!
        Modified Text: Hello, Design Patterns!
        After Undo: Hello, World!
        ```

-   备忘录模式的优缺点 <br/>
    优点 <br/>
    状态保存：在不破坏对象封装性的前提下，保存并恢复对象状态。 <br/>
          便于撤销与恢复：为实现撤销（Undo）功能提供了天然支持。 <br/>
          封装性：发起者和备忘录的内部状态对外界隐藏，符合单一职责原则。 <br/>
    缺点 <br/>
    资源开销：频繁保存对象状态会增加内存和时间开销。 <br/>
          实现复杂度：如果对象状态较为复杂（如引用其他对象），实现备忘录的存储和恢复可能较困难。 <br/>

-   备忘录模式的应用场景 <br/>
    需要记录和恢复对象状态：游戏中的存档与加载功能；文本编辑器的撤销与恢复功能。 <br/>
          需要保存历史状态以支持回退：数据库事务管理。 <br/>
          需要提供一个后悔操作的功能：图形编辑器的操作记录。 <br/>

-   备忘录模式与其他模式对比 <br/>
    
    | 模式  | 特点              | 适用场景             |
    |-----|-----------------|------------------|
    | 备忘录模式 | 保存对象的状态以支持恢复 | 需要保存和回滚操作的系统 |
    | 命令模式 | 封装请求为对象，同时支持撤销和恢复 | 需要对请求进行封装并支持撤销/重做的场景 |
    | 原型模式 | 通过克隆对象来保存状态 | 创建对象成本较高，且需要复制当前状态时 |

-   Java中的备忘录模式实例 <br/>
    Java中的 java.util.Stack 和 java.util.LinkedList 可以用来存储备忘录，用于实现撤销/恢复功能。 <br/>
    示例：栈实现撤销功能 <br/>
    ```java
    import java.util.Stack;
    
    public class MementoExample {
        public static void main(String[] args) {
            Stack<String> history = new Stack<>();
    
            // 当前状态
            String currentState = "State1";
            System.out.println("Current State: " + currentState);
    
            // 保存状态
            history.push(currentState);
    
            // 修改状态
            currentState = "State2";
            System.out.println("Current State: " + currentState);
    
            // 撤销
            currentState = history.pop();
            System.out.println("After Undo: " + currentState);
        }
    }
    ```
    输出结果： <br/>
    ```text
    Current State: State1
    Current State: State2
    After Undo: State1
    ```

-   总结 <br/>
    备忘录模式通过封装对象状态，为恢复和撤销功能提供了解耦和灵活的实现方案，是实现状态回滚和撤销功能的首选模式之一。在实际开发中，需根据对象状态的复杂程度选择合适的存储方式和策略，以平衡性能和功能需求。 <br/>


### 解释器模式（Interpreter Pattern） {#解释器模式-interpreter-pattern}

解释器模式是一种行为型设计模式，用来定义一种语言的文法，并通过解释器来解析和执行语言中的句子。 <br/>

-   解释器模式的核心思想 <br/>
    将一个语言的语法规则表示为类（抽象语法树）。 <br/>
          通过递归调用这些类的实例来解释（执行）一个特定的语句或表达式。 <br/>
          适用于需要解释的语言或定制化语法（如脚本语言、规则引擎等）。 <br/>

-   解释器模式的结构 <br/>
    核心角色 <br/>
    AbstractExpression（抽象表达式）：定义解释操作的接口。 <br/>
          TerminalExpression（终结符表达式）：实现与文法中终结符相关的解释操作。 <br/>
    NonTerminalExpression（非终结符表达式）：实现与文法中非终结符相关的解释操作，通常递归调用其他表达式。 <br/>
          Context（上下文）：包含解释器之外的全局信息，用于存储或传递数据。 <br/>
          Client（客户端）：构建语法树（表达式的组合）并通过上下文调用解释操作。 <br/>

-   解释器模式的实现示例 <br/>
    场景：我们需要实现一个简单的数学表达式解释器，支持加法和减法。 <br/>
    1.  定义抽象表达式 <br/>
        ```java
        // 抽象表达式接口
        public interface Expression {
            int interpret(Context context);
        }
        ```
    2.  定义终结符表达式 <br/>
        ```java
        // 终结符表达式：数字
        public class NumberExpression implements Expression {
            private final int number;
        
            public NumberExpression(int number) {
                this.number = number;
            }
        
            @Override
            public int interpret(Context context) {
                return number;
            }
        }
        ```
    3.  定义非终结符表达式 <br/>
        ```java
        // 非终结符表达式：加法
        public class AddExpression implements Expression {
            private final Expression leftExpression;
            private final Expression rightExpression;
        
            public AddExpression(Expression leftExpression, Expression rightExpression) {
                this.leftExpression = leftExpression;
                this.rightExpression = rightExpression;
            }
        
            @Override
            public int interpret(Context context) {
                return leftExpression.interpret(context) + rightExpression.interpret(context);
            }
        }
        
        // 非终结符表达式：减法
        public class SubtractExpression implements Expression {
            private final Expression leftExpression;
            private final Expression rightExpression;
        
            public SubtractExpression(Expression leftExpression, Expression rightExpression) {
                this.leftExpression = leftExpression;
                this.rightExpression = rightExpression;
            }
        
            @Override
            public int interpret(Context context) {
                return leftExpression.interpret(context) - rightExpression.interpret(context);
            }
        }
        ```
    4.  定义上下文类 <br/>
        ```java
        // 上下文类（本例中无特殊逻辑，仅作占位）
        public class Context {
            // 可以扩展上下文以存储变量或全局信息
        }
        ```
    5.  客户端代码 <br/>
        ```java
        public class InterpreterPatternDemo {
            public static void main(String[] args) {
                // 创建上下文
                Context context = new Context();
        
                // 构建表达式： (5 + 3) - (2 + 1)
                Expression expression = new SubtractExpression(
                        new AddExpression(new NumberExpression(5), new NumberExpression(3)),
                        new AddExpression(new NumberExpression(2), new NumberExpression(1))
                );
        
                // 解释表达式
                int result = expression.interpret(context);
                System.out.println("Result: " + result); // 输出: 5
            }
        }
        ```
    6.  输出结果 <br/>
        ```text
        Result: 5
        ```

-   解释器模式的优缺点 <br/>
    优点 <br/>
    扩展性强：新增语法规则只需添加对应的表达式类。 <br/>
          结构清晰：语法规则与实现分离，符合单一职责原则。 <br/>
          灵活性高：客户端可以通过组合表达式实现复杂语法解析。 <br/>
    缺点 <br/>
    性能问题：随着表达式的复杂度增加，效率会显著下降（递归解析）。 <br/>
          可读性差：复杂的语法规则会导致类数量激增，难以维护。 <br/>
          应用场景有限：适合简单的语言规则，不适合复杂的语言解析。 <br/>

-   解释器模式的应用场景 <br/>
    定制语言解析：如脚本语言、DSL（领域特定语言）解析。 <br/>
          表达式求值：计算器、公式解析器。 <br/>
          语法规则引擎：如规则匹配、数据过滤引擎。 <br/>
          编译器设计：抽象语法树的构建和解释。 <br/>

-   解释器模式与其他模式的对比 <br/>
    
    | 模式  | 特点             | 适用场景            |
    |-----|----------------|-----------------|
    | 解释器模式 | 定义和解释语言的语法规则 | 需要对特定语言或表达式进行解析和执行 |
    | 命令模式 | 封装请求为对象，并支持执行和撤销 | 请求的行为固定且需要支持撤销功能的场景 |
    | 策略模式 | 动态替换算法实现 | 需要选择算法或行为的场景 |
    | 状态模式 | 根据状态切换行为 | 对象行为受状态影响且状态变化频繁的场景 |

-   Java中的解释器模式应用实例 <br/>
    正则表达式解析：Java 的 java.util.regex 使用解释器模式解析正则表达式语法。 <br/>
          表达式引擎：Spring 的 SpEL 表达式引擎。 <br/>

-   总结 <br/>
    解释器模式适合在系统需要固定且简单的语法规则解析时使用。它提供了灵活的扩展能力，但可能会导致性能和维护问题。在实际开发中，若语法复杂，可选择更专业的工具（如 ANTLR 或 Parsers）来代替解释器模式。 <br/>

