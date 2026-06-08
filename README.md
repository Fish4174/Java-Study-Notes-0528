# Java-Study-Notes-0528
5月28日Java学习笔记
# 第6章实践 面向对象的三大特征

## 【实践题目与要求】

运用封装、继承、多态的知识编写 Java 程序，模拟电子宠物乐园。 

1.假设电子宠物乐园中有很多宠物，目前有宠物狗和海豚：

宠物狗的属性有昵称name、健康值health、亲密度love和品种strain。 

海豚的属性有昵称name、健康值health、亲密度love和性别sex。 

利用继承知识定义合适类，能够描述宠物狗和海豚的私有属性和对它们操作的必要的方法，如构造方法、访问方法、设置方法和输出信息方法，同时减少代码冗余，提高代码的可重用性和可扩展性，以便将来能够快速扩充功能，方便为宠物乐园添加更多新的宠物，例如猫、兔子、海龟等等。

2.为宠物添加一个表示编号的属性index，程序能自动为创建的某个特定宠物设置编号，第一个宠物编号是1，第二个宠物编号是2，...。 

3.编写宠物乐园类PetPark，定义main方法，创建宠物对象，宠物的数量自拟，但至少有两个宠物对象，一只是宠物狗，另一只是宠物海豚，要求用多态来实现。

4.定义主人类Master，有私有属性名字name，定义必要的方法。在PetPark的main方法中创建两个主人对象，名字自拟。

5.在类Master中定义adopt()方法，实现领养一只宠物功能，确保主人可以领养不同类型的宠物。 在类PetPark的main方法中模拟该功能。

6.定义feed()方法实现主人给宠物喂食功能。确保主人可以喂养不同类型的宠物，注意：不同宠物吃的东西不同，狗吃骨头，吃完东西后，增加3个健康值；企鹅吃鱼肉，吃完东西后，增加5个健康值。在类PetPark的main方法中模拟该功能。

7.定义playWith()方法实现主人和宠物玩耍功能。 主人与不同宠物玩耍活动不同：

主人和狗狗玩接飞盘游戏catchingFlyDisk，狗狗健康值减少5，与主人亲密度增加3；

主人和企鹅玩游泳游戏swimming，企鹅健康值减少4，与主人亲密度增加5。

在PetPark的main方法中模拟该功能。

## 【实现步骤描述】

1.为了实现代码的可扩展性和可重用性，提炼两类宠物的共性特征，定义父类 Pet，然后运用继承知识定义狗类 Dog 和海豚类 Penguin。 设计类图如下：

