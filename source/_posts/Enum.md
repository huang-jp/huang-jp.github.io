---
title:  Java 枚举(enum) 详解7种常见的用法
---
用法一：常量
public enum Color {  
RED, GREEN, BLANK, YELLOW  
}  


用法二：switch
enum Signal {  
    GREEN, YELLOW, RED  
}  
public class TrafficLight {  
    Signal color = Signal.RED;  
    public void change() {  
        switch (color) {  
        case RED:  
            color = Signal.GREEN;  
            break;  
        case YELLOW:  
            color = Signal.RED;  
            break;  
        case GREEN:  
            color = Signal.YELLOW;  
            break;  
        }  
    }  
}  


用法三：向枚举中添加新方法
public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
    // 普通方法  
    public static String getName(int index) {  
        for (Color c : Color.values()) {  
            if (c.getIndex() == index) {  
                return c.name;  
            }  
        }  
        return null;  
    }  
    // get set 方法  
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
    public int getIndex() {  
        return index;  
    }  
    public void setIndex(int index) {  
        this.index = index;  
    }  
}  


用法四：覆盖枚举的方法
public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
    //覆盖方法  
    @Override  
    public String toString() {  
        return this.index+"_"+this.name;  
    }  
}  


用法五：实现接口
public interface Behaviour {  
    void print();  
    String getInfo();  
}  
public enum Color implements Behaviour{  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }  
//接口方法  
    @Override  
    public String getInfo() {  
        return this.name;  
    }  
    //接口方法  
    @Override  
    public void print() {  
        System.out.println(this.index+":"+this.name);  
    }  
}  


用法六：使用接口组织枚举
public interface Food {  
    enum Coffee implements Food{  
        BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO  
    }  
    enum Dessert implements Food{  
        FRUIT, CAKE, GELATO  
    }  
}  


测试继承接口的枚举的使用（by 大师兄 or 大湿胸。） 
private static void testImplementsInterface() {  
    for (Food.DessertEnum dessertEnum : Food.DessertEnum.values()) {  
        System.out.print(dessertEnum + "  ");  
    }  
    System.out.println();  
    //我这地方这么写，是因为我在自己测试的时候，把这个coffee单独到一个文件去实现那个food接口，而不是在那个接口的内部。  
    for (CoffeeEnum coffee : CoffeeEnum.values()) {  
        System.out.print(coffee + "  ");  
    }  
    System.out.println();  
    //搞个实现接口，来组织枚举，简单讲，就是分类吧。如果大量使用枚举的话，这么干，在写代码的时候，就很方便调用啦。  
    //还有就是个“多态”的功能吧，  
    Food food = Food.DessertEnum.CAKE;  
    System.out.println(food);  
    food = CoffeeEnum.BLACK_COFFEE;  
    System.out.println(food);  
}  

运行结果
![1](/images/Enum/1.png)


用法七：关于枚举集合的使用
package com.lxk.enumTest;  
  
/** 
 * Java枚举用法测试 
 * <p> 
 * Created by lxk on 2016/12/15 
 */  
public class EnumTest {  
    public static void main(String[] args) {  
        forEnum();  
        useEnumInJava();  
    }  
  
    /** 
     * 循环枚举,输出ordinal属性；若枚举有内部属性，则也输出。(说的就是我定义的TYPE类型的枚举的typeName属性) 
     */  
    private static void forEnum() {  
        for (SimpleEnum simpleEnum : SimpleEnum.values()) {  
            System.out.println(simpleEnum + "  ordinal  " + simpleEnum.ordinal());  
        }  
        System.out.println("------------------");  
        for (TYPE type : TYPE.values()) {  
            System.out.println("type = " + type + "    type.name = " + type.name() + "   typeName = " + type.getTypeName() + "   ordinal = " + type.ordinal());  
        }  
    }  
  
    /** 
     * 在Java代码使用枚举 
     */  
    private static void useEnumInJava() {  
        String typeName = "f5";  
        TYPE type = TYPE.fromTypeName(typeName);  
        if (TYPE.BALANCE.equals(type)) {  
            System.out.println("根据字符串获得的枚举类型实例跟枚举常量一致");  
        } else {  
            System.out.println("大师兄代码错误");  
        }  
  
    }  
  
    /** 
     * 季节枚举(不带参数的枚举常量)这个是最简单的枚举使用实例 
     * Ordinal 属性，对应的就是排列顺序，从0开始。 
     */  
    private enum SimpleEnum {  
        SPRING,  
        SUMMER,  
        AUTUMN,  
        WINTER  
    }  
  
  
    /** 
     * 常用类型(带参数的枚举常量，这个只是在书上不常见，实际使用还是很多的，看懂这个，使用就不是问题啦。) 
     */  
    private enum TYPE {  
        FIREWALL("firewall"),  
        SECRET("secretMac"),  
        BALANCE("f5");  
  
        private String typeName;  
  
        TYPE(String typeName) {  
            this.typeName = typeName;  
        }  
  
        /** 
         * 根据类型的名称，返回类型的枚举实例。 
         * 
         * @param typeName 类型名称 
         */  
        public static TYPE fromTypeName(String typeName) {  
            for (TYPE type : TYPE.values()) {  
                if (type.getTypeName().equals(typeName)) {  
                    return type;  
                }  
            }  
            return null;  
        }  
  
        public String getTypeName() {  
            return this.typeName;  
        }  
    }  
}  

运行结果
![2](/images/Enum/2.png)


总结：
enum这个关键字，可以理解为跟class差不多，这也个单独的类。可以看到，上面的例子里面有属性，有构造方法，有getter，也可以有setter，但是一般都是构造传参数。还有其他自定义方法。那么在这些东西前面的，以逗号隔开的，最后以分号结尾的，这部分叫做，这个枚举的实例。也可以理解为，class  new 出来的实例对象。这下就好理解了。只是，class，new对象，可以自己随便new，想几个就几个，而这个enum关键字，他就不行，他的实例对象，只能在这个enum里面体现。也就是说，他对应的实例是有限的。这也就是枚举的好处了，限制了某些东西的范围，举个栗子：一年四季，只能有春夏秋冬，你要是字符串表示的话，那就海了去了，但是，要用枚举类型的话，你在enum的大括号里面把所有的选项，全列出来，那么这个季节的属性，对应的值，只能在里面挑。不能有其他的。