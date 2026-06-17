# 设计模式通俗指南

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

<sub>查看我的[其他项目](http://roadmap.sh)并在[Twitter](https://twitter.com/kamrify)上打个招呼。</sub>

<br>

|[创建型设计模式](#创建型设计模式)|[结构型设计模式](#结构型设计模式)|[行为型设计模式](#行为型设计模式)|
|:-|:-|:-|
|[简单工厂](#-简单工厂)|[适配器](#-适配器)|[责任链](#-责任链)|
|[工厂方法](#-工厂方法)|[桥接](#-桥接)|[命令](#-命令)|
|[抽象工厂](#-抽象工厂)|[组合](#-组合)|[迭代器](#-迭代器)|
|[建造者](#-建造者)|[装饰器](#-装饰器)|[中介者](#-中介者)|
|[原型](#-原型)|[外观](#-外观)|[备忘录](#-备忘录)|
|[单例](#-单例)|[享元](#-享元)|[观察者](#-观察者)|
||[代理](#-代理)|[访问者](#-访问者)|
|||[策略](#-策略)|
|||[状态](#-状态)|
|||[模板方法](#-模板方法)|

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

> 还要注意，下面的代码示例是用PHP-7编写的，但这不应该阻止你，因为概念是相同的。

设计模式类型
-----------------

* [创建型](#创建型设计模式)
* [结构型](#结构型设计模式)
* [行为型](#行为型设计模式)

## 创建型设计模式

通俗地说
> 创建型模式专注于如何实例化一个对象或一组相关对象。

维基百科说
> 在软件工程中，创建型设计模式是处理对象创建机制的设计模式，试图以适合情况的方式创建对象。对象创建的基本形式可能导致设计问题或增加设计的复杂性。创建型设计模式通过某种方式控制这种对象创建来解决这个问题。

 * [简单工厂](#-简单工厂)
 * [工厂方法](#-工厂方法)
 * [抽象工厂](#-抽象工厂)
 * [建造者](#-建造者)
 * [原型](#-原型)
 * [单例](#-单例)

### 🏠 简单工厂

现实世界的例子
> 假设你正在建造一栋房子，需要门。你可以穿上木匠的衣服，准备一些木材、胶水、钉子和所有制作门所需的工具，然后在你的房子里开始制作；或者你可以直接打电话给工厂，让他们把制作好的门送到你这里，这样你就不需要学习任何关于门制作的知识，也不需要处理制作过程中产生的混乱。

通俗地说
> 简单工厂只是为客户端生成一个实例，而不向客户端暴露任何实例化逻辑。

维基百科说
> 在面向对象编程（OOP）中，工厂是一个用于创建其他对象的对象——正式地说，工厂是一个函数或方法，它从某个方法调用返回不同原型或类的对象，这个方法调用被认为是"new"。

**编程示例**

首先，我们有一个门接口和它的实现
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```
然后我们有制作门的工厂
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```
然后可以这样使用
```php
// 制作一个100x200的门
$door = DoorFactory::makeDoor(100, 200);

echo '宽度: ' . $door->getWidth();
echo '高度: ' . $door->getHeight();

// 制作一个50x100的门
$door2 = DoorFactory::makeDoor(50, 100);
```

**何时使用？**

当创建对象不仅仅是几个赋值操作，而是涉及一些逻辑时，将其放在专门的工厂中是有意义的，而不是在各处重复相同的代码。

### 🏭 工厂方法

现实世界的例子
> 考虑招聘经理的情况。一个人不可能面试所有职位。根据职位空缺，她必须决定并将面试步骤委托给不同的人。

通俗地说
> 它提供了一种将实例化逻辑委托给子类的方法。

维基百科说
> 在基于类的编程中，工厂方法模式是一种创建型模式，它使用工厂方法来处理创建对象的问题，而无需指定将要创建的对象的确切类。这是通过调用工厂方法来创建对象来完成的——工厂方法可以在接口中指定并由子类实现，或者在基类中实现并可选地由派生类覆盖——而不是通过调用构造函数。

**编程示例**

以上面的招聘经理为例。首先，我们有一个面试官接口和一些实现

```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo '询问设计模式相关问题！';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo '询问社区建设相关问题';
    }
}
```

现在让我们创建我们的 `HiringManager`

```php
abstract class HiringManager
{

    // 工厂方法
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```
现在任何子类都可以扩展它并提供所需的面试官
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```
然后可以这样使用

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // 输出: 询问设计模式相关问题

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // 输出: 询问社区建设相关问题。
```

**何时使用？**

当类中有一些通用处理，但所需的子类在运行时动态决定时很有用。换句话说，当客户端不知道它可能需要什么确切的子类时。

### 🔨 抽象工厂

现实世界的例子
> 扩展我们简单工厂中的门示例。根据你的需求，你可能从木门店获得木门，从铁门店获得铁门，或者从相关商店获得PVC门。此外，你可能需要具有不同专业技能的人来安装门，例如木匠安装木门，焊工安装铁门等。正如你所看到的，门之间现在存在依赖关系，木门需要木匠，铁门需要焊工等。

通俗地说
> 工厂的工厂；一个将各个但相关/依赖的工厂分组在一起而不指定它们的具体类的工厂。

维基百科说
> 抽象工厂模式提供了一种封装一组具有共同主题的各个工厂的方法，而不指定它们的具体类。

**编程示例**

翻译上面的门示例。首先，我们有我们的 `Door` 接口和一些实现

```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇木门';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo '我是一扇铁门';
    }
}
```
然后我们有每种门类型的安装专家

```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安装铁门';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo '我只能安装木门';
    }
}
```

现在我们有抽象工厂，它允许我们创建相关对象族，即木门工厂将创建木门和木门安装专家，铁门工厂将创建铁门和铁门安装专家
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// 木门工厂返回木匠和木门
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// 铁门工厂获取铁门和相关安装专家
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```
然后可以这样使用
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // 输出: 我是一扇木门
$expert->getDescription(); // 输出: 我只能安装木门

// 铁门工厂相同
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // 输出: 我是一扇铁门
$expert->getDescription(); // 输出: 我只能安装铁门
```

正如你所看到的，木门工厂封装了 `木匠` 和 `木门`，铁门工厂也封装了 `铁门` 和 `焊工`。这有助于我们确保对于每扇创建的门，我们不会得到错误的安装专家。

**何时使用？**

当存在相互关联的依赖关系，且涉及不太简单的创建逻辑时。

### 👷 建造者

现实世界的例子
> 想象你在肯德基，你点了一个特定的套餐，比如"大份肯德基"，他们没有任何疑问就递给你；这是简单工厂的例子。但有些情况下创建逻辑可能涉及更多步骤。例如，你想要一个定制的赛百味套餐，你有几个关于汉堡制作方式的选项，比如你想要什么面包？你想要什么类型的酱料？你想要什么奶酪？在这种情况下，建造者模式就派上用场了。

通俗地说
> 允许你创建对象的不同变体，同时避免构造函数污染。当对象可能有多种变体时很有用。或者当对象的创建涉及很多步骤时。

维基百科说
> 建造者模式是一种对象创建软件设计模式，旨在找到解决可伸缩构造函数反模式的方案。

说到这里，让我补充一下什么是可伸缩构造函数反模式。在某个时候，我们都见过这样的构造函数：

```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

正如你所看到的，构造函数参数的数量可能很快失控，理解参数的排列可能变得困难。此外，如果你想在未来添加更多选项，这个参数列表可能会继续增长。这被称为可伸缩构造函数反模式。

**编程示例**

明智的替代方案是使用建造者模式。首先，我们有我们想要制作的汉堡

```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

然后我们有建造者

```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```
然后可以这样使用：

```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**何时使用？**

当对象可能有多种变体时，以及为了避免构造函数可伸缩。与工厂模式的关键区别是：工厂模式用于创建是单步过程的情况，而建造者模式用于创建是多步过程的情况。

### 🐑 原型

现实世界的例子
> 还记得多莉吗？那只被克隆的羊！让我们不要深入细节，但关键是这都是关于克隆的。

通俗地说
> 通过克隆基于现有对象创建对象。

维基百科说
> 原型模式是软件开发中的一种创建型设计模式。当要创建的对象类型由原型实例确定时使用它，该原型实例被克隆以产生新对象。

简而言之，它允许你创建现有对象的副本并根据需要修改它，而不是从头创建对象并设置它的麻烦。

**编程示例**

在PHP中，可以使用 `clone` 轻松完成

```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Mountain Sheep')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```
然后可以像下面这样克隆
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Mountain Sheep

// 克隆并修改所需内容
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Mountain sheep
```

你也可以使用魔术方法 `__clone` 来修改克隆行为。

**何时使用？**

当需要类似于现有对象的对象时，或者当创建比克隆更昂贵时。

### 💍 单例

现实世界的例子
> 一个国家一次只能有一个总统。每当职责召唤时，都必须让同一个总统行动起来。这里的总统是单例。

通俗地说
> 确保特定类只有一个对象被创建。

维基百科说
> 在软件工程中，单例模式是一种将类的实例化限制为一个对象的软件设计模式。当只需要一个对象来协调系统中的操作时很有用。

单例模式实际上被认为是一种反模式，应避免过度使用。它不一定坏，可能有一些有效的用例，但应谨慎使用，因为它在应用程序中引入了全局状态，在一个地方的更改可能会影响其他区域，并且可能变得相当难以调试。另一个缺点是它使代码紧密耦合，加上模拟单例可能很困难。

**编程示例**

要创建单例，请将构造函数设为私有，禁用克隆，禁用扩展，并创建一个静态变量来容纳实例
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // 隐藏构造函数
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // 禁用克隆
    }

    private function __wakeup()
    {
        // 禁用反序列化
    }
}
```
然后为了使用
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

## 结构型设计模式

通俗地说
> 结构型模式主要关注对象组合，或者换句话说，实体如何相互使用。另一种解释是，它们有助于回答"如何构建软件组件？"

维基百科说
> 在软件工程中，结构型设计模式是通过识别实现关系之间的简单方法来简化设计的设计模式。

 * [适配器](#-适配器)
 * [桥接](#-桥接)
 * [组合](#-组合)
 * [装饰器](#-装饰器)
 * [外观](#-外观)
 * [享元](#-享元)
 * [代理](#-代理)

### 🔌 适配器

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

```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```
猎人期望任何 `Lion` 接口的实现来猎杀。
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

现在假设我们必须在游戏中添加一个 `WildDog`，以便猎人也可以猎杀它。但我们不能直接这样做，因为狗有不同的接口。为了使它对我们的猎人兼容，我们必须创建一个兼容的适配器

```php
// 这需要添加到游戏中
class WildDog
{
    public function bark()
    {
    }
}

// 围绕野狗的适配器，使其与我们的游戏兼容
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```
现在 `WildDog` 可以通过 `WildDogAdapter` 在我们的游戏中使用。

```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

### 🚡 桥接

现实世界的例子
> 假设你有一个包含不同页面的网站，你应该允许用户更改主题。你会怎么做？为每个主题创建每个页面的多个副本，还是只创建单独的主题并根据用户偏好加载它们？桥接模式允许你做后者，即：

![有无桥接模式的对比](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

通俗地说
> 桥接模式是关于优先使用组合而不是继承。实现细节从一个层次结构推送到另一个具有单独层次结构的对象。

维基百科说
> 桥接模式是软件工程中使用的一种设计模式，旨在"将抽象与其实现解耦，以便两者可以独立变化"。

**编程示例**

翻译上面的网页示例。这里我们有 `WebPage` 层次结构

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "关于页面使用 " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "职业页面使用 " . $this->theme->getColor();
    }
}
```
以及单独的主题层次结构
```php
interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return '深黑色';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return '米白色';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return '浅蓝色';
    }
}
```
以及两个层次结构的使用
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "关于页面使用深黑色";
echo $careers->getContent(); // "职业页面使用深黑色";
```

### 🌿 组合

现实世界的例子
> 每个组织都由员工组成。每个员工都有相同的特征，即有工资、有一些职责、可能向某人汇报，也可能有一些下属等。

通俗地说
> 组合模式允许客户端以统一的方式处理单个对象。

维基百科说
> 在软件工程中，组合模式是一种分区设计模式。组合模式描述了一组对象应该以与单个对象实例相同的方式对待。组合的意图是将对象"组合"成树结构以表示部分-整体层次结构。实现组合模式允许客户端统一处理单个对象和组合。

**编程示例**

以上面的员工示例为例。这里我们有不同的员工类型

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

然后我们有一个由几种不同类型的员工组成的组织

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

然后可以这样使用

```php
// 准备员工
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// 将他们添加到组织
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "净工资: " . $organization->getNetSalaries(); // 净工资: 27000
```

### ☕ 装饰器

现实世界的例子

> 想象你经营一家汽车维修店，提供多种服务。现在你如何计算要收取的账单？你选择一项服务，并动态地不断添加所提供服务的价格，直到获得最终成本。这里每种类型的服务都是一个装饰器。

通俗地说
> 装饰器模式允许你通过将对象包装在装饰器类的对象中，在运行时动态更改对象的行为。

维基百科说
> 在面向对象编程中，装饰器模式是一种设计模式，它允许向单个对象添加行为，无论是静态还是动态，而不影响同一类中其他对象的行为。装饰器模式通常有助于遵守单一职责原则，因为它允许将功能划分为具有独特关注领域的类。

**编程示例**

以咖啡为例。首先我们有一个实现咖啡接口的简单咖啡

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return '简单咖啡';
    }
}
```
我们希望使代码可扩展，以允许在需要时修改它。让我们制作一些附加组件（装饰器）
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . '，牛奶';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . '，奶油';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . '，香草';
    }
}
```

现在让我们制作一杯咖啡

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // 简单咖啡

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // 简单咖啡，牛奶

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // 简单咖啡，牛奶，奶油

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // 简单咖啡，牛奶，奶油，香草
```

### 📦 外观

现实世界的例子
> 你如何打开电脑？"按下电源按钮"你会说！这就是你所相信的，因为你正在使用电脑外部提供的简单接口，内部它必须做很多事情才能实现。这种复杂子系统的简单接口就是外观。

通俗地说
> 外观模式为复杂子系统提供简化的接口。

维基百科说
> 外观是一个为更大的代码体（如类库）提供简化接口的对象。

**编程示例**

以上面的电脑示例为例。这里我们有电脑类

```php
class Computer
{
    public function getElectricShock()
    {
        echo "哎哟！";
    }

    public function makeSound()
    {
        echo "哔哔！";
    }

    public function showLoadingScreen()
    {
        echo "加载中..";
    }

    public function bam()
    {
        echo "准备就绪！";
    }

    public function closeEverything()
    {
        echo "嗡嗡嗡！";
    }

    public function sooth()
    {
        echo "滋滋滋";
    }

    public function pullCurrent()
    {
        echo "哈啊！";
    }
}
```
这里我们有外观
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
现在使用外观
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // 哎哟！哔哔！加载中.. 准备就绪！
$computer->turnOff(); // 嗡嗡嗡！哈啊！滋滋滋
```

### 🍃 享元

现实世界的例子
> 你曾经在某个摊位喝过新鲜的茶吗？他们通常会制作超过你需求的一杯，并为其他顾客节省剩余的资源，例如燃气等。享元模式就是关于共享的。

通俗地说
> 它用于通过与类似对象共享尽可能多的数据来最小化内存使用或计算费用。

维基百科说
> 在计算机编程中，享元是一种软件设计模式。享元是一个通过与其他类似对象共享尽可能多的数据来最小化内存使用的对象；这是一种在简单重复表示将使用不可接受的内存量时大量使用对象的方法。

**编程示例**

翻译上面的茶示例。首先我们有茶类型和茶制作者

```php
// 任何将被缓存的都是享元。
// 这里的茶类型将是享元。
class KarakTea
{
}

// 充当工厂并保存茶
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

然后我们有 `TeaShop`，它接受订单并提供服务

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "为第 " . $table . " 桌上茶";
        }
    }
}
```
可以这样使用

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('少糖', 1);
$shop->takeOrder('多奶', 2);
$shop->takeOrder('无糖', 5);

$shop->serve();
// 为第 1 桌上茶
// 为第 2 桌上茶
// 为第 5 桌上茶
```

### 🎱 代理

现实世界的例子
> 你曾经使用门禁卡通过门吗？打开那扇门有多种选择，即可以使用门禁卡打开，也可以按下绕过安全系统的按钮。门的主要功能是打开，但在其上添加了代理以添加一些功能。让我用下面的代码示例更好地解释它。

通俗地说
> 使用代理模式，一个类代表另一个类的功能。

维基百科说
> 代理，以其最一般的形式，是一个充当其他事物接口的类。代理是一个包装器或代理对象，客户端调用它以访问幕后的真实服务对象。代理的使用可以简单地转发到真实对象，或者可以提供额外的逻辑。在代理中可以提供额外的功能，例如当对真实对象的操作资源密集时进行缓存，或在调用对真实对象的操作之前检查先决条件。

**编程示例**

以上面的安全门示例为例。首先我们有门接口和门的实现

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "打开实验室门";
    }

    public function close()
    {
        echo "关闭实验室门";
    }
}
```
然后我们有一个代理来保护我们想要的任何门
```php
class SecuredDoor implements Door
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "不行！这不可能。";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
可以这样使用
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // 不行！这不可能。

$door->open('$ecr@t'); // 打开实验室门
$door->close(); // 关闭实验室门
```
另一个例子是某种数据映射器实现。例如，我最近使用这种模式为MongoDB制作了一个ODM（对象数据映射器），我在其中围绕mongo类编写了一个代理，同时利用魔术方法 `__call()`。所有方法调用都被代理到原始mongo类，检索到的结果按原样返回，但在 `find` 或 `findOne` 的情况下，数据被映射到所需的类对象，并且返回对象而不是 `Cursor`。

## 行为型设计模式

通俗地说
> 它关注对象之间的责任分配。与结构型模式的不同之处在于，它们不仅指定结构，还概述了它们之间消息传递/通信的模式。换句话说，它们有助于回答"如何在软件组件中运行行为？"

维基百科说
> 在软件工程中，行为型设计模式是识别对象之间常见通信模式并实现这些模式的设计模式。通过这样做，这些模式增加了执行此通信的灵活性。

* [责任链](#-责任链)
* [命令](#-命令)
* [迭代器](#-迭代器)
* [中介者](#-中介者)
* [备忘录](#-备忘录)
* [观察者](#-观察者)
* [访问者](#-访问者)
* [策略](#-策略)
* [状态](#-状态)
* [模板方法](#-模板方法)

### 🔗 责任链

现实世界的例子
> 例如，你在账户中设置了三种支付方式（`A`、`B` 和 `C`）；每种都有不同的金额。`A` 有100美元，`B` 有300美元，`C` 有1000美元，支付偏好选择为 `A` 然后 `B` 然后 `C`。你尝试购买价值210美元的东西。使用责任链，首先检查账户 `A` 是否可以进行购买，如果是，则进行购买并且链将被打破。如果不是，请求将转发到账户 `B` 检查金额，如果是，则链将被打破，否则请求将继续转发，直到找到合适的处理程序。这里 `A`、`B` 和 `C` 是链的链接，整个现象就是责任链。

通俗地说
> 它帮助构建对象链。请求从一端进入，并不断从对象传递到对象，直到找到合适的处理程序。

维基百科说
> 在面向对象设计中，责任链模式是一种由命令对象源和一系列处理对象组成的设计模式。每个处理对象都包含定义它可以处理的命令对象类型的逻辑；其余的被传递到链中的下一个处理对象。

**编程示例**

翻译上面的账户示例。首先我们有一个基础账户，包含将账户链接在一起的逻辑和一些账户

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('使用 %s 支付了 %s' . PHP_EOL, get_called_class(), $amountToPay);
        } elseif ($this->successor) {
            echo sprintf('无法使用 %s 支付。继续..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('没有账户有足够的余额');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

现在让我们使用上面定义的链接（即银行、Paypal、比特币）准备链

```php
// 让我们准备一个像下面这样的链
//      $bank->$paypal->$bitcoin
//
// 第一优先级银行
//      如果银行无法支付，则使用Paypal
//      如果Paypal无法支付，则使用比特币

$bank = new Bank(100);          // 余额为100的银行
$paypal = new Paypal(200);      // 余额为200的Paypal
$bitcoin = new Bitcoin(300);    // 余额为300的比特币

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// 让我们尝试使用第一优先级即银行支付
$bank->pay(259);

// 输出将是
// ==============
// 无法使用 Bank 支付。继续..
// 无法使用 Paypal 支付。继续..
// 使用 Bitcoin 支付了 259
```

### 👮 命令

现实世界的例子
> 一个通用的例子是你在餐厅点餐。你（即 `客户端`）要求服务员（即 `调用者`）带一些食物（即 `命令`），服务员只是将请求转发给厨师（即 `接收者`），他知道做什么以及如何烹饪。
> 另一个例子是你（即 `客户端`）使用遥控器（`调用者`）打开（即 `命令`）电视（即 `接收者`）。

通俗地说
> 允许你将操作封装在对象中。此模式背后的关键思想是提供将客户端与接收者解耦的方法。

维基百科说
> 在面向对象编程中，命令模式是一种行为设计模式，其中对象用于封装执行操作或在以后触发事件所需的所有信息。此信息包括方法名称、拥有该方法的对象以及方法参数的值。

**编程示例**

首先我们有接收者，它具有可以执行的每个操作的实现
```php
// 接收者
class Bulb
{
    public function turnOn()
    {
        echo "灯泡已点亮";
    }

    public function turnOff()
    {
        echo "一片黑暗！";
    }
}
```
然后我们有一个接口，每个命令都将实现它，然后我们有一组命令
```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// 命令
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```
然后我们有一个 `调用者`，客户端将与之交互以处理任何命令
```php
// 调用者
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```
最后让我们看看如何在客户端中使用它
```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // 灯泡已点亮！
$remote->submit($turnOff); // 一片黑暗！
```

命令模式也可用于实现基于事务的系统。你在执行命令时立即维护命令历史记录。如果最终命令成功执行，则一切正常，否则只需遍历历史记录并对所有已执行的命令执行 `undo`。

### ➿ 迭代器

现实世界的例子
> 旧收音机将是迭代器的一个很好的例子，用户可以从某个频道开始，然后使用下一个或上一个按钮浏览相应的频道。或者以MP3播放器或电视机为例，你可以按下下一个和上一个按钮浏览连续的频道，或者换句话说，它们都提供了一个接口来迭代相应的频道、歌曲或电台。

通俗地说
> 它提供了一种访问对象元素的方式，而不暴露底层表示。

维基百科说
> 在面向对象编程中，迭代器是一种设计模式，其中迭代器用于遍历容器并访问容器的元素。迭代器模式将算法与容器解耦；在某些情况下，算法特定于容器，因此无法解耦。

**编程示例**

在PHP中，使用SPL（标准PHP库）很容易实现。翻译上面的电台示例。首先我们有 `RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```
然后我们有我们的迭代器

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```
然后可以这样使用
```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // 将移除89电台
```

### 👽 中介者

现实世界的例子
> 一个通用的例子是当你通过手机与某人交谈时，有一个网络提供商坐在你们之间，你们的对话通过它而不是直接发送。在这种情况下，网络提供商是中介者。

通俗地说
> 中介者模式添加了一个第三方对象（称为中介者）来控制两个对象（称为同事）之间的交互。它有助于减少相互通信的类之间的耦合。因为现在它们不需要了解彼此的实现。

维基百科说
> 在软件工程中，中介者模式定义了一个封装一组对象如何交互的对象。由于它可以改变程序的运行行为，因此被认为是一种行为模式。

**编程示例**

这是聊天室（即中介者）与用户（即同事）相互发送消息的最简单示例。

首先，我们有中介者即聊天室

```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// 中介者
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

然后我们有我们的用户即同事
```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```
以及用法
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('你好！');
$jane->send('嘿！');

// 输出将是
// Feb 14, 10:58 [John]: 你好！
// Feb 14, 10:58 [Jane]: 嘿！
```

### 💾 备忘录

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

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

然后我们有我们的编辑器即发起者，它将使用备忘录对象

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

然后可以这样使用

```php
$editor = new Editor();

// 输入一些内容
$editor->type('这是第一句话。');
$editor->type('这是第二句。');

// 保存状态以恢复到：这是第一句话。这是第二句。
$saved = $editor->save();

// 再输入一些
$editor->type('这是第三句。');

// 输出：保存前的内容
echo $editor->getContent(); // 这是第一句话。这是第二句。这是第三句。

// 恢复到上次保存的状态
$editor->restore($saved);

$editor->getContent(); // 这是第一句话。这是第二句。
```

### 😎 观察者

现实世界的例子
> 一个很好的例子是求职者，他们订阅某个招聘网站，每当有匹配的工作机会时都会收到通知。

通俗地说
> 定义对象之间的依赖关系，以便每当对象更改其状态时，所有依赖对象都会收到通知。

维基百科说
> 观察者模式是一种软件设计模式，其中称为主题的对象维护其称为观察者的依赖列表，并通常通过调用其中一个方法来自动通知它们任何状态更改。

**编程示例**

翻译上面的示例。首先我们有需要收到职位发布通知的求职者
```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

interface Observer
{
    public function onJobPosted(JobPost $job);
}

interface Observable
{
    public function attach(Observer $observer);
    public function notify(JobPost $jobPosting);
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // 对职位发布做一些处理
        echo '嗨 ' . $this->name . '！新职位发布：'. $job->getTitle();
    }
}
```
然后我们有求职者将订阅的职位发布
```php
class EmploymentAgency implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```
然后可以这样使用
```php
// 创建订阅者
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// 创建发布者并附加订阅者
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// 添加新职位并查看订阅者是否收到通知
$jobPostings->addJob(new JobPost('软件工程师'));

// 输出
// 嗨 John Doe！新职位发布：软件工程师
// 嗨 Jane Doe！新职位发布：软件工程师
```

### 🏃 访问者

现实世界的例子
> 考虑有人访问迪拜。他们只需要一种方式（即签证）进入迪拜。到达后，他们可以自己参观迪拜的任何地方，无需请求许可或做任何准备工作即可参观这里的任何地方；只要告诉他们一个地方，他们就可以参观它。访问者模式让你做到这一点，它帮助你添加要参观的地方，以便他们可以尽可能多地参观，而无需做任何准备工作。

通俗地说
> 访问者模式允许你向对象添加更多操作，而无需修改它们。

维基百科说
> 在面向对象编程和软件工程中，访问者设计模式是一种将算法与其操作的对象结构分离的方式。这种分离的实际结果是能够向现有对象结构添加新操作，而无需修改这些结构。这是遵循开闭原则的一种方式。

**编程示例**

让我们以动物园模拟为例，我们有几种不同类型的动物，我们必须让它们发出声音。让我们使用访问者模式来翻译这个

```php
// 被访问者
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// 访问者
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
然后我们有动物的实现
```php
class Monkey implements Animal
{
    public function shout()
    {
        echo '喔喔喔！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo '吼吼吼！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo '嘟嘟嘟！';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```
让我们实现我们的访问者
```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

然后可以这样使用
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // 喔喔喔！    
$lion->accept($speak);      // 吼吼吼！
$dolphin->accept($speak);   // 嘟嘟嘟！
```
我们本可以通过为动物创建继承层次结构来简单地做到这一点，但那样每当我们要向动物添加新操作时，都必须修改动物。但现在我们不必更改它们。例如，假设我们被要求向动物添加跳跃行为，我们只需创建一个新的访问者即可添加，即

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo '跳了20英尺高！跳到树上！';
    }

    public function visitLion(Lion $lion)
    {
        echo '跳了7英尺！回到地面！';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo '在水上走了一会儿就消失了';
    }
}
```
以及用法
```php
$jump = new Jump();

$monkey->accept($speak);   // 喔喔喔！
$monkey->accept($jump);    // 跳了20英尺高！跳到树上！

$lion->accept($speak);     // 吼吼吼！
$lion->accept($jump);      // 跳了7英尺！回到地面！

$dolphin->accept($speak);  // 嘟嘟嘟！
$dolphin->accept($jump);   // 在水上走了一会儿就消失了
```

### 💡 策略

现实世界的例子
> 考虑排序的例子，我们实现了冒泡排序，但数据开始增长，冒泡排序开始变得非常慢。为了解决这个问题，我们实现了快速排序。但现在尽管快速排序算法对大型数据集表现更好，但对小型数据集来说非常慢。为了处理这个问题，我们实现了一种策略，即对于小型数据集使用冒泡排序，对于大型数据集使用快速排序。

通俗地说
> 策略模式允许你根据情况切换算法或策略。

维基百科说
> 在计算机编程中，策略模式（也称为政策模式）是一种行为软件设计模式，它允许在运行时选择算法的行为。

**编程示例**

翻译上面的示例。首先我们有策略接口和不同的策略实现

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用冒泡排序";

        // 执行排序
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "使用快速排序";

        // 执行排序
        return $dataset;
    }
}
```

然后我们有将使用任何策略的客户端
```php
class Sorter
{
    protected $sorterSmall;
    protected $sorterBig;

    public function __construct(SortStrategy $sorterSmall, SortStrategy $sorterBig)
    {
        $this->sorterSmall = $sorterSmall;
        $this->sorterBig = $sorterBig;
    }

    public function sort(array $dataset): array
    {
        if (count($dataset) > 5) {
            return $this->sorterBig->sort($dataset);
        } else {
            return $this->sorterSmall->sort($dataset);
        }
    }
}
```
可以这样使用
```php
$smalldataset = [1, 3, 4, 2];
$bigdataset = [1, 4, 3, 2, 8, 10, 5, 6, 9, 7];

$sorter = new Sorter(new BubbleSortStrategy(), new QuickSortStrategy());

$sorter->sort($smalldataset); // 输出：使用冒泡排序

$sorter->sort($bigdataset); // 输出：使用快速排序
```

### 💢 状态

现实世界的例子
> 想象你正在使用某个绘图应用程序，你选择画笔来绘制。现在画笔会根据所选颜色改变其行为，即如果你选择了红色，它将以红色绘制，如果选择了蓝色，则将以蓝色绘制，等等。

通俗地说
> 它允许你在状态更改时更改类的行为。

维基百科说
> 状态模式是一种行为软件设计模式，它以面向对象的方式实现状态机。通过状态模式，通过将每个单独的状态实现为状态模式接口的派生类，并通过调用模式超类定义的方法来实现状态转换，从而实现状态机。
> 状态模式可以解释为一种策略模式，它能够通过调用模式接口中定义的方法来切换当前策略。

**编程示例**

让我们以手机为例。首先我们有状态接口和一些状态实现

```php
interface PhoneState {
    public function pickUp(): PhoneState;
    public function hangUp(): PhoneState;
    public function dial(): PhoneState;
}

// 状态实现
class PhoneStateIdle implements PhoneState {
    public function pickUp(): PhoneState {
        return new PhoneStatePickedUp();
    }
    public function hangUp(): PhoneState {
        throw new Exception("已经是空闲状态");
    }
    public function dial(): PhoneState {
        throw new Exception("无法在空闲状态下拨号");
    }
}

class PhoneStatePickedUp implements PhoneState {
    public function pickUp(): PhoneState {
        throw new Exception("已经拿起");
    }
    public function hangUp(): PhoneState {
        return new PhoneStateIdle();
    }
    public function dial(): PhoneState {
        return new PhoneStateCalling();
    }
}

class PhoneStateCalling implements PhoneState {
    public function pickUp(): PhoneState {
        throw new Exception("已经拿起");
    }
    public function hangUp(): PhoneState {
        return new PhoneStateIdle();
    }
    public function dial(): PhoneState {
        throw new Exception("正在拨号中");
    }
}
```

然后我们有Phone类，它在不同的行为调用时改变状态

```php
class Phone {
    private $state;

    public function __construct() {
        $this->state = new PhoneStateIdle();
    }
    public function pickUp() {
        $this->state = $this->state->pickUp();
    }
    public function hangUp() {
        $this->state = $this->state->hangUp();
    }
    public function dial() {
        $this->state = $this->state->dial();
    }
}
```

然后可以这样使用，它将调用相关的状态方法：

```php
$phone = new Phone();

$phone->pickUp();
$phone->dial();
```

### 📒 模板方法

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
```php
abstract class Builder
{

    // 模板方法
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

然后我们可以有我们的实现

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo '运行Android测试';
    }

    public function lint()
    {
        echo '检查Android代码';
    }

    public function assemble()
    {
        echo '组装Android构建';
    }

    public function deploy()
    {
        echo '部署Android构建到服务器';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo '运行iOS测试';
    }

    public function lint()
    {
        echo '检查iOS代码';
    }

    public function assemble()
    {
        echo '组装iOS构建';
    }

    public function deploy()
    {
        echo '部署iOS构建到服务器';
    }
}
```
然后可以这样使用

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// 输出：
// 运行Android测试
// 检查Android代码
// 组装Android构建
// 部署Android构建到服务器

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// 输出：
// 运行iOS测试
// 检查iOS代码
// 组装iOS构建
// 部署iOS构建到服务器
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