![](https://img2024.cnblogs.com/blog/2979549/202606/2979549-20260608213129847-1944716668.png)

2.在父类Pet中定义一个static的成员变量count，初值是0，用于保存对象个数，再定义一个实例成员变量index，存编号；

然后在所有的Pet构造方法中都先让count值加1，再将count赋值给index；

并在Pet的print方法中输出index值；

3.定义宠物乐园类PetPark，定义main方法，用子类的构造方法创建以父之名声明的对象，用强制类型转换调用独属于子类的set方法；

4.定义主人类Master，定义私有类成员变量name，并设置set和get方法；

在PetPark的main方法中实例化两个主人对象；

5.在Master类中定义adopt方法，参数为父类Pet的对象，并在该方法中输出主人与宠物的关系；

在PetPark的main方法中调用每个主人对象的adopt方法，参数为宠物名；

6.在Master类中定义feed方法，参数为父类Pet的对象，并在该方法中调用Pet对象的eat方法；

在父类Pet中定义抽象方法eat，并在Dog类和Penguin类中分别重写不同的eat方法，Dog类中的eat使健康值加3，输出吃骨头，Penguin类中的eat使健康值加5，输出吃鱼；

在PetPark的main方法中调用每个主人对象的feed方法，参数为宠物名；

7.在Master类中定义playWith方法，参数为父类Pet的对象，在该方法中用instanceof关键字，判断参数中的对象是Dog类还是Penguin类，并依次调用不同的方法；

如果属于Dog类，则用强制类型转换调用catchingFlyDisk方法，参数为Master类的对象this，在Dog类中实现该方法，输出行为和更新后的信息；

如果属于Penguin类，则用强制类型转换调用swimming方法，参数为Master类的对象this，在Penguin类中实现该方法，输出行为和更新后的信息；

在PetPark的main方法中调用每个主人对象的playWith方法，参数为宠物名；

## 【程序代码】

**Pet.java**

```java
package PetPark;

public abstract class Pet
{
    private int index;//编号
    private static int count = 0;//记录宠物个数
    private String name;
    private int health;
    private int love;
    public Pet()
    {
        index = ++count;
        name = "";
        health = 60;
        love = 60;
    }
    public Pet(String name)
    {
        index = ++count;
        this.name = name;
        health = 60;
        love = 60;
    }
    public Pet(String name, int health, int love)
    {
        index = ++count;
        this.name = name;
        this.health = health;
        this.love = love;
    }
    public String getName()
    {
        return name;
    }
    public void setName(String name)
    {
        this.name = name;
    }
    public int getHealth()
    {
        return health;
    }
    public void setHealth(int health)
    {
        this.health = health;
    }
    public int getLove()
    {
        return love;
    }
    public void setLove(int love)
    {
        this.love = love;
    }

    public int getIndex() {
        return index;
    }

    public void setIndex(int index) {
        this.index = index;
    }

    public void print()
    {
        System.out.println("编号：" + index);
        System.out.println("昵称：" + name);
        System.out.println("健康值：" + health);
        System.out.println("亲密度：" + love);
    }
    //设置抽象方法：吃饭方法，没有方法体，方法用abstract声明
    //抽象方法必须在子类中实现
    public abstract void eat();
}
```

**Dog.java**

```java
package PetPark;

public class Dog extends Pet
{
    private String strain;//品种
    public Dog()
    {
        super();
        strain = "";
    }
    public Dog(String name)
    {
        super(name);
        strain = "";
    }
    public Dog(String name, int health, int love, String strain)
    {
        super(name, health, love);
        this.strain = strain;
    }
    public String getStrain()
    {
        return strain;
    }
    public void setStrain(String strain)
    {
        this.strain = strain;
    }
    @Override
    public void print()
    {
        System.out.println("-----宠物狗的自白-----");
        super.print();
        System.out.println("品种：" + strain);
        System.out.println("--------------------");
    }

    @Override
    public void eat()
    {
        setHealth(getHealth()+3);
        System.out.println("小狗" + getName() + "正在吃骨头……它的健康值增长到" + getHealth());
    }

    public void catchingFlyDisk(Master master)
    {
        System.out.println("主人" + master.getName() + "正在和编号是" + getIndex() + "的宠物狗" + getName() + "玩接飞盘游戏...");
        setHealth(getHealth()-5);
        setLove(getLove()+3);
        System.out.println("宠物狗" + getName() + "的健康值减少到" + getHealth() + "，亲密度增加到" + getLove());
    }
}
```

**Penguin.java**

```java
package PetPark;

public class Penguin extends Pet
{
    private String sex;
    public Penguin()
    {
        super();
        sex = "";
    }
    public Penguin(String name)
    {
        super(name);
        sex = "";
    }
    public Penguin(String name, int health, int love, String sex)
    {
        super(name, health, love);
        this.sex = sex;
    }
    public String getSex()
    {
        return sex;
    }
    public void setSex(String sex)
    {
        this.sex = sex;
    }
    @Override
    public void print()
    {
        System.out.println("-----企鹅的自白-----");
        super.print();
        System.out.println("性别：" + sex);
        System.out.println("------------------");
    }

    @Override
    public void eat()
    {
        setHealth(getHealth()+5);
        System.out.println("企鹅" + getName() + "正在吃鱼肉……它的健康值增长到" + getHealth());
    }

    public void swimming(Master master)
    {
        System.out.println("主人" + master.getName() + "正在和编号是" + getIndex() + "的企鹅" + getName() + "游泳...");
        setHealth(getHealth()-4);
        setLove(getLove()+5);
        System.out.println("企鹅" + getName() + "的健康值减少到" + getHealth() + "，亲密度增加到" + getLove());
    }
}
```

**Master.java**

```java
package PetPark;

public class Master
{
    private String name;
    public Master(String name)
    {
        this.name = name;
    }
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    public void adopt(Pet pet)
    {
        System.out.println("主人" + name + "领养了宠物" + pet.getName());
    }
    public void feed(Pet pet)
    {
        pet.eat();
    }
    public void playWith(Pet pet)
    {
        if(pet instanceof Dog)
        {
            ((Dog)pet).catchingFlyDisk(this);
        }
        else
        {
            ((Penguin)pet).swimming(this);
        }
    }
}
```

**PetPark.java**

```java
package PetPark;

public class PetPark
{
    public static void main(String[] args)
    {
        Pet dog = new Dog("特朗普");//用子类的构造方法创建以父之名声明的对象，本质是子类的对象
        ((Dog)dog).setStrain("金毛");//用强制类型转换，调用独属于子类的方法
        dog.print();//这里调用的是子类的方法
        //静态多态：编译时多态，通过方法重载实现
        //动态多态：运行时多态，通过方法覆盖实现
        Pet penguin = new Penguin("咕咕嘎嘎", 80, 90, "F");
        penguin.print();

        Master master1 = new Master("Tina");
        Master master2 = new Master("Tear");
        master1.adopt(dog);
        master2.adopt(penguin);
        System.out.println("----------------------");
        master1.feed(dog);
        master2.feed(penguin);
        System.out.println("----------------------");

        master1.playWith(dog);
        master2.playWith(penguin);
    }
}
```

## 【运行结果截图】

![](https://img2024.cnblogs.com/blog/2979549/202606/2979549-20260608213133855-195732445.png)

## 【思考题】

1.面向对象的三大特征分别是什么？各是什么含义。

答：

<u>封装性：把对象的状态（属性）和行为（方法）结合成一个独立的软件单元，并尽可能隐藏对象的内部细节；</u>

<u>继承性：一个对象能获得另一个对象的属性；</u>

<u>多态性：一个程序中相同的名字能表示不同的含义；</u>

2.什么是静态多态和动态多态？分别如何实现？

答：

<u>静态多态是编译时多态，通过方法重载实现；</u>

<u>动态多态是运行时多态，通过方法覆盖实现；</u>
