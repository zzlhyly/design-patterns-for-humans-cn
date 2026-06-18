# 设计模式通俗指南（C++ 版本）

<br>
<p align="center">
  <img src="./.github/banner.svg" height="150px" />
</p>

***

<p align="center">
🎉 超简化设计模式讲解！🎉
</p>
<p align="center">
设计模式是一个容易让人困惑的话题。在这里，我尝试用<i>最简单</i>的方式讲解，让它们深入你的心（也许还有我的心）。
</p>

***

> **注意：** 本版本为 C++ 实现版本，原项目和 PHP 版本请参阅 [README.md](./README.md) 和 [README_CN.md](./README_CN.md)。

<sub>查看我的[其他项目](http://roadmap.sh)并在[Twitter](https://twitter.com/kamrify)上打个招呼。</sub>

<br>

|[创建型设计模式（Creational）](#创建型设计模式)|[结构型设计模式（Structural）](#结构型设计模式)|[行为型设计模式（Behavioral）](#行为型设计模式)|
|:-|:-|:-|
|[简单工厂](#-简单工厂-simple-factory)|[适配器](#-适配器-adapter)|[责任链](#-责任链-chain-of-responsibility)|
|[工厂方法](#-工厂方法-factory-method)|[桥接](#-桥接-bridge)|[命令](#-命令-command)|
|[抽象工厂](#-抽象工厂-abstract-factory)|[组合](#-组合-composite)|[迭代器](#-迭代器-iterator)|
|[建造者](#-建造者-builder)|[装饰器](#-装饰器-decorator)|[中介者](#-中介者-mediator)|
|[原型](#-原型-prototype)|[外观](#-外观-facade)|[备忘录](#-备忘录-memento)|
|[单例](#-单例-singleton)|[享元](#-享元-flyweight)|[观察者](#-观察者-observer)|
||[代理](#-代理-proxy)|[访问者](#-访问者-visitor)|
|||[策略](#-策略-strategy)|
|||[状态](#-状态-state)|
|||[模板方法](#-模板方法-template-method)|

<br>

## 引言

设计模式是针对反复出现的问题的解决方案；**是处理特定问题的指南**。它们不是可以插入应用程序并等待奇迹发生的类、包或库。相反，它们是在特定情况下处理特定问题的指南。

> 设计模式是针对反复出现的问题的解决方案；是处理特定问题的指南

维基百科将其描述为

> 在软件工程中，软件设计模式是在给定上下文中针对常见问题的通用可重用解决方案。它不是可以直接转换为源代码或机器代码的最终设计。它是如何解决问题的描述或模板，可以在许多不同情况下使用。

⚠️ 注意事项
-----------------
- 设计模式不是解决所有问题的银弹。
- 不要强行使用它们；如果这样做，可能会产生不良后果。
- 请记住，设计模式是**针对**问题的解决方案，而不是**寻找**问题的解决方案；所以不要过度思考。
- 如果在正确的地方以正确的方式使用，它们可以成为救星；否则可能导致代码混乱。

> 还要注意，下面的代码示例是用 C++ 编写的，但这不应该阻止你，因为概念是相同的。

设计模式类型
-----------------

* [创建型](#创建型设计模式-creational-design-patterns)
* [结构型](#结构型设计模式-structural-design-patterns)
* [行为型](#行为型设计模式-behavioral-design-patterns)

## 创建型设计模式（Creational Design Patterns）

通俗地说
> 创建型模式专注于如何实例化一个对象或一组相关对象。

维基百科说
> 在软件工程中，创建型设计模式是处理对象创建机制的设计模式，试图以适合情况的方式创建对象。对象创建的基本形式可能导致设计问题或增加设计的复杂性。创建型设计模式通过某种方式控制这种对象创建来解决这个问题。

 * [简单工厂](#-简单工厂-simple-factory)
 * [工厂方法](#-工厂方法-factory-method)
 * [抽象工厂](#-抽象工厂-abstract-factory)
 * [建造者](#-建造者-builder)
 * [原型](#-原型-prototype)
 * [单例](#-单例-singleton)

### 🏠 简单工厂（Simple Factory）

现实世界的例子
> 假设你正在建造一栋房子，需要门。你可以穿上木匠的衣服，准备一些木材、胶水、钉子和所有制作门所需的工具，然后在你的房子里开始制作；或者你可以直接打电话给工厂，让他们把制作好的门送到你这里，这样你就不需要学习任何关于门制作的知识，也不需要处理制作过程中产生的混乱。

通俗地说
> 简单工厂只是为客户端生成一个实例，而不向客户端暴露任何实例化逻辑。

维基百科说
> 在面向对象编程（OOP）中，工厂是一个用于创建其他对象的对象——正式地说，工厂是一个函数或方法，它从某个方法调用返回不同原型或类的对象，这个方法调用被认为是"new"。

**编程示例**

首先，我们有一个门接口和它的实现
```cpp
class Door {
public:
    virtual ~Door() = default;
    virtual float getWidth() const = 0;
    virtual float getHeight() const = 0;
};

class WoodenDoor : public Door {
protected:
    float width;
    float height;

public:
    WoodenDoor(float width, float height) : width(width), height(height) {}
    
    float getWidth() const override { return width; }
    float getHeight() const override { return height; }
};
```
然后我们有制作门的工厂
```cpp
class DoorFactory {
public:
    static Door* makeDoor(float width, float height) {
        return new WoodenDoor(width, height);
    }
};
```
然后可以这样使用
```cpp
// Make me a door of 100x200
Door* door = DoorFactory::makeDoor(100, 200);

std::cout << "Width: " << door->getWidth() << std::endl;
std::cout << "Height: " << door->getHeight() << std::endl;

// Make me a door of 50x100
Door* door2 = DoorFactory::makeDoor(50, 100);

// Don't forget to clean up
delete door;
delete door2;
```

**何时使用？**

当创建对象不仅仅是几个赋值操作，而是涉及一些逻辑时，将其放在专门的工厂中是有意义的，而不是在各处重复相同的代码。

### 🏭 工厂方法（Factory Method）

现实世界的例子
> 考虑招聘经理的情况。一个人不可能面试所有职位。根据职位空缺，她必须决定并将面试步骤委托给不同的人。

通俗地说
> 它提供了一种将实例化逻辑委托给子类的方法。

维基百科说
> 在基于类的编程中，工厂方法模式是一种创建型模式，它使用工厂方法来处理创建对象的问题，而无需指定将要创建的对象的确切类。这是通过调用工厂方法来创建对象来完成的——工厂方法可以在接口中指定并由子类实现，或者在基类中实现并可选地由派生类覆盖——而不是通过调用构造函数。

**编程示例**

以上面的招聘经理为例。首先，我们有一个面试官接口和一些实现

```cpp
class Interviewer {
public:
    virtual ~Interviewer() = default;
    virtual void askQuestions() = 0;
};

class Developer : public Interviewer {
public:
    void askQuestions() override {
        std::cout << "Asking about design patterns!" << std::endl;
    }
};

class CommunityExecutive : public Interviewer {
public:
    void askQuestions() override {
        std::cout << "Asking about community building" << std::endl;
    }
};
```

现在让我们创建我们的 `HiringManager`

```cpp
class HiringManager {
public:
    virtual ~HiringManager() = default;
    
    // Factory method
    virtual Interviewer* makeInterviewer() = 0;
    
    void takeInterview() {
        Interviewer* interviewer = this->makeInterviewer();
        interviewer->askQuestions();
        delete interviewer;
    }
};
```
现在任何子类都可以扩展它并提供所需的面试官
```cpp
class DevelopmentManager : public HiringManager {
protected:
    Interviewer* makeInterviewer() override {
        return new Developer();
    }
};

class MarketingManager : public HiringManager {
protected:
    Interviewer* makeInterviewer() override {
        return new CommunityExecutive();
    }
};
```
然后可以这样使用

```cpp
DevelopmentManager devManager;
devManager.takeInterview(); // Output: Asking about design patterns

MarketingManager marketingManager;
marketingManager.takeInterview(); // Output: Asking about community building.
```

**何时使用？**

当类中有一些通用处理，但所需的子类在运行时动态决定时很有用。换句话说，当客户端不知道它可能需要什么确切的子类时。

### 🔨 抽象工厂（Abstract Factory）

现实世界的例子
> 扩展我们简单工厂中的门示例。根据你的需求，你可能从木门店获得木门，从铁门店获得铁门，或者从相关商店获得PVC门。此外，你可能需要具有不同专业技能的人来安装门，例如木匠安装木门，焊工安装铁门等。正如你所看到的，门之间现在存在依赖关系，木门需要木匠，铁门需要焊工等。

通俗地说
> 工厂的工厂；一个将各个但相关/依赖的工厂分组在一起而不指定它们的具体类的工厂。

维基百科说
> 抽象工厂模式提供了一种封装一组具有共同主题的各个工厂的方法，而不指定它们的具体类。

**编程示例**

翻译上面的门示例。首先，我们有我们的 `Door` 接口和一些实现

```cpp
class Door {
public:
    virtual ~Door() = default;
    virtual void getDescription() = 0;
};

class WoodenDoor : public Door {
public:
    void getDescription() override {
        std::cout << "I am a wooden door" << std::endl;
    }
};

class IronDoor : public Door {
public:
    void getDescription() override {
        std::cout << "I am an iron door" << std::endl;
    }
};
```
然后我们有每种门类型的安装专家

```cpp
class DoorFittingExpert {
public:
    virtual ~DoorFittingExpert() = default;
    virtual void getDescription() = 0;
};

class Welder : public DoorFittingExpert {
public:
    void getDescription() override {
        std::cout << "I can only fit iron doors" << std::endl;
    }
};

class Carpenter : public DoorFittingExpert {
public:
    void getDescription() override {
        std::cout << "I can only fit wooden doors" << std::endl;
    }
};
```

现在我们有抽象工厂，它允许我们创建相关对象族，即木门工厂将创建木门和木门安装专家，铁门工厂将创建铁门和铁门安装专家
```cpp
class DoorFactory {
public:
    virtual ~DoorFactory() = default;
    virtual Door* makeDoor() = 0;
    virtual DoorFittingExpert* makeFittingExpert() = 0;
};

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory : public DoorFactory {
public:
    Door* makeDoor() override {
        return new WoodenDoor();
    }
    
    DoorFittingExpert* makeFittingExpert() override {
        return new Carpenter();
    }
};

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory : public DoorFactory {
public:
    Door* makeDoor() override {
        return new IronDoor();
    }
    
    DoorFittingExpert* makeFittingExpert() override {
        return new Welder();
    }
};
```
然后可以这样使用
```cpp
DoorFactory* woodenFactory = new WoodenDoorFactory();

Door* door = woodenFactory->makeDoor();
DoorFittingExpert* expert = woodenFactory->makeFittingExpert();

door->getDescription();  // Output: I am a wooden door
expert->getDescription(); // Output: I can only fit wooden doors

// Clean up
delete door;
delete expert;
delete woodenFactory;

// Same for Iron Factory
DoorFactory* ironFactory = new IronDoorFactory();

door = ironFactory->makeDoor();
expert = ironFactory->makeFittingExpert();

door->getDescription();  // Output: I am an iron door
expert->getDescription(); // Output: I can only fit iron doors

// Clean up
delete door;
delete expert;
delete ironFactory;
```

正如你所看到的，木门工厂封装了 `木匠` 和 `木门`，铁门工厂也封装了 `铁门` 和 `焊工`。这有助于我们确保对于每扇创建的门，我们不会得到错误的安装专家。

**何时使用？**

当存在相互关联的依赖关系，且涉及不太简单的创建逻辑时。

### 👷 建造者（Builder）

现实世界的例子
> 想象你在肯德基，你点了一个特定的套餐，比如"大份肯德基"，他们没有任何疑问就递给你；这是简单工厂的例子。但有些情况下创建逻辑可能涉及更多步骤。例如，你想要一个定制的赛百味套餐，你有几个关于汉堡制作方式的选项，比如你想要什么面包？你想要什么类型的酱料？你想要什么奶酪？在这种情况下，建造者模式就派上用场了。

通俗地说
> 允许你创建对象的不同变体，同时避免构造函数污染。当对象可能有多种变体时很有用。或者当对象的创建涉及很多步骤时。

维基百科说
> 建造者模式是一种对象创建软件设计模式，旨在找到解决可伸缩构造函数反模式的方案。

说到这里，让我补充一下什么是可伸缩构造函数反模式。在某个时候，我们都见过这样的构造函数：

```cpp
Burger(int size, bool cheese = true, bool pepperoni = true, bool tomato = false, bool lettuce = true)
{
}
```

正如你所看到的，构造函数参数的数量可能很快失控，理解参数的排列可能变得困难。此外，如果你想在未来添加更多选项，这个参数列表可能会继续增长。这被称为可伸缩构造函数反模式。

**编程示例**

明智的替代方案是使用建造者模式。首先，我们有我们想要制作的汉堡

```cpp
class Burger
{
protected:
    int size;
    bool cheese = false;
    bool pepperoni = false;
    bool lettuce = false;
    bool tomato = false;

public:
    Burger(int size, bool cheese, bool pepperoni, bool lettuce, bool tomato)
        : size(size), cheese(cheese), pepperoni(pepperoni), lettuce(lettuce), tomato(tomato) {}

    friend class BurgerBuilder;
};
```

然后我们有建造者

```cpp
class BurgerBuilder
{
    int size;
    bool cheese = false;
    bool pepperoni = false;
    bool lettuce = false;
    bool tomato = false;

public:
    BurgerBuilder(int size) : size(size) {}

    BurgerBuilder& addPepperoni()
    {
        pepperoni = true;
        return *this;
    }

    BurgerBuilder& addLettuce()
    {
        lettuce = true;
        return *this;
    }

    BurgerBuilder& addCheese()
    {
        cheese = true;
        return *this;
    }

    BurgerBuilder& addTomato()
    {
        tomato = true;
        return *this;
    }

    Burger build()
    {
        return Burger(size, cheese, pepperoni, lettuce, tomato);
    }
};
```
然后可以这样使用：

```cpp
Burger burger = BurgerBuilder(14)
                    .addPepperoni()
                    .addLettuce()
                    .addTomato()
                    .build();
```

**何时使用？**

当对象可能有多种变体时，以及为了避免构造函数可伸缩。与工厂模式的关键区别是：工厂模式用于创建是单步过程的情况，而建造者模式用于创建是多步过程的情况。

### 🐑 原型（Prototype）

现实世界的例子
> 还记得多莉吗？那只被克隆的羊！让我们不要深入细节，但关键是这都是关于克隆的。

通俗地说
> 通过克隆基于现有对象创建对象。

维基百科说
> 原型模式是软件开发中的一种创建型设计模式。当要创建的对象类型由原型实例确定时使用它，该原型实例被克隆以产生新对象。

简而言之，它允许你创建现有对象的副本并根据需要修改它，而不是从头创建对象并设置它的麻烦。

**编程示例**

在PHP中，可以使用 `clone` 轻松完成

```cpp
#include <string>

class Sheep
{
    std::string name;
    std::string category;

public:
    Sheep(const std::string& name, const std::string& category = "Mountain Sheep")
        : name(name), category(category) {}

    void setName(const std::string& name) { this->name = name; }
    std::string getName() const { return name; }

    void setCategory(const std::string& category) { this->category = category; }
    std::string getCategory() const { return category; }

    // Clone method for prototype pattern
    Sheep* clone() const { return new Sheep(*this); }
};
```
然后可以像下面这样克隆
```cpp
Sheep* original = new Sheep("Jolly");
std::cout << original->getName() << std::endl;      // Jolly
std::cout << original->getCategory() << std::endl;   // Mountain Sheep

// Clone and modify what is required
Sheep* cloned = original->clone();
cloned->setName("Dolly");
std::cout << cloned->getName() << std::endl;         // Dolly
std::cout << cloned->getCategory() << std::endl;     // Mountain Sheep

delete original;
delete cloned;
```

你也可以重写拷贝构造函数来修改克隆行为。

**何时使用？**

当需要类似于现有对象的对象时，或者当创建比克隆更昂贵时。

### 💍 单例（Singleton）

现实世界的例子
> 一个国家一次只能有一个总统。每当职责召唤时，都必须让同一个总统行动起来。这里的总统是单例。

通俗地说
> 确保特定类只有一个对象被创建。

维基百科说
> 在软件工程中，单例模式是一种将类的实例化限制为一个对象的软件设计模式。当只需要一个对象来协调系统中的操作时很有用。

单例模式实际上被认为是一种反模式，应避免过度使用。它不一定坏，可能有一些有效的用例，但应谨慎使用，因为它在应用程序中引入了全局状态，在一个地方的更改可能会影响其他区域，并且可能变得相当难以调试。另一个缺点是它使代码紧密耦合，加上模拟单例可能很困难。

**编程示例**

要创建单例，请将构造函数设为私有，禁用克隆，禁用扩展，并创建一个静态变量来容纳实例
```cpp
class President
{
    static President* instance;

    President()
    {
        // Hide the constructor
    }

public:
    // Disable copy constructor and assignment operator
    President(const President&) = delete;
    President& operator=(const President&) = delete;

    static President* getInstance()
    {
        if (!instance) {
            instance = new President();
        }
        return instance;
    }
};

President* President::instance = nullptr;
```
然后为了使用
```cpp
President* president1 = President::getInstance();
President* president2 = President::getInstance();

std::cout << (president1 == president2) << std::endl; // 1 (true)
```

## 结构型设计模式（Structural Design Patterns）

通俗地说
> 结构型模式主要关注对象组合，或者换句话说，实体如何相互使用。另一种解释是，它们有助于回答"如何构建软件组件？"

维基百科说
> 在软件工程中，结构型设计模式是通过识别实现关系之间的简单方法来简化设计的设计模式。

 * [适配器](#-适配器-adapter)
 * [桥接](#-桥接-bridge)
 * [组合](#-组合-composite)
 * [装饰器](#-装饰器-decorator)
 * [外观](#-外观-facade)
 * [享元](#-享元-flyweight)
 * [代理](#-代理-proxy)

### 🔌 适配器（Adapter）

现实世界的例子
> 假设你有一些图片在存储卡中，需要将它们传输到计算机。为了传输它们，你需要某种与计算机端口兼容的适配器，以便可以将存储卡连接到计算机。在这种情况下，读卡器就是适配器。
> 另一个例子是著名的电源适配器；三脚插头无法连接到两孔插座，需要使用电源适配器使其与两孔插座兼容。
> 另一个例子是翻译人员将一个人说的话翻译给另一个人。

通俗地说
> 适配器模式允许你将原本不兼容的对象包装在适配器中，使其与另一个类兼容。

维基百科说
> 在软件工程中，适配器模式是一种允许将现有类的接口用作另一个接口的软件设计模式。它通常用于使现有类与其他类一起工作，而无需修改它们的源代码。

**编程示例**

考虑一个有猎人和他猎杀狮子的游戏。

首先我们有一个 `Lion` 接口，所有类型的狮子都必须实现它

```cpp
class Lion
{
public:
    virtual ~Lion() = default;
    virtual void roar() = 0;
};

class AfricanLion : public Lion
{
public:
    void roar() override {}
};

class AsianLion : public Lion
{
public:
    void roar() override {}
};
```
猎人期望任何 `Lion` 接口的实现来猎杀。
```cpp
class Hunter
{
public:
    void hunt(Lion* lion)
    {
        lion->roar();
    }
};
```

现在假设我们必须在游戏中添加一个 `WildDog`，以便猎人也可以猎杀它。但我们不能直接这样做，因为狗有不同的接口。为了使它对我们的猎人兼容，我们必须创建一个兼容的适配器

```cpp
// This needs to be added to the game
class WildDog
{
public:
    void bark() {}
};

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter : public Lion
{
    WildDog* dog;

public:
    WildDogAdapter(WildDog* dog) : dog(dog) {}

    void roar() override
    {
        dog->bark();
    }
};
```
现在 `WildDog` 可以通过 `WildDogAdapter` 在我们的游戏中使用。

```cpp
WildDog* wildDog = new WildDog();
WildDogAdapter* wildDogAdapter = new WildDogAdapter(wildDog);

Hunter* hunter = new Hunter();
hunter->hunt(wildDogAdapter);

delete wildDogAdapter;
delete wildDog;
delete hunter;
```

### 🚡 桥接（Bridge）

现实世界的例子
> 假设你有一个包含不同页面的网站，你应该允许用户更改主题。你会怎么做？为每个主题创建每个页面的多个副本，还是只创建单独的主题并根据用户偏好加载它们？桥接模式允许你做后者，即：

![有无桥接模式的对比](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

通俗地说
> 桥接模式是关于优先使用组合而不是继承。实现细节从一个层次结构推送到另一个具有单独层次结构的对象。

维基百科说
> 桥接模式是软件工程中使用的一种设计模式，旨在"将抽象与其实现解耦，以便两者可以独立变化"。

**编程示例**

翻译上面的网页示例。这里我们有 `WebPage` 层次结构

```cpp
class Theme
{
public:
    virtual ~Theme() = default;
    virtual std::string getColor() = 0;
};

class WebPage
{
public:
    virtual ~WebPage() = default;
    virtual std::string getContent() = 0;
};

class About : public WebPage
{
    Theme* theme;

public:
    About(Theme* theme) : theme(theme) {}

    std::string getContent() override
    {
        return "About page in " + theme->getColor();
    }
};

class Careers : public WebPage
{
    Theme* theme;

public:
    Careers(Theme* theme) : theme(theme) {}

    std::string getContent() override
    {
        return "Careers page in " + theme->getColor();
    }
};
```
以及单独的主题层次结构
```cpp
class DarkTheme : public Theme
{
public:
    std::string getColor() override { return "Dark Black"; }
};

class LightTheme : public Theme
{
public:
    std::string getColor() override { return "Off white"; }
};

class AquaTheme : public Theme
{
public:
    std::string getColor() override { return "Light blue"; }
};
```
以及两个层次结构的使用
```cpp
DarkTheme darkTheme;

About about(&darkTheme);
Careers careers(&darkTheme);

std::cout << about.getContent() << std::endl;   // About page in Dark Black
std::cout << careers.getContent() << std::endl;  // Careers page in Dark Black
```

### 🌿 组合（Composite）

现实世界的例子
> 每个组织都由员工组成。每个员工都有相同的特征，即有工资、有一些职责、可能向某人汇报，也可能有一些下属等。

通俗地说
> 组合模式允许客户端以统一的方式处理单个对象。

维基百科说
> 在软件工程中，组合模式是一种分区设计模式。组合模式描述了一组对象应该以与单个对象实例相同的方式对待。组合的意图是将对象"组合"成树结构以表示部分-整体层次结构。实现组合模式允许客户端统一处理单个对象和组合。

**编程示例**

以上面的员工示例为例。这里我们有不同的员工类型

```cpp
#include <string>
#include <vector>

class Employee
{
public:
    virtual ~Employee() = default;
    virtual std::string getName() = 0;
    virtual void setSalary(float salary) = 0;
    virtual float getSalary() = 0;
    virtual std::vector<std::string> getRoles() = 0;
};

class Developer : public Employee
{
    float salary;
    std::string name;
    std::vector<std::string> roles;

public:
    Developer(const std::string& name, float salary)
        : name(name), salary(salary) {}

    std::string getName() override { return name; }
    void setSalary(float salary) override { this->salary = salary; }
    float getSalary() override { return salary; }
    std::vector<std::string> getRoles() override { return roles; }
};

class Designer : public Employee
{
    float salary;
    std::string name;
    std::vector<std::string> roles;

public:
    Designer(const std::string& name, float salary)
        : name(name), salary(salary) {}

    std::string getName() override { return name; }
    void setSalary(float salary) override { this->salary = salary; }
    float getSalary() override { return salary; }
    std::vector<std::string> getRoles() override { return roles; }
};
```

然后我们有一个由几种不同类型的员工组成的组织

```cpp
class Organization
{
    std::vector<Employee*> employees;

public:
    void addEmployee(Employee* employee)
    {
        employees.push_back(employee);
    }

    float getNetSalaries()
    {
        float netSalary = 0;

        for (auto& employee : employees) {
            netSalary += employee->getSalary();
        }

        return netSalary;
    }
};
```

然后可以这样使用

```cpp
// Prepare the employees
Employee* john = new Developer("John Doe", 12000);
Employee* jane = new Designer("Jane Doe", 15000);

// Add them to organization
Organization organization;
organization.addEmployee(john);
organization.addEmployee(jane);

std::cout << "Net salaries: " << organization.getNetSalaries() << std::endl; // Net Salaries: 27000

delete john;
delete jane;
```

### ☕ 装饰器（Decorator）

现实世界的例子

> 想象你经营一家汽车维修店，提供多种服务。现在你如何计算要收取的账单？你选择一项服务，并动态地不断添加所提供服务的价格，直到获得最终成本。这里每种类型的服务都是一个装饰器。

通俗地说
> 装饰器模式允许你通过将对象包装在装饰器类的对象中，在运行时动态更改对象的行为。

维基百科说
> 在面向对象编程中，装饰器模式是一种设计模式，它允许向单个对象添加行为，无论是静态还是动态，而不影响同一类中其他对象的行为。装饰器模式通常有助于遵守单一职责原则，因为它允许将功能划分为具有独特关注领域的类。

**编程示例**

以咖啡为例。首先我们有一个实现咖啡接口的简单咖啡

```cpp
class Coffee
{
public:
    virtual ~Coffee() = default;
    virtual int getCost() = 0;
    virtual std::string getDescription() = 0;
};

class SimpleCoffee : public Coffee
{
public:
    int getCost() override { return 10; }
    std::string getDescription() override { return "Simple coffee"; }
};
```
我们希望使代码可扩展，以允许在需要时修改它。让我们制作一些附加组件（装饰器）
```cpp
class MilkCoffee : public Coffee
{
    Coffee* coffee;

public:
    MilkCoffee(Coffee* coffee) : coffee(coffee) {}

    int getCost() override { return coffee->getCost() + 2; }
    std::string getDescription() override { return coffee->getDescription() + ", milk"; }
};

class WhipCoffee : public Coffee
{
    Coffee* coffee;

public:
    WhipCoffee(Coffee* coffee) : coffee(coffee) {}

    int getCost() override { return coffee->getCost() + 5; }
    std::string getDescription() override { return coffee->getDescription() + ", whip"; }
};

class VanillaCoffee : public Coffee
{
    Coffee* coffee;

public:
    VanillaCoffee(Coffee* coffee) : coffee(coffee) {}

    int getCost() override { return coffee->getCost() + 3; }
    std::string getDescription() override { return coffee->getDescription() + ", vanilla"; }
};
```

现在让我们制作一杯咖啡

```cpp
Coffee* someCoffee = new SimpleCoffee();
std::cout << someCoffee->getCost() << std::endl;         // 10
std::cout << someCoffee->getDescription() << std::endl;   // Simple coffee

someCoffee = new MilkCoffee(someCoffee);
std::cout << someCoffee->getCost() << std::endl;         // 12
std::cout << someCoffee->getDescription() << std::endl;   // Simple coffee, milk

someCoffee = new WhipCoffee(someCoffee);
std::cout << someCoffee->getCost() << std::endl;         // 17
std::cout << someCoffee->getDescription() << std::endl;   // Simple coffee, milk, whip

someCoffee = new VanillaCoffee(someCoffee);
std::cout << someCoffee->getCost() << std::endl;         // 20
std::cout << someCoffee->getDescription() << std::endl;   // Simple coffee, milk, whip, vanilla

delete someCoffee;
```

### 📦 外观（Facade）

现实世界的例子
> 你如何打开电脑？"按下电源按钮"你会说！这就是你所相信的，因为你正在使用电脑外部提供的简单接口，内部它必须做很多事情才能实现。这种复杂子系统的简单接口就是外观。

通俗地说
> 外观模式为复杂子系统提供简化的接口。

维基百科说
> 外观是一个为更大的代码体（如类库）提供简化接口的对象。

**编程示例**

以上面的电脑示例为例。这里我们有电脑类

```cpp
class Computer
{
public:
    void getElectricShock() { std::cout << "Ouch!" << std::endl; }
    void makeSound() { std::cout << "Beep beep!" << std::endl; }
    void showLoadingScreen() { std::cout << "Loading.." << std::endl; }
    void bam() { std::cout << "Ready to be used!" << std::endl; }
    void closeEverything() { std::cout << "Bup bup bup buzzzz!" << std::endl; }
    void sooth() { std::cout << "Zzzzz" << std::endl; }
    void pullCurrent() { std::cout << "Haaah!" << std::endl; }
};
```
这里我们有外观
```cpp
class ComputerFacade
{
    Computer* computer;

public:
    ComputerFacade(Computer* computer) : computer(computer) {}

    void turnOn()
    {
        computer->getElectricShock();
        computer->makeSound();
        computer->showLoadingScreen();
        computer->bam();
    }

    void turnOff()
    {
        computer->closeEverything();
        computer->pullCurrent();
        computer->sooth();
    }
};
```
现在使用外观
```cpp
Computer computer;
ComputerFacade facade(&computer);
facade.turnOn();  // Ouch! Beep beep! Loading.. Ready to be used!
facade.turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

### 🍃 享元（Flyweight）

现实世界的例子
> 你曾经在某个摊位喝过新鲜的茶吗？他们通常会制作超过你需求的一杯，并为其他顾客节省剩余的资源，例如燃气等。享元模式就是关于共享的。

通俗地说
> 它用于通过与类似对象共享尽可能多的数据来最小化内存使用或计算费用。

维基百科说
> 在计算机编程中，享元是一种软件设计模式。享元是一个通过与其他类似对象共享尽可能多的数据来最小化内存使用的对象；这是一种在简单重复表示将使用不可接受的内存量时大量使用对象的方法。

**编程示例**

翻译上面的茶示例。首先我们有茶类型和茶制作者

```cpp
#include <map>
#include <string>

// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea {};

// Acts as a factory and saves the tea
class TeaMaker
{
    std::map<std::string, KarakTea*> availableTea;

public:
    ~TeaMaker() {
        for (auto& pair : availableTea) {
            delete pair.second;
        }
    }

    KarakTea* make(const std::string& preference)
    {
        if (availableTea.find(preference) == availableTea.end()) {
            availableTea[preference] = new KarakTea();
        }
        return availableTea[preference];
    }
};
```

然后我们有 `TeaShop`，它接受订单并提供服务

```cpp
class TeaShop
{
    std::map<int, KarakTea*> orders;
    TeaMaker* teaMaker;

public:
    TeaShop(TeaMaker* teaMaker) : teaMaker(teaMaker) {}

    void takeOrder(const std::string& teaType, int table)
    {
        orders[table] = teaMaker->make(teaType);
    }

    void serve()
    {
        for (auto& order : orders) {
            std::cout << "Serving tea to table# " << order.first << std::endl;
        }
    }
};
```
可以这样使用

```cpp
TeaMaker teaMaker;
TeaShop shop(&teaMaker);

shop.takeOrder("less sugar", 1);
shop.takeOrder("more milk", 2);
shop.takeOrder("without sugar", 5);

shop.serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

### 🎱 代理（Proxy）

现实世界的例子
> 你曾经使用门禁卡通过门吗？打开那扇门有多种选择，即可以使用门禁卡打开，也可以按下绕过安全系统的按钮。门的主要功能是打开，但在其上添加了代理以添加一些功能。让我用下面的代码示例更好地解释它。

通俗地说
> 使用代理模式，一个类代表另一个类的功能。

维基百科说
> 代理，以其最一般的形式，是一个充当其他事物接口的类。代理是一个包装器或代理对象，客户端调用它以访问幕后的真实服务对象。代理的使用可以简单地转发到真实对象，或者可以提供额外的逻辑。在代理中可以提供额外的功能，例如当对真实对象的操作资源密集时进行缓存，或在调用对真实对象的操作之前检查先决条件。

**编程示例**

以上面的安全门示例为例。首先我们有门接口和门的实现

```cpp
#include <string>

class Door
{
public:
    virtual ~Door() = default;
    virtual void open() = 0;
    virtual void close() = 0;
};

class LabDoor : public Door
{
public:
    void open() override { std::cout << "Opening lab door" << std::endl; }
    void close() override { std::cout << "Closing the lab door" << std::endl; }
};
```
然后我们有一个代理来保护我们想要的任何门
```cpp
class SecuredDoor : public Door
{
    Door* door;

    bool authenticate(const std::string& password)
    {
        return password == "$ecr@t";
    }

public:
    SecuredDoor(Door* door) : door(door) {}

    void open(const std::string& password)
    {
        if (authenticate(password)) {
            door->open();
        } else {
            std::cout << "Big no! It ain't possible." << std::endl;
        }
    }

    void open() override {} // Not used directly

    void close() override
    {
        door->close();
    }
};
```
可以这样使用
```cpp
LabDoor labDoor;
SecuredDoor securedDoor(&labDoor);
securedDoor.open("invalid");   // Big no! It ain't possible.
securedDoor.open("$ecr@t");    // Opening lab door
securedDoor.close();           // Closing the lab door
```
另一个例子是某种数据映射器实现。例如，我最近使用这种模式为MongoDB制作了一个ODM（对象数据映射器），我在其中围绕mongo类编写了一个代理，同时利用魔术方法 `__call()`。所有方法调用都被代理到原始mongo类，检索到的结果按原样返回，但在 `find` 或 `findOne` 的情况下，数据被映射到所需的类对象，并且返回对象而不是 `Cursor`。

## 行为型设计模式（Behavioral Design Patterns）

通俗地说
> 它关注对象之间的责任分配。与结构型模式的不同之处在于，它们不仅指定结构，还概述了它们之间消息传递/通信的模式。换句话说，它们有助于回答"如何在软件组件中运行行为？"

维基百科说
> 在软件工程中，行为型设计模式是识别对象之间常见通信模式并实现这些模式的设计模式。通过这样做，这些模式增加了执行此通信的灵活性。

* [责任链](#-责任链-chain-of-responsibility)
* [命令](#-命令-command)
* [迭代器](#-迭代器-iterator)
* [中介者](#-中介者-mediator)
* [备忘录](#-备忘录-memento)
* [观察者](#-观察者-observer)
* [访问者](#-访问者-visitor)
* [策略](#-策略-strategy)
* [状态](#-状态-state)
* [模板方法](#-模板方法-template-method)

### 🔗 责任链（Chain of Responsibility）

现实世界的例子
> 例如，你在账户中设置了三种支付方式（`A`、`B` 和 `C`）；每种都有不同的金额。`A` 有100美元，`B` 有300美元，`C` 有1000美元，支付偏好选择为 `A` 然后 `B` 然后 `C`。你尝试购买价值210美元的东西。使用责任链，首先检查账户 `A` 是否可以进行购买，如果是，则进行购买并且链将被打破。如果不是，请求将转发到账户 `B` 检查金额，如果是，则链将被打破，否则请求将继续转发，直到找到合适的处理程序。这里 `A`、`B` 和 `C` 是链的链接，整个现象就是责任链。

通俗地说
> 它帮助构建对象链。请求从一端进入，并不断从对象传递到对象，直到找到合适的处理程序。

维基百科说
> 在面向对象设计中，责任链模式是一种由命令对象源和一系列处理对象组成的设计模式。每个处理对象都包含定义它可以处理的命令对象类型的逻辑；其余的被传递到链中的下一个处理对象。

**编程示例**

翻译上面的账户示例。首先我们有一个基础账户，包含将账户链接在一起的逻辑和一些账户

```cpp
#include <string>
#include <stdexcept>

class Account
{
protected:
    Account* successor = nullptr;
    float balance;

    bool canPay(float amount)
    {
        return balance >= amount;
    }

public:
    virtual ~Account() = default;

    void setNext(Account* account)
    {
        successor = account;
    }

    virtual std::string getName() = 0;

    void pay(float amountToPay)
    {
        if (canPay(amountToPay)) {
            std::cout << "Paid " << amountToPay << " using " << getName() << std::endl;
        } else if (successor) {
            std::cout << "Cannot pay using " << getName() << ". Proceeding .." << std::endl;
            successor->pay(amountToPay);
        } else {
            throw std::runtime_error("None of the accounts have enough balance");
        }
    }
};

class Bank : public Account
{
public:
    Bank(float balance) { this->balance = balance; }
    std::string getName() override { return "Bank"; }
};

class Paypal : public Account
{
public:
    Paypal(float balance) { this->balance = balance; }
    std::string getName() override { return "Paypal"; }
};

class Bitcoin : public Account
{
public:
    Bitcoin(float balance) { this->balance = balance; }
    std::string getName() override { return "Bitcoin"; }
};
```

现在让我们使用上面定义的链接（即银行、Paypal、比特币）准备链

```cpp
// Let's prepare a chain like below
//      bank -> paypal -> bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bitcoin

Bank bank(100);           // Bank with balance 100
Paypal paypal(200);       // Paypal with balance 200
Bitcoin bitcoin(300);     // Bitcoin with balance 300

bank.setNext(&paypal);
paypal.setNext(&bitcoin);

// Let's try to pay using the first priority i.e. bank
bank.pay(259);

// Output will be
// ==============
// Cannot pay using Bank. Proceeding ..
// Cannot pay using Paypal. Proceeding ..
// Paid 259 using Bitcoin
```

### 👮 命令（Command）

现实世界的例子
> 一个通用的例子是你在餐厅点餐。你（即 `客户端`）要求服务员（即 `调用者`）带一些食物（即 `命令`），服务员只是将请求转发给厨师（即 `接收者`），他知道做什么以及如何烹饪。
> 另一个例子是你（即 `客户端`）使用遥控器（`调用者`）打开（即 `命令`）电视（即 `接收者`）。

通俗地说
> 允许你将操作封装在对象中。此模式背后的关键思想是提供将客户端与接收者解耦的方法。

维基百科说
> 在面向对象编程中，命令模式是一种行为设计模式，其中对象用于封装执行操作或在以后触发事件所需的所有信息。此信息包括方法名称、拥有该方法的对象以及方法参数的值。

**编程示例**

首先我们有接收者，它具有可以执行的每个操作的实现
```cpp
// Receiver
class Bulb
{
public:
    void turnOn() { std::cout << "Bulb has been lit" << std::endl; }
    void turnOff() { std::cout << "Darkness!" << std::endl; }
};
```
然后我们有一个接口，每个命令都将实现它，然后我们有一组命令
```cpp
// Command interface
class Command
{
public:
    virtual ~Command() = default;
    virtual void execute() = 0;
    virtual void undo() = 0;
    virtual void redo() = 0;
};

class TurnOn : public Command
{
    Bulb* bulb;

public:
    TurnOn(Bulb* bulb) : bulb(bulb) {}

    void execute() override { bulb->turnOn(); }
    void undo() override { bulb->turnOff(); }
    void redo() override { execute(); }
};

class TurnOff : public Command
{
    Bulb* bulb;

public:
    TurnOff(Bulb* bulb) : bulb(bulb) {}

    void execute() override { bulb->turnOff(); }
    void undo() override { bulb->turnOn(); }
    void redo() override { execute(); }
};
```
然后我们有一个 `调用者`，客户端将与之交互以处理任何命令
```cpp
// Invoker
class RemoteControl
{
public:
    void submit(Command* command)
    {
        command->execute();
    }
};
```
最后让我们看看如何在客户端中使用它
```cpp
Bulb bulb;

TurnOn turnOn(&bulb);
TurnOff turnOff(&bulb);

RemoteControl remote;
remote.submit(&turnOn);   // Bulb has been lit
remote.submit(&turnOff);  // Darkness!
```

命令模式也可用于实现基于事务的系统。你在执行命令时立即维护命令历史记录。如果最终命令成功执行，则一切正常，否则只需遍历历史记录并对所有已执行的命令执行 `undo`。

### ➿ 迭代器（Iterator）

现实世界的例子
> 旧收音机将是迭代器的一个很好的例子，用户可以从某个频道开始，然后使用下一个或上一个按钮浏览相应的频道。或者以MP3播放器或电视机为例，你可以按下下一个和上一个按钮浏览连续的频道，或者换句话说，它们都提供了一个接口来迭代相应的频道、歌曲或电台。

通俗地说
> 它提供了一种访问对象元素的方式，而不暴露底层表示。

维基百科说
> 在面向对象编程中，迭代器是一种设计模式，其中迭代器用于遍历容器并访问容器的元素。迭代器模式将算法与容器解耦；在某些情况下，算法特定于容器，因此无法解耦。

**编程示例**

在PHP中，使用SPL（标准PHP库）很容易实现。翻译上面的电台示例。首先我们有 `RadioStation`

```cpp
class RadioStation
{
    float frequency;

public:
    RadioStation(float frequency) : frequency(frequency) {}
    float getFrequency() const { return frequency; }
};
```
然后我们有我们的迭代器

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

class StationList
{
    std::vector<RadioStation> stations;
    size_t counter = 0;

public:
    void addStation(const RadioStation& station)
    {
        stations.push_back(station);
    }

    void removeStation(const RadioStation& toRemove)
    {
        auto it = std::remove_if(stations.begin(), stations.end(),
            [&toRemove](const RadioStation& station) {
                return station.getFrequency() == toRemove.getFrequency();
            });
        stations.erase(it, stations.end());
    }

    size_t count() const { return stations.size(); }

    // Iterator-like interface
    void rewind() { counter = 0; }

    bool hasNext() const { return counter < stations.size(); }

    RadioStation& next() { return stations[counter++]; }
};
```
然后可以这样使用
```cpp
StationList stationList;

stationList.addStation(RadioStation(89));
stationList.addStation(RadioStation(101));
stationList.addStation(RadioStation(102));
stationList.addStation(RadioStation(103.2));

stationList.rewind();
while (stationList.hasNext()) {
    std::cout << stationList.next().getFrequency() << std::endl;
}

stationList.removeStation(RadioStation(89)); // Will remove station 89
```

### 👽 中介者（Mediator）

现实世界的例子
> 一个通用的例子是当你通过手机与某人交谈时，有一个网络提供商坐在你们之间，你们的对话通过它而不是直接发送。在这种情况下，网络提供商是中介者。

通俗地说
> 中介者模式添加了一个第三方对象（称为中介者）来控制两个对象（称为同事）之间的交互。它有助于减少相互通信的类之间的耦合。因为现在它们不需要了解彼此的实现。

维基百科说
> 在软件工程中，中介者模式定义了一个封装一组对象如何交互的对象。由于它可以改变程序的运行行为，因此被认为是一种行为模式。

**编程示例**

这是聊天室（即中介者）与用户（即同事）相互发送消息的最简单示例。

首先，我们有中介者即聊天室

```cpp
#include <string>
#include <iostream>
#include <ctime>

class User; // Forward declaration

// Mediator interface
class ChatRoomMediator
{
public:
    virtual ~ChatRoomMediator() = default;
    virtual void showMessage(User* user, const std::string& message) = 0;
};

// Mediator implementation
class ChatRoom : public ChatRoomMediator
{
public:
    void showMessage(User* user, const std::string& message) override;
};
```

然后我们有我们的用户即同事
```cpp
class User
{
    std::string name;
    ChatRoomMediator* chatMediator;

public:
    User(const std::string& name, ChatRoomMediator* chatMediator)
        : name(name), chatMediator(chatMediator) {}

    std::string getName() const { return name; }

    void send(const std::string& message)
    {
        chatMediator->showMessage(this, message);
    }
};

// Implement ChatRoom::showMessage after User is fully defined
void ChatRoom::showMessage(User* user, const std::string& message)
{
    std::time_t now = std::time(nullptr);
    char timeStr[20];
    std::strftime(timeStr, sizeof(timeStr), "%b %d, %H:%M", std::localtime(&now));

    std::cout << timeStr << " [" << user->getName() << "]: " << message << std::endl;
}
```
以及用法
```cpp
ChatRoom mediator;

User john("John Doe", &mediator);
User jane("Jane Doe", &mediator);

john.send("Hi there!");
jane.send("Hey!");

// Output will be
// Feb 14, 10:58 [John Doe]: Hi there!
// Feb 14, 10:58 [Jane Doe]: Hey!
```

### 💾 备忘录（Memento）

现实世界的例子
> 以计算器（即发起者）为例，每当你执行某些计算时，最后一次计算都会保存在内存（即备忘录）中，以便你可以返回到它，并可能使用某些操作按钮（即管理者）恢复它。

通俗地说
> 备忘录模式是关于捕获和存储对象的当前状态，以便以后可以顺利恢复。

维基百科说
> 备忘录模式是一种软件设计模式，它提供了将对象恢复到其先前状态的能力（通过回滚撤销）。

通常在你需要提供某种撤销功能时很有用。

**编程示例**

让我们以文本编辑器为例，它不断保存状态，如果你愿意，可以恢复。

首先，我们有能够保存编辑器状态的备忘录对象

```cpp
#include <string>

class EditorMemento
{
    std::string content;

public:
    EditorMemento(const std::string& content) : content(content) {}
    std::string getContent() const { return content; }
};
```

然后我们有我们的编辑器即发起者，它将使用备忘录对象

```cpp
class Editor
{
    std::string content = "";

public:
    void type(const std::string& words)
    {
        content += " " + words;
    }

    std::string getContent() const
    {
        return content;
    }

    EditorMemento save()
    {
        return EditorMemento(content);
    }

    void restore(const EditorMemento& memento)
    {
        content = memento.getContent();
    }
};
```

然后可以这样使用

```cpp
Editor editor;

// Type some stuff
editor.type("This is the first sentence.");
editor.type("This is second.");

// Save the state to restore to : This is the first sentence. This is second.
EditorMemento saved = editor.save();

// Type some more
editor.type("And this is third.");

// Output: Content before Saving
std::cout << editor.getContent() << std::endl;
// This is the first sentence. This is second. And this is third.

// Restoring to last saved state
editor.restore(saved);

std::cout << editor.getContent() << std::endl;
// This is the first sentence. This is second.
```

### 😎 观察者（Observer）

现实世界的例子
> 一个很好的例子是求职者，他们订阅某个招聘网站，每当有匹配的工作机会时都会收到通知。

通俗地说
> 定义对象之间的依赖关系，以便每当对象更改其状态时，所有依赖对象都会收到通知。

维基百科说
> 观察者模式是一种软件设计模式，其中称为主题的对象维护其称为观察者的依赖列表，并通常通过调用其中一个方法来自动通知它们任何状态更改。

**编程示例**

翻译上面的示例。首先我们有需要收到职位发布通知的求职者
```cpp
#include <string>
#include <vector>
#include <iostream>

class JobPost
{
    std::string title;

public:
    JobPost(const std::string& title) : title(title) {}
    std::string getTitle() const { return title; }
};

class Observer
{
public:
    virtual ~Observer() = default;
    virtual void onJobPosted(const JobPost& job) = 0;
};

class Observable
{
public:
    virtual ~Observable() = default;
    virtual void attach(Observer* observer) = 0;
    virtual void notify(const JobPost& jobPosting) = 0;
};

class JobSeeker : public Observer
{
    std::string name;

public:
    JobSeeker(const std::string& name) : name(name) {}

    void onJobPosted(const JobPost& job) override
    {
        std::cout << "Hi " << name << "! New job posted: " << job.getTitle() << std::endl;
    }
};
```
然后我们有求职者将订阅的职位发布
```cpp
class EmploymentAgency : public Observable
{
    std::vector<Observer*> observers;

protected:
    void notify(const JobPost& jobPosting) override
    {
        for (auto* observer : observers) {
            observer->onJobPosted(jobPosting);
        }
    }

public:
    void attach(Observer* observer) override
    {
        observers.push_back(observer);
    }

    void addJob(const JobPost& jobPosting)
    {
        notify(jobPosting);
    }
};
```
然后可以这样使用
```cpp
// Create subscribers
JobSeeker johnDoe("John Doe");
JobSeeker janeDoe("Jane Doe");

// Create publisher and attach subscribers
EmploymentAgency jobPostings;
jobPostings.attach(&johnDoe);
jobPostings.attach(&janeDoe);

// Add a new job and see if subscribers get notified
jobPostings.addJob(JobPost("Software Engineer"));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

### 🏃 访问者（Visitor）

现实世界的例子
> 考虑有人访问迪拜。他们只需要一种方式（即签证）进入迪拜。到达后，他们可以自己参观迪拜的任何地方，无需请求许可或做任何准备工作即可参观这里的任何地方；只要告诉他们一个地方，他们就可以参观它。访问者模式让你做到这一点，它帮助你添加要参观的地方，以便他们可以尽可能多地参观，而无需做任何准备工作。

通俗地说
> 访问者模式允许你向对象添加更多操作，而无需修改它们。

维基百科说
> 在面向对象编程和软件工程中，访问者设计模式是一种将算法与其操作的对象结构分离的方式。这种分离的实际结果是能够向现有对象结构添加新操作，而无需修改这些结构。这是遵循开闭原则的一种方式。

**编程示例**

让我们以动物园模拟为例，我们有几种不同类型的动物，我们必须让它们发出声音。让我们使用访问者模式来翻译这个

```cpp
class Monkey;
class Lion;
class Dolphin;

// Visitor interface
class AnimalOperation
{
public:
    virtual ~AnimalOperation() = default;
    virtual void visitMonkey(Monkey* monkey) = 0;
    virtual void visitLion(Lion* lion) = 0;
    virtual void visitDolphin(Dolphin* dolphin) = 0;
};

// Visitee interface
class Animal
{
public:
    virtual ~Animal() = default;
    virtual void accept(AnimalOperation* operation) = 0;
};
```
然后我们有动物的实现
```cpp
class Monkey : public Animal
{
public:
    void shout() { std::cout << "Ooh oo aa aa!" << std::endl; }

    void accept(AnimalOperation* operation) override
    {
        operation->visitMonkey(this);
    }
};

class Lion : public Animal
{
public:
    void roar() { std::cout << "Roaaar!" << std::endl; }

    void accept(AnimalOperation* operation) override
    {
        operation->visitLion(this);
    }
};

class Dolphin : public Animal
{
public:
    void speak() { std::cout << "Tuut tuttu tuutt!" << std::endl; }

    void accept(AnimalOperation* operation) override
    {
        operation->visitDolphin(this);
    }
};
```
让我们实现我们的访问者
```cpp
class Speak : public AnimalOperation
{
public:
    void visitMonkey(Monkey* monkey) override { monkey->shout(); }
    void visitLion(Lion* lion) override { lion->roar(); }
    void visitDolphin(Dolphin* dolphin) override { dolphin->speak(); }
};
```

然后可以这样使用
```cpp
Monkey monkey;
Lion lion;
Dolphin dolphin;

Speak speak;

monkey.accept(&speak);    // Ooh oo aa aa!
lion.accept(&speak);      // Roaaar!
dolphin.accept(&speak);   // Tuut tutt tuutt!
```
我们本可以通过为动物创建继承层次结构来简单地做到这一点，但那样每当我们要向动物添加新操作时，都必须修改动物。但现在我们不必更改它们。例如，假设我们被要求向动物添加跳跃行为，我们只需创建一个新的访问者即可添加，即

```cpp
class Jump : public AnimalOperation
{
public:
    void visitMonkey(Monkey* monkey) override
    {
        std::cout << "Jumped 20 feet high! on to the tree!" << std::endl;
    }

    void visitLion(Lion* lion) override
    {
        std::cout << "Jumped 7 feet! Back on the ground!" << std::endl;
    }

    void visitDolphin(Dolphin* dolphin) override
    {
        std::cout << "Walked on water a little and disappeared" << std::endl;
    }
};
```
以及用法
```cpp
Jump jump;

monkey.accept(&speak);    // Ooh oo aa aa!
monkey.accept(&jump);     // Jumped 20 feet high! on to the tree!

lion.accept(&speak);      // Roaaar!
lion.accept(&jump);       // Jumped 7 feet! Back on the ground!

dolphin.accept(&speak);   // Tuut tutt tuutt!
dolphin.accept(&jump);    // Walked on water a little and disappeared
```

### 💡 策略（Strategy）

现实世界的例子
> 考虑排序的例子，我们实现了冒泡排序，但数据开始增长，冒泡排序开始变得非常慢。为了解决这个问题，我们实现了快速排序。但现在尽管快速排序算法对大型数据集表现更好，但对小型数据集来说非常慢。为了处理这个问题，我们实现了一种策略，即对于小型数据集使用冒泡排序，对于大型数据集使用快速排序。

通俗地说
> 策略模式允许你根据情况切换算法或策略。

维基百科说
> 在计算机编程中，策略模式（也称为政策模式）是一种行为软件设计模式，它允许在运行时选择算法的行为。

**编程示例**

翻译上面的示例。首先我们有策略接口和不同的策略实现

```cpp
#include <vector>
#include <iostream>

class SortStrategy
{
public:
    virtual ~SortStrategy() = default;
    virtual std::vector<int> sort(std::vector<int> dataset) = 0;
};

class BubbleSortStrategy : public SortStrategy
{
public:
    std::vector<int> sort(std::vector<int> dataset) override
    {
        std::cout << "Sorting using bubble sort" << std::endl;

        // Do sorting
        return dataset;
    }
};

class QuickSortStrategy : public SortStrategy
{
public:
    std::vector<int> sort(std::vector<int> dataset) override
    {
        std::cout << "Sorting using quick sort" << std::endl;

        // Do sorting
        return dataset;
    }
};
```

然后我们有将使用任何策略的客户端
```cpp
class Sorter
{
    SortStrategy* sorterSmall;
    SortStrategy* sorterBig;

public:
    Sorter(SortStrategy* sorterSmall, SortStrategy* sorterBig)
        : sorterSmall(sorterSmall), sorterBig(sorterBig) {}

    std::vector<int> sort(std::vector<int> dataset)
    {
        if (dataset.size() > 5) {
            return sorterBig->sort(dataset);
        } else {
            return sorterSmall->sort(dataset);
        }
    }
};
```
可以这样使用
```cpp
BubbleSortStrategy bubbleSort;
QuickSortStrategy quickSort;

Sorter sorter(&bubbleSort, &quickSort);

std::vector<int> smalldataset = {1, 3, 4, 2};
std::vector<int> bigdataset = {1, 4, 3, 2, 8, 10, 5, 6, 9, 7};

sorter.sort(smalldataset); // Output : Sorting using bubble sort
sorter.sort(bigdataset);   // Output : Sorting using quick sort
```

### 💢 状态（State）

现实世界的例子
> 想象你正在使用某个绘图应用程序，你选择画笔来绘制。现在画笔会根据所选颜色改变其行为，即如果你选择了红色，它将以红色绘制，如果选择了蓝色，则将以蓝色绘制，等等。

通俗地说
> 它允许你在状态更改时更改类的行为。

维基百科说
> 状态模式是一种行为软件设计模式，它以面向对象的方式实现状态机。通过状态模式，通过将每个单独的状态实现为状态模式接口的派生类，并通过调用模式超类定义的方法来实现状态转换，从而实现状态机。
> 状态模式可以解释为一种策略模式，它能够通过调用模式接口中定义的方法来切换当前策略。

**编程示例**

让我们以手机为例。首先我们有状态接口和一些状态实现

```cpp
#include <stdexcept>
#include <iostream>

class PhoneState
{
public:
    virtual ~PhoneState() = default;
    virtual PhoneState* pickUp() = 0;
    virtual PhoneState* hangUp() = 0;
    virtual PhoneState* dial() = 0;
};

class PhoneStateIdle : public PhoneState
{
public:
    PhoneState* pickUp() override { return new PhoneStatePickedUp(); }
    PhoneState* hangUp() override { throw std::runtime_error("already idle"); }
    PhoneState* dial() override { throw std::runtime_error("unable to dial in idle state"); }
};

class PhoneStatePickedUp : public PhoneState
{
public:
    PhoneState* pickUp() override { throw std::runtime_error("already picked up"); }
    PhoneState* hangUp() override { return new PhoneStateIdle(); }
    PhoneState* dial() override { return new PhoneStateCalling(); }
};

class PhoneStateCalling : public PhoneState
{
public:
    PhoneState* pickUp() override { throw std::runtime_error("already picked up"); }
    PhoneState* hangUp() override { return new PhoneStateIdle(); }
    PhoneState* dial() override { throw std::runtime_error("already dialing"); }
};
```

然后我们有Phone类，它在不同的行为调用时改变状态

```cpp
class Phone
{
    PhoneState* state;

public:
    Phone() : state(new PhoneStateIdle()) {}

    ~Phone() { delete state; }

    void pickUp()
    {
        PhoneState* newState = state->pickUp();
        delete state;
        state = newState;
    }

    void hangUp()
    {
        PhoneState* newState = state->hangUp();
        delete state;
        state = newState;
    }

    void dial()
    {
        PhoneState* newState = state->dial();
        delete state;
        state = newState;
    }
};
```

然后可以这样使用，它将调用相关的状态方法：

```cpp
Phone phone;

phone.pickUp();
phone.dial();
```

### 📒 模板方法（Template Method）

现实世界的例子
> 假设我们正在建造一栋房子。建造步骤可能如下：
> - 准备房屋地基
> - 建造墙壁
> - 添加屋顶
> - 添加其他楼层

> 这些步骤的顺序永远不能改变，即你不能在建造墙壁之前建造屋顶等，但每个步骤都可以修改，例如墙壁可以用木头、聚酯或石头制成。

通俗地说
> 模板方法定义了如何执行某个算法的骨架，但将这些步骤的实现推迟到子类。

维基百科说
> 在软件工程中，模板方法模式是一种行为设计模式，它定义了操作中算法的程序骨架，将某些步骤推迟到子类。它允许在不更改算法结构的情况下重新定义算法的某些步骤。

**编程示例**

想象我们有一个构建工具，帮助我们测试、检查、构建、生成构建报告（即代码覆盖率报告、检查报告等）并在测试服务器上部署我们的应用程序。

首先我们有指定构建算法骨架的基类
```cpp
class Builder
{
public:
    virtual ~Builder() = default;

    // Template method
    void build()
    {
        test();
        lint();
        assemble();
        deploy();
    }

    virtual void test() = 0;
    virtual void lint() = 0;
    virtual void assemble() = 0;
    virtual void deploy() = 0;
};
```

然后我们可以有我们的实现

```cpp
class AndroidBuilder : public Builder
{
public:
    void test() override { std::cout << "Running android tests" << std::endl; }
    void lint() override { std::cout << "Linting the android code" << std::endl; }
    void assemble() override { std::cout << "Assembling the android build" << std::endl; }
    void deploy() override { std::cout << "Deploying android build to server" << std::endl; }
};

class IosBuilder : public Builder
{
public:
    void test() override { std::cout << "Running ios tests" << std::endl; }
    void lint() override { std::cout << "Linting the ios code" << std::endl; }
    void assemble() override { std::cout << "Assembling the ios build" << std::endl; }
    void deploy() override { std::cout << "Deploying ios build to server" << std::endl; }
};
```
然后可以这样使用

```cpp
AndroidBuilder androidBuilder;
androidBuilder.build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

IosBuilder iosBuilder;
iosBuilder.build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 总结

以上就是全部内容。我会继续改进这个项目，所以你可能想要关注/收藏这个仓库以便重新访问。另外，我计划编写关于架构模式的类似内容，请保持关注。

## 👬 贡献

- 报告问题
- 提交改进建议的拉取请求
- 分享给更多人
- 通过 [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/kamrify.svg?style=social&label=Follow%20%40kamrify)](https://twitter.com/kamrify) 联系我提供任何反馈

## 许可证

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
