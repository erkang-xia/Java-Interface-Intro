# Java-Interface-Intro


Hello, everyone. I've written an article about Java interfaces that you should read. It is a fundamental feature , yet it was quite controversial. Interfaces were created to be used in the development of loosely connected, extendable, and testable programs. However, with the new Java 8 upgrades, there is a new feature of static method and default method. It may appear to be at odds with its intended aim. If you're curious, here's the link to the article.

Introduction

Similar to blueprints, interfaces define what a class must do rather than how it must do it. It is used to group related methods with empty bodies. An interface is declared with the interface keyword. That is, by default, all methods in an interface have an empty body and are public, while all fields are public, static, and final. To access the interface methods, another class need to  "implement" the interface (similar to inheriting) using the implements keyword (instead of extends).  The body of the interface method is provided by the "implement" class.

Why we need it?

With interface, we can achieve loose coupling and security by hiding details and revealing only the critical details of an object. It makes scaling process so much easier.  Additionally, it provides the possibility of “multi-inheritance”. Java does not allow the concept of "many inheritance" (a class can only inherit from one parent class). However, with interface, the class now can implement multiple interfaces. 

Interfaces are used to implement abstraction. Thus, a frequently asked topic is why use interfaces when abstract classes are available. The reason is, abstract classes may contain non-final variables, whereas variables in the interface are final, public and static.

New Features Added in Interfaces in JDK 8

Prior to JDK 8, the interface could not have a method with body. We now can include default implementations for interface methods. 
As with static methods in classes, static methods in interfaces may now be defined as well. As with static methods in classes, static methods in an interface may be called independently of any object.(Static interface methods are not inherited by-Implementing classes)










public interface MyInterface {
 int method1();
 // default method, providing default implementation
 default String displayGreeting(){
    return "Hello from MyInterface";
 }
 //static method
 static int getDefaultAmount(){
    return 0;
 }
}

Why we need those updates (still working on it)

Suppose we need to add a new function in an existing interface. Obviously, the old code will not work as the classes have not implemented those new functions. So with the help of default implementation, we will give a default body for the newly added functions. Then the old codes will still work.


Another feature that was added in JDK 8 is that we can now define static methods in interfaces that can be called independently without an object. Note: these methods are not inherited.

https://beginnersbook.com/2013/04/java-static-dynamic-binding/


1.	Construction is part of the implementation, not the interface. Any code that works successfully with the interface doesn't care about the constructor. Any code that cares about the constructor needs to know the concrete type anyway, and the interface can be ignored.

If we know some method can be shared across all class, we can write it as static method in the interface. Those method can’t be overridden but it can also clean up a lot of the code.




2.	Since we can inherit from multiple interfaces, if two interfaces contained the same static method signature and then a class implemented them both and called that method, then things could become complicated in a way that Java language creators wanted to avoid by disallowing multiple class inheritance in the first place. Same argument could obviously be made for interface not allowing any method definition in them at all. 


You can inherit two interface that has methods with same name and return type. But you can’t two interface that has methods with same name and different return type.
Static methods in interfaces are never inherited.

interface A {
    int method();
    //int method1(); //method with different return type
    static int method2(){
        return 1;
    }
}

interface B {
    int method();
    //void method1(); //method with different return type
    static int method2(){
        return 2;
    }
}

class Test implements A, B {
    public int method() {
        System.out.println("haha");
        return 0;
    }

    //public int method1(){ return 0} // Can't decide which type to return

    public static void main(String[] args) {
        Test test = new Test();
        test.method();
        //Static methods in interfaces are never inherited
        System.out.println(A.method2()); //static method can only be called through interface name
    }
}


default method doesn’t need to be implemented in the inherited class. The multiple inheritance problem can occur, when we have two interfaces with the default methods of same signature.
This is because we have the same method in both the interface and the compiler is not sure which method to be invoked. Can be solved by overriding the method.,

interface MyInterface{

    default void newMethod(){
        System.out.println("Newly added default method");
    }
}
interface MyInterface2{

    default void newMethod(){
        System.out.println("Newly added default method");
    }
}
public class Example implements MyInterface, MyInterface2{
    //Implementation of duplicate default method
    public void newMethod(){
        System.out.println("Implementation of default method");
    }
    public static void main(String[] args) {
        Example obj = new Example();

        //calling the default method of interface
        obj.newMethod();
   


    }
}


3.	why can you define static fields and static inner types within an interface, but not static methods?

I'm going to go with my pet theory with this one, which is that the lack of consistency in this case is a matter of convenience rather than design or necessity, since I've heard no convincing argument that it was either of those two.
Static fields are there (a) because they were there in JDK 1.0, and many dodgy decisions were made in JDK 1.0, and (b) static final fields in interfaces are the closest thing java had to constants at the time.
Static inner classes in interfaces were allowed because that's pure syntactic sugar - the inner class isn't actually anything to do with the parent class.
So static methods aren't allowed simply because there's no compelling reason to do so; consistency isn't sufficiently compelling to change the status quo.
Of course, this could be permitted in future JLS versions without breaking anything.

![image](https://user-images.githubusercontent.com/92690031/155376417-ddb462df-f5b4-4d08-aa67-361a4a0cf1aa.png)
