---
title:  封装，继承，多态和抽象,好处与用法
---
1.封装
封装的概念：把对象的属性和方法结合成一个独立的整体，隐藏实现细节，并提供对外访问的接口。
封装的好处：
（1）：隐藏实现细节。好比你买了台电视机，你只需要怎么使用，并不用了解其实现原理。
（2）：安全性。比如你在程序中私有化了age属性，并提供了对外的get和set方法，当外界 使用set
方法为属性设值的时候 你可以在set方法里面做个if判断，把值设值在0-80岁，那样他就不能随意赋值了。
（3）：增加代码的复用性。好比在工具类中封装的各种方法，你可以在任意地方重复调用，
而不用再每处都去实现其细节。
（4）：模块化。封装分为属性封装，方法封装，类封装，插件封装，模块封装，系统封装等等。有
利于程序的协助分工，互不干扰，方便了模块之间的相互组合与分解，也有利于代码的调试和维护。比如人体由各个器官所组成，如果有个器官出现问题，你只要去对这个器官进行医治就行了。
2.继承
继承的概念：从已知的一个类中派生出新的一个类，叫子类。子类实现了父类所有非私有化属性和方
法，并能根据自己的实际需求扩展出新的行为。
继承的好处：
（1）：继承是传递的，容易在其基础上构造，建立和扩充出新的类。
（2）：简化了人们对事物的认识和描述，能清晰体现相关类之间的层次结构关系。
（3）：能减少数据和代码的冗余度。
（4）：大大增加了代码的维护性。
3.多态
多态的概念：多个不同的对象对同一消息作出响应，同一消息根据不同的对象而采用各种不同的行为方法。
多态的好处：主要是利于扩展。直接上代码自己来体会。
//比如我在马剑威磨砺营毕业后，找到一份工作，前期买了一辆大众车，实现代码如下：  
public class test{  
    public static void main(String[] args){  
        DZ dz =new DZ("大众");  
        people p =new people();  
        p.drive(dz);  
    }  
}  
  
class people{  
    public String name;  
    public DZ dz;  
    public void drive(DZ dz){  
        this.dz=dz;  
        dz.run();  
    }  
}  
  
class DZ{  
    public String name ;  
      
    public DZ(String name){  
        this.name=name;  
    }  
    public void run(){  
        System.out.print("上海大众在已120迈的速度在跑");  
    }  
}  

//后期如果我有钱以后又买了一辆奔驰，我要换奔驰 ，不用多态的情况下代码如下：  
public class test{  
    public static void main(String[] args){       
        Benz benz=new Benz("奔驰车");  
        people p =new people();  
        p.drive(benz);  
    }  
}  
    }  
      
  
class people{  
    public String name;  
    public DZ dz;  
    public Benz benz;  
      
    public void drive(DZ dz){  
        this.dz=dz;  
        dz.run();  
    }  
      
    public void drive(Benz benz){  
        this.benz=benz;  
        benz.run();  
    }  
}  
  
class DZ{  
    public String name ;  
      
    public DZ(String name){  
        this.name=name;  
    }  
    public void run(){  
        System.out.print(name+"已在路上飞快的奔跑");  
    }  
}  
  
class Benz{  
    public String name;  
    public Benz(String name){  
        this.name=name;  
    }  
    public void run(){  
        System.out.print(name+"已在路上飞快的奔跑");  
    }  
      
}  

//这样是不是程序粘连性太强，不利于扩展，而且Java特性是对扩展开放，对修改关闭 我每换一辆车就需要改代码。  
//那样代码修改太多，也不安全，用了多态以后，效果就很不一样了，妈妈在也不用担心我换车了
public class test{  
    public static void main(String[] args){       
        Benz benz=new Benz("奔驰车");  
        DZ dz=new DZ("大众车");  
        BMW bmw=new BMW("宝马车");  
        people p =new people();  
        p.drive(benz);  
        p.drive(dz);  
        p.drive(bmw);  
    }  
}  
  
class people{  
    public String name;  
    public Car car;  
      
      
        this.car=car;  
        car.run();  
    public void drive(Car car){  
}  
  
class Car{  
    public String name;  
      
    public Car(String name){  
        this.name=name;  
    }  
      
    public void run(){  
        System.out.println(name+"已在路上飞快的奔跑");  
    }  
}  
class DZ extends Car{  
   public DZ(String name){  
        super(name);  
    }  
}  
  
class Benz extends Car{  
    public Benz(String name){  
        super(name);  
    }  
}  
  
class BMW extends Car{  
    public BMW(String name){  
        super(name);  
    }  
   
}  
4.抽象
抽象的概念：通过特定的实例抽取出共同的特征以后形成的概念的过程，它强调主要特征和忽略次要
特征。