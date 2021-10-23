---
title: "Spring 中控制反轉及依賴注入"
date: 2019-10-29T10:07:47+06:00
draft: false


# meta description
description: "IOC 及 DI 的運作"

# taxonomies
categories:
  - "Java"
tags:
  - "Java"
  - "Spring"


# post type
type: "featured"
---

<hr>

#### IoC = Inversion of Control (控制反轉)

什麼是IoC呢？我們以印表機來舉個例子。

假設這世界上只有Hp品牌的印表機，而學校的老師和同學們都需要一台印表機，這時我們會這麼寫

```
public interface Printer {
    void print(String message);
}

public class HpPrinter implements Printer{
    @Override
    public void print(String message){
        System.out.println("HP印表機: " + message);
    }
}

public class Teacher {
    private Printer printer = new HpPrinter();
    public void teach(){
        printer.print("I'm a teacher");
    }
}

public class Student {
    private Printer printer = new HpPrinter();
    public void learn(){
        printer.print("I'm a student");
    }
}
```

這時老師跟學生都有一台印表機，且都能將自己的身份印出

但這時，新的印表機品牌出現了

```
public class CanonPrinter implements Printer{
    @Override
    public void print(String message){
        System.out.println("Canon印表機: " + message);
    }
}
```

這時候，如果老師跟學生都要統一換成新品牌時，我們必須改兩個地方，會造成一個困擾，假設我用10次，就要改10次，非常麻煩

而學生跟老師只是需要一台印表機，並不在乎品牌．

這時候呢spring出現了，他幫我們保管了印表機這個Object，而當有人需要使用印表機時，Spring就會提供他使用

程式就變這樣

```
public class Teacher {
    private Printer;
    public void teach(){
        printer.print("I'm a teacher");
    }
}

public class Student {
    private Printer;
    public void learn(){
        printer.print("I'm a student");
    }
}
```

當程式啟動時，Spring會預先存放一台印表機object在Spring 容器內，而當老師或學生需要使用時，容器會提供給他們使用

好了我們再回頭看IoC的定義

原本印表幾的控制權教在Teacher和 Student手上，而利用Spring後，控制權轉交到Spring 容器手上

由Spring 容器去new Object，這會帶來以下好處：
1. Loose coupling 鬆耦合
2. Lifecycle Management生命週期管理
3. More testable 方便測試

在我們了解SpringIoC觀念後，我們來講講如何將印表機交給Spring容器管理

#### @Component

- 用法：只能加在class上
- 用途：將該class變成Spring 容所管理的object

```
@Component
public class HpPrinter implements Printer{
    @Override
    public void print(String message){
        System.out.println("HP印表機: " + message);
    }
}
```

這時再啟動Spring 程式時，Spring會提供一個Spring容器，並把有@Component註解的class new出一個object並存放在容器內，而這些被Spring容器創建並管理的Object，我們統稱為Bean。

hint. Bean的名稱為class name的第一個字母轉小寫

![image](../../images/post/post1-1.jpg)

#### DI = Dependency Injection (依賴注入)

我們已經成功將hpPrinter成功註冊到Spring容器內，接下來我們要讓Teacher和Student能夠取得這台hpPrinter

有兩個步驟

1. 將Teacher也變成Bean
2. printer 上面加上@Autowired

```
@Component
public class Teacher {
    @Autowired
    private Printer printer;
    public void teach(){
        printer.print("I'm a teacher");
    }
}
```

在printer上加上@Autowired，Spring就會將hpPrinter交給teacher，這個動作稱之為Dependency Injection(依賴注入)

這時teacher就可以拿hpPrinter去印東西了。

#### 總結
IoC與DI相輔相成，IoC將object存放在容器裡面，而DI使Object可以取得其他Object來使用

以下為Spring IoC 常見名詞：

- IoC = Inversion of Control (控制反轉)

- DI = Dependency Injection (依賴注入)

- Bean = 存放在Spring容器裡的object

- @Component註解 = 加在class上，將該class變成Spring容器所管理的bean

- @Autowired註解 = 加在變數上，取得Spring容器中的Bean