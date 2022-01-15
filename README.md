# java14
类、代码块与属性顺序、main方法、饿汉式，懒汉式，单列设计模式
第二阶段

类变量和类方法
理解main方法语法static
代码块
单列设计模式
final关键字
抽象类
接口
内部类

类变量--提出问题
提出问题的主要目的就是让大家思考解决之道，从而引出我要讲的知识点，说：有一群小孩在玩堆雪人，不时有新的小孩加入，请问如何知道现在共有多少人在玩？
思路：
在main方法中定义一个变量count
2.当以一个小孩加入游戏后count++，最后个count就记录有多少小孩玩游戏
package com.hspedu.static_;

public class ChildGame {
    public static void main(String[] args) {

        Child child1= new Child("狐狸精");
        child1.join();
       child1.count++;
        Child child2= new Child("狐");
        child1.join();
        child2.count++;
        Child child3= new Child("狸精");
        child1.join();
        child3.count++;
        //============
        System.out.println("共有"+ Child.count);//3，类变量可以通过类名来访问
    }

}
class Child{
    private String name;
    //定义一个变量count, 是一个类变量（static）静态
    //该变量最大的特点就是会被Child类的所有的对象实例共享
    public  static int count =0;//静态变量是被对象共有的
    public Child(String name){
        this.name = name;
    }
    public void join(){
        System.out.println("小孩加入了游戏");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }
}










什么事类变量
类变量也叫静态变量/静态属性，是该类的所有对象共享的变量，任何一个该类的对象去访问它时，取到的都是相同的值，同样任何一个该类的对象去修改它时，任何一个该类的对象去访问它时，取到的都是相同的值，同样任何一个该类的对象去修改它时，修改的也是同一个变量。这个从前面的图可以看出来。

如何定义类变量
访问修饰符static数据类型 变量名； 推荐
static 访问修饰符 数据类型 变量名；
如何访问类变量
类名。类变量名
或者对象名.类变量名
推荐：类名.类变量名;

类变量使用注意事项和细节讨论
1.什么时候需要用类变量
当我们需要让某个类的所有对象都共享一个变量时，就可以考虑使用类变量（静态变量）：比如：定义学生类，统计所有学生共交多少钱。Student(name,fee)
2.类变量时与实例变量(普通属性)区别
类变量时该类的所有对象共享的，而实例变量时每个对象独享的。
3.加上static称为类变量或静态变量，否则称为实例变量/普通变量/非静态变量
4.类变量可以通过类名，类变量名或者对象名.类变量名 来访问，但java设计者推荐我们使用 类名.类变量名方式访问。
5.实例变量不能通过 类名.类变量名方式访问。
6.类变量是在类加载时就初始化了，也就是说，即使你没有创建对象， 只要类加载了，就可以使用类变量了。
7.类变量的声明周期是随类的加载开始，随着类消亡而销毁。

类变量和类方法
类方法也叫静态方法
形式：
访问修饰符 static数据返回类型方法名（）{}
调用方式：
类名.类方法名 或者 对象名. 类方法名

类方法经典的使用场景
当方法中不涉及到任何和对象相关的成员，则可以将方法设计成静态方法， 提高开发效率

比如：工具类中的方法 utils
Math类，Arrays类，Collections 集合类看下源代码
程序员实际开发，往往会将一些通用的方法，设计成静态方法，这样我们不需要创建对象就可以使用了，比如打印一堆数组，冒泡排序，完成某个计算任务

package com.hspedu.static_;

import java.lang.reflect.Array;
import java.util.Arrays;

class Stti{
    public static void main(String[] args) {
        Stu stu = new Stu("tom");
        stu.payFee(100);
        stu.showFee();
        Stu.payFee(200);
        Stu.showFee();
        stu.showFee();

    }
}
public class Stu {
    private String name;
    private static double fee = 0;

    public Stu(String name){
        this.name=name;

    }
    //1.当方法使用了static修饰后，该方法就是静态方法
    //2.静态方法就可以访问静态属性
    public static void payFee(double fee){
        Stu.fee += fee;
    }
    public static void showFee(){
        System.out.println("总学费有：" + Stu.fee);
      
    }
}//如果我们希望不创建实例，也可以调用某个方法（即当做工具来使用）



使用的时候注意事项
1.类方法和普通方法都是随着类的加载而加载，将结构信息存储在方法区；
类方法中无this的参数
普通方法中隐含着this的参数
2.类方法可以通过类名调用，也可以通过对象名调用。
3.普通方法和对象有关，需要通过对象名调用，比如对象名.方法名（参数），不能通过类名调用
4.类方法中不允许使用和对象有关的关键字，比如this和super。普通方法（成员方法）可以。
5.类方法（静态方法）中 只能访问 静态变量 或静态方法
6.普通成员方法，既可以访问 普通变量（方法），也可以访问静态变量

小结：静态方法，只能访问静态的成员，非静态的方法，可以访问静态成员和非静态成员-遵守访问权限

