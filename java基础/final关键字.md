[参考自final关键字](https://www.cnblogs.com/xiaoxi/p/6392154.html)

### final关键字的使用方法

#### 一、final关键字的用法
Java中的final关键字可以用来修复类、方法、和变量。具体使用场景如下。
1. 修饰类

   
    被final修饰的类代表这个类永远无法被继承。如果在开发过程中有类希望无法被继承，则可以用final进行修饰。实验结果如下：
```java
// 类1  为一个final修饰的类
public final class FinalTest {

}
// 类2 尝试继承这个final类
// error Cannot inherit from final FinalTest
// 提示无法继承一个final类
public class FinalTestSon extends FinalTest {

}
```
2. 修饰方法

   
    使用final修饰的方法表示这个方法无法进行重写。如果你存在一个方法，不希望子类覆盖改方法的实现，则可以将其设计为final方法。因此如果一个方法使用了final进行了修饰，其子类是不能出现一个与其相同的方法，如下。
```java
// 父类中方法使用了 final进行修饰
public class Test {
    public final void finalMethod(){
        
    }
}

// 子类
public class Son extends Test {
    // error  'finalMethod()' cannot override 'finalMethod()' in 'com.Test(即父类)'; overridden method is final
    // 即不能覆盖父类的final方法
    public void finalMethod() {

    }
}
```
3. 修饰变量

   
    对于final修饰变量的情况相对较多，分别包括final修饰的成员变量、局部变量、以及方法参数，对于修饰的变量又可分为基本类型和引用类型。对于各种情况的详情如下。

- 被final修饰的成员变量  被final修饰的成员变量需要在**定义**或者**构造函数**或者**实例化代码块**中，对于这个部分的理解可以自己补充一下变量定义、静态代码块、实例化代码块、构造函数的初始化先后顺序的了解。

```java 
// 一、不进行初始化
public class Son {
    // error Variable 'a' might not have been initialized
    // 提示 a 未进行初始化
    final String a;
}

// 二、在定义的时候进行初始化 编译通过
public class Son {
    final int a = 1; 
}

// 三、构造方法中进行初始化 编译通过
public class Son {
    final int a;
    public Son() {
        this.a = 1;
    }
}
// 四、在创建类实例代码块中进行初始化 编译通过
public class Son {
    final int a;
    {
        this.a = 1;
    }
}
```

- 被final修饰的成员变量 对于被final修饰的局部变量，只需要在使用的时候将其初始化即可，可以是在定义的时候进行初始化，也可以在第一次使用的时候进行初始化，但是一经初始化赋值，则不能进行第二次赋值。

```java
// 正常情况一
public class Son {
    public static void main(String[] args) {
        final int c;
        c = 1;
    }
}
// 正常情况二
public class Son {
    public static void main(String[] args) {
        final int c = 1;
    }
}
// 在第一次赋值后再对其进行赋值
public class Son {
    public static void main(String[] args) {
        final int c;
        c = 1;
        // error variable 'c' might already have been assigned to
        // 变脸c不能再进行赋值
        c = 2;
    }
}

```

- 被final修饰的方法参数 

```java
// 一、不能直接为修饰final的变量进行赋值
public class Son {
    public static void method1(final String str) {
        // error Cannot assign a value to final variable 
        // 提示不能改变一个final修饰的变量
        str = str + "";
        System.out.println(str);
    }
    public static void main(String[] args) {
        String str = "hello";
        method1(str);
    }
}
// 二、final修饰的变量为引用类型 可以改变对象内容
public class Test {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer("str ");
        new Test().method1(str);
    }
    public void method1(final StringBuffer str) {
        str.append("haha");
        System.out.println(str);
    }
}
// console=> str haha
```

**从上面结果可以看出，被final修饰的变量，其本身不能再进行赋值，但是其指向的对象的内容是可以进行改变的。**

- 被final修饰的变量编译器优化的情况

```java
public class Test {
    public static void main(String[] args) {
        String a = "final2";
        String b = "final";
        final String c = "final";
        String d = c + 2;
        String e = b + 2;
        System.out.println(a == d);
        System.out.println(a == e);
        System.out.println(a == "final2");
        System.out.println(d == "final2");
    }
}
// console 
/*
true
false
true
true
*/
```

这里涉及到编译器优化的问题，在将Java源程序成字节码的过程中存在词法分析、语法分析、语义分析的过程。在语义分析过程中编译器会进行优化，对于**可确定的表达式**会进行优化处理。上面c + 2，对于c被final修饰，编译器将其认定为常量，因此对于表达式c + 2会被优化成结果 "final2"的常量，d则指向常量池中的final2，与a所指向的相同。而对于e则需要在运行过程中才能确定。