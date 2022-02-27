# Java Interface Introduction

## Intro

Similar to blueprints, [interfaces](https://www.w3schools.com/java/java_interface.asp) define what a class must do rather than how it must do it. It is used to group related methods with empty bodies. An interface is declared with the interface keyword. That is, by default, all methods in an interface have an empty body and are public, while all fields are [public](https://www.geeksforgeeks.org/access-modifiers-java/), [static](https://www.geeksforgeeks.org/static-keyword-java/?ref=gcse), and [final](https://www.geeksforgeeks.org/final-keyword-in-java/?ref=gcse). To access the interface methods, another class needs to "implement" the interface (similar to inheriting) using the implements keyword (instead of extends). The body of the interface method is provided by the "implement" class.

## Why we need it?

With interfaces, we can achieve [loose coupling](https://www.geeksforgeeks.org/coupling-in-java/?ref=gcse) and security by hiding details and revealing only the critical details of an object. It makes the scaling process so much easier. Additionally, it provides the possibility of "multi-inheritance." Java does not allow the concept of "many inheritance" (a class can only inherit from one parent class). However, with interfaces, the class can now implement multiple interfaces. 
 
Interfaces are used to implement abstractions. Thus, a frequently asked question is why we use interfaces when abstract classes are available. The reason is that abstract classes may contain non-final variables, whereas variables in the interface are final, public, and static.

## New Features Added in Interfaces in JDK 8

Prior to JDK 8, the interface could not have a method with a body. We can now include default implementations for interface methods, which don’t need to be implemented in the inherited class. Suppose we need to add a new function to an existing interface. Obviously, the old code will not work as the classes have not implemented those new functions. So, with the help of the default implementation, we will provide a default body for the newly added functions. Then the old codes will still work.
As with static methods in classes, static methods in interfaces may now be defined as well. As with static methods in classes, static methods in an interface may be called independently of any object.Static interface methods are not inherited by implementing classes.









```Java
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
```

## Questions:

### Why do we even need the static method in the interface?
1.	Construction is part of the implementation, not the interface. Any code that works successfully with the interface doesn't care about the constructor. Any code that cares about the constructor needs to know the concrete type anyway, and the interface can be ignored.
2.	If we know some method can be shared across all class, we can write it as static method in the interface. Those method can’t be overridden but it can also clean up a lot of the code.




### Since we can inherit from multiple interfaces, if two interfaces contained the same static method signature and then a class implemented them both and called that method, then things could become complicated in a way that Java language creators wanted to avoid by disallowing multiple class inheritance in the first place. Same argument could obviously be made for interface not allowing any method definition in them at all. 


1.	You can inherit two interface that has methods with same name and return type. But you can’t two interface that has methods with same name and different return type.
Static methods in interfaces are never inherited.

```Java
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
```

The multiple inheritance problem can occur too, when we have two interfaces with the default methods of same signature. This is because we have the same method in both the interface and the compiler is not sure which method to be invoked. Can be solved by overriding the method.,

```Java
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

```
### why can you define static fields and static inner types within an interface, but not static methods?

It could be the lack of consistency. It is a matter of convenience rather than design or necessity.Static fields are there (a) because they were there in JDK 1.0, and many dodgy decisions were made in JDK 1.0, and (b) static final fields in interfaces are the closest thing java had to constants at the time.
Static inner classes in interfaces were allowed because that's pure syntactic sugar - the inner class isn't actually anything to do with the parent class.
So static methods aren't allowed simply because there's no compelling reason to do so; consistency isn't sufficiently compelling to change the status quo.
Of course, this could be permitted in future JLS versions without breaking anything.