class A{
    public  static String name ="函数屁股";//private 在其他类不能访问其他类
    public int sss=5;//private 在其他类不能访问
    public void say(){
        this.name = name;
        this.say();
        System.out.println(this.name);
    
        
     say2();//静态非静态都可以
     A.say2();
     

    }
    public  static void say2(){
        
    }
class A{
    public  static String name ="函数屁股";//private 在其他类不能访问其他类
    public int sss=5;//private 在其他类不能访问
    public static void say(){
        this.name = name;//错
        this.say();//错
        System.out.println(this.name);


     say2();//静态非静态都可以
     A.say2();


    }
    public  static void say2(){

    }

}

class A{
    public  static String name ="函数屁股";//private 在其他类不能访问其他类
    public int sss=5;//private 在其他类不能访问
    public static void say(){
        this.name = name;//错
        this.say();//错
        System.out.println(this.name);


     say2();//错
     A.say2();//错


    }
    public   void say2(){

    }

}

main方法
1.java虚拟机需要调用类的main（）方法， 所以该方法的访问权限必须是public
2.java虚拟机在执行main（）方法时不必创建对象，所以该方法必须是static
3.该方法接收String类型的数组参数，该数组中保存执行java命令时传递给所运行的类的参数
4.java执行的程序参数1 参数2 参数3




代码块
代码化块又称为初始化块，属于类中的成员，类似于方法，将逻辑语句封装在方法体中，通过{}包围起来。
但和方法不同，没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显示调用，而是加载类时，或创建对象时隐式调用。
基本语法
【修饰符】{
代码
}；
注意：
1.修饰符 可选，要写的话，也只能写static
2.代码块分为两类，使用static 修饰的叫静态代码块，没有static修饰的，叫普通代码块
3.逻辑语句可以为任何逻辑语句（输入、输出、方法调用、循环、判断等）
4.；号可以写上，也可以省略


代码块
1.相当于另外一种形式的构造器（对构造器的补充机制）,可以做初始化的操作
2.场景：如果多个构造器中都有重复的语句，可以抽取到初始化块中，提高代码的重用性
3.代码块的快速入门

代码块细节
创建一个对象时，在一个类 调用顺序是：
1.调用静态代码块和静态属性初始化（注意：静态代码块和静态属性初始化调用的优先级一样，如果有多个静态代码块和多个静态变量初始化，则按他们定义的顺序调用）
2.调用普通代码块和普通属性的初始化（注意：普通代码块和普通属性初始化调用的优先级一样，如果有多个普通代码块和多个普通属性初始化，则按定义顺序调用）
3.调用构造方法


代码块细节
1.static代码块也叫静态代码块，作用就是对类进行初始化，而且它随着类的加载而执行，并且只会执行一次，如果是普通代码块，每创建一个对象，就执行
2.类什么时候加载
1.创建对象实例时new
2.创建子类对象实例时，父类也会被加载
3.使用类的静态成员时（静态属性、静态方法）
3.普通代码块，在创建对象实例时，会被隐式的调用，被创建一次，就会调用一次。
如果只是使用类的静态成员时，普通代码块并不会执行


public class CodeBlockDetail01 {
    public static void main(String[] args) {
        //类被加载的轻快举例
        //1.创建对象实例时，父类也会被加载
       BB bb = new BB();

        System.out.println(DD.n1);//静态代码块一定执行，普通代码块不执行

    }

}
class AA{
    static{
        System.out.println("AA 的静态代码1被执行");
    }
}class BB extends AA{
    static{
        System.out.println("BB的代码块被执行");
    }//普通代码块，new对象时，被调用，而且是每创建一个对象，调用一次
}class DD {
    public static int n1 = 8888;
    static{
        System.out.println("DD的静态代码块");//每创建一次调用一次
    }
    //可以这样简单的，理解普通代码块是构造器的补充
    {
        System.out.println("DD的普通代码块");
    }
    //小结：static代码块是类加载时，执行，只会执行一次，
    //普通代码块是在创建对象时的调用的，创建一次，调用一次
    //类加载的3中情况需要记住
}

细节2：
创建一个对象时，在一个类调用顺序是：
1.调用静态代码块和静态属性初始化（注意：静态代码块和静态属性初始化调用的优先级一样，如果有多个静态代码块和多个静态变量初始化，则按他们定义的顺序调用）
上面两个最先执行
2.调用普通代码块和普通属性的初始化（注意：普通代码块和普通属性初始化，则按定义顺序调用）
3.调用构造方法


package com.hspedu.static_;

public class CodeBlockDetail02 {
    public static void main(String[] args) {
        AAA a = new AAA();//(1)getN1被调用。。。（2）A静态代码块1 按上下顺序来
//先搞静态的再搞普通代码块(3)getM2被调用（4）普通代码块（5）最后出无限构造器
    }
}
class AAA{


    //静态属性的初始化
    private int  n2 = getN2();
    static{
        System.out.println("A的静态代码块");
    }
    private static int n1 =getN1();
    public static int getN1(){
        System.out.println("静态方法getN1被调用...");
        return 100;
    }public int getN2(){
        System.out.println("公用方法getN2被调用");
        return 200;
    } {
        System.out.println("普通代码块");
    }

    public AAA() {
        this.n2 = n2;
        System.out.println("构造器");
    }
}
A的静态代码块
静态方法getN1被调用...
公用方法getN2被调用
普通代码块
构造器

代码块
构造器的最前面其实隐含了super()和调用普通代码块，都写一个类演示
静态相关的代码块，属性初始化，在类加载时，就执行完毕






代码块最难的
我们看一下创建一个子类是（继承关系），他们的静态代码块，静态属性初始化，普通代码块，普通属性初始化，构造方法的调用顺序如下：
1.父类的静态代码块和静态属性（优先级一样，按定义顺序执行）
2.子类的静态代码块和静态属性（优先级一样，按定义顺序执行）
3.父类的普通代码块和普通属性初始化（优先级一样，按定义孙旭执行）
4.父类的构造方法
5.子类的普通代码块和普通属性初始化（优先级一样，按定义顺序执行）
6.子类的构造方法//面试题

package com.hspedu.static_;

public class CodeBlockDetail03 {
    public static void main(String[] args) {
        new DDD();
    }
}class MMM{//父类Object
    public static int a = getMMM();
    public static int getMMM(){
        System.out.println("MMM静态属性");
        return 555;
    }
    static{
        System.out.println("MMM的静态代码块");
    }
    {
        System.out.println("MMM的普通代码块");
    }

    public MMM(){
        //（1）super()
        //（2）调用本类的普通代码块
        System.out.println("MMM()构造器被调用。。。");
    }
}class DDD extends MMM{
    public static int a = getDDD();
    public static int getDDD(){
        System.out.println("DDD静态属性");
        return 555;
    }
    static {
        System.out.println("DDD的静态代码块");
    }
    {
        System.out.println("DDD的普通代码块");
    }public DDD(){

        //（1）super()
        //（2）调用本类的普通代码块
        System.out.println("DDD()构造器被调用。。。");
    }

}
MMM静态属性
MMM的静态代码块与（静态属性）同界别按上下顺序
DDD静态属性
DDD的静态代码块 与（静态属性）同界别按上下顺序

MMM的普通代码块和（普通属性）同级别按上下顺序
MMM()构造器被调用。。。
DDD的普通代码块      （普通属性）同级别按上下顺序
DDD()构造器被调用。。。


单例设计模式
什么是设计模式
1.静态方法和属性的经典使用
2.设计模式是在大量的实践中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模式就像是经典的棋谱，不同的棋局，我们用不同的棋谱，免去我们自己再思考和摸索

什么是单例模式
单个的实例
1.所谓类的单例设计模式，就是采取一定的方法保证整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法
2.单例模式有两种方式：1）饿汉式 2）懒汉式

单例模式应用实例
演示饿汉式和懒汉式
1.构造器初始化
2.类的内部结构变化
3.向外展露一个静态的公共方法。getlnstance
4.代码施行
public class SingleTon01 {
    public static void main(String[] args) {
//        GirlFriend 小红 = new GirlFriend("小红");
//        GirlFriend 小白 = new GirlFriend("小白");
        //小红小白这不叫单例
        //直接通过方法可以获取对象
        GirlFriend instance = GirlFriend.getInstance();
        System.out.println(instance);
        GirlFriend instance1 = GirlFriend.getInstance();
        //instance1 = instance
    }

}

//有一个类，GIrlFriend
//只能有一个女朋友
class GirlFriend{
    private String name;
    //为了能够在静态方法中使用，返回gf对象，需要将其修饰为static
    private static GirlFriend gf = new GirlFriend("小红红")
   //如何保障我们只能创建一个GRILFriend 对象
    //步骤 饿汉式单例模式
    //1.将构造器私有化
    //2.在类内部直接创建
    //3.提供一个公共的static方法，返回gf对象
    private GirlFriend(String name) {
        this.name = name;
    }
    public static GirlFriend getInstance(){
        return gf;
    }
}

懒汉式

public class SingleTon {
    public static void main(String[] args) {
        Cat instance = Cat.getInstance();
        

        System.out.println(instance);
    }
}
class Cat{
    private String name;
    public static int n1 = 999;
    private static Cat cat;
//懒汉式 只有当用户使用getInstance时，才返回cat对象，后面再次调用时，会返回上次创建的cat对象
    public Cat(String name) {
        this.name = name;
    }
    public static Cat getInstance(){
        if (cat == null){
            cat = new Cat("xxx");
        }return cat;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                '}';
    }

}
饿汉式VS懒汉式
1.二者最重要的区别在与创建对象的时机不同：饿汉式是在类加载就创建了对象实例而懒汉式是在使用时才创建。
2.饿汉式不存在线层安全问题，懒汉式存在线层安全问题（后面学习线层后，会完善一把）
3.饿汉式存在浪费资源的可能。因为如果程序员一个对象实例都没有使用，name饿汉式创建的对象就浪费了，懒汉式是使用时才创建，就不存在这个问题。
4.在我们javaSE标准类中，java.lang.Runtime 就是经典的单例模式


单例设计模式
小结：
1.单例模式的两种实现方式（1）饿汉式2（懒汉式0
2.饿汉式的问题“在类加载是就加在创建，可能存在资源浪费问题
3.懒汉式的问题：线层安全隐患，）
