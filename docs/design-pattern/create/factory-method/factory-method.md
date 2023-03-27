# 设计模式之工厂方法

<h3>1、背景</h3>

<h3>2、定义</h3>

<h3>3、结构</h3>

>此模式角色有抽象产品、具体产品、抽象工厂、具体工厂;具体的产品依赖于某具体的工厂类，就是说具体产品由具体工厂创建；符合开闭原则，每当引入新的产品只需新增新的具体工厂即可。

![image](factory-method.png)

<h3>4、实例</h3>

>现在有一个飞行射击类的需求，需要在屏幕上生成坦克、飞机、Boss等角色。

>以上用例结构图如下：

![image](example4.4.png)

>部分代码如下，详细代码在：<br>

```java
package org.arvinw.example4_4;
import org.arvinw.example4_3.Enemy;

/**
 * 抽象工厂
 */
public interface Factory {
    Enemy create (int screenWidth);
}
```

```java
/**
 * 抽象产品
 */
public abstract class Enemy {

    protected int x;
    protected int y;

    public Enemy(int x,int y){
        this.x = x;
        this.y = y;
    }
    public abstract void show();
}
```

```java
/**
 * 具体工厂
 */
public class BossFactory implements Factory{
    @Override
    public Enemy create(int screenWidth) {
        return new Boss(screenWidth/2,0);
    }
}
```

```java
import org.arvinw.example4_3.Enemy;
/**
 * 具体产品
 */
public class Boss extends Enemy {
    public Boss(int x, int y) {
        super(x, y);
    }

    @Override
    public void show() {
        System.out.println("Boss出现的位置");
        System.out.println("攻击玩家的boss");
    }
}
```

```java
/**
 * 使用工厂方法设计模式
 */
public class Client {
    public static void main(String[] args) {
        int screenWidth = 100;
        System.out.println("游戏开始");

        Factory factory = new TankFactory();
        for (int i=0;i<5;i++){
            factory.create(screenWidth).show();
        }

        factory = new AirplaneFactory();
        for (int i=0;i<10;i++){
            factory.create(screenWidth).show();
        }

        System.out.println("抵达关底");
        factory = new BossFactory();
        factory.create(screenWidth).show();
    }
}
```

<h3>5、其他框架实例</h3>

>JDK-Collection中的方法工厂：<br>抽象工厂:Collection<br>具体工厂：ArrayList、LinkedList、HashSet、TreeSet<br>抽象产品：Iterator<br>具体产品：定义在各个具体工厂的Itr，实现了Iterator