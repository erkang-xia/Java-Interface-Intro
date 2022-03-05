# Java Interface Introduction
## Intro

Similar to blueprints, [interfaces](https://www.w3schools.com/java/java_interface.asp) define what a class must do rather than how it must do it. It is used to group related methods with empty bodies. An interface is declared with the interface keyword. That is, by default, all methods in an interface have an empty body and are public, while all fields are [public](https://www.geeksforgeeks.org/access-modifiers-java/), [static](https://www.geeksforgeeks.org/static-keyword-java/?ref=gcse), and [final](https://www.geeksforgeeks.org/final-keyword-in-java/?ref=gcse). To access the interface methods, another class needs to "implement" the interface (similar to inheriting) using the implements keyword (instead of extends). The body of the interface method is provided by the "implement" class.

## Why do we need it?

With interfaces, we can achieve [loose coupling](https://www.geeksforgeeks.org/coupling-in-java/?ref=gcse) and security by revealing only the critical details of an object. It makes the scaling process so much easier. Additionally, it provides the possibility of "multi-[inheritance](https://www.geeksforgeeks.org/inheritance-in-java/?ref=gcse)"，which is not allowed in Java.
 

Interfaces are used to implement [abstractions](https://www.geeksforgeeks.org/abstraction-in-java-2/?ref=gcse). Thus, a frequently asked question is why we use interfaces when abstract classes are available. The reason is that abstract classes can have variables that aren't final, but the variables in the interface are final, public, and static.

## New Features Added to Interfaces in JDK 8

Prior to JDK 8, the interface could not have a method with a body. We can now include default implementations for interface methods, which don’t need to be implemented in the inherited class. Suppose we need to add a new function to an existing interface. The old code will not work as the classes have not implemented those new functions. So, with the help of the default implementation, we will provide a default body for the newly added functions. Then the old codes will still work.

As with static methods in classes, static methods in interfaces may now be defined as well. It may be called independently of any object and is not inherited by the implementing classes.








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

The fundamental purpose of the interface is to build a loosely coupled system that requires abstraction. In that case, why do we need a static method that requires detailed implementation and can't be overridden? In some circumstances, we know some methods can be shared across all classes, so we can write them as static methods in the interface. Because all the inherited classes require the same implementation, by declaring it in the interface, it can clean up a lot of the code.




### Because we can inherit from multiple interfaces, what would happen if two interfaces had the same static method name?


Because functions with the same signature in different base classes can cause naming problems,(we have the same method in both the interface and the compiler is not sure which method to be invoked) multi-inhritance is prohibited in Java. It was avoided for the interface since the interface's functions are already overloaded. However, how can we overcome this complexity with a static function specified in the interface? The truth is that there is no reason to worry about conflict. It is impossible to implement both interfaces at the same time if they contain a method with the same signature but different return types. Another reason is that the static method cannot be inherited, hence there will be no conflections as a result of inheritance.

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

Even for the instance of method that can be inherited and have detailed implementation, such as default method,it can be solved by overriding the method.

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
### Why is it possible to initialize an interface with static fields and static inner types but not with static methods?

It may be because of inconsistency. Rather than being a design necessity, it is a matter of convenience. Because static fields were in JDK 1.0, which made a lot of bad decisions, and because static final fields in interfaces were the closest thing Java had to constants at the time. Static inner classes are permitted in interfaces because they are pure syntactic sugar (the inner class has no relationship to the parent class).

Because there isn't a good reason to change the way things are, static methods aren't allowed. Consistency isn't strong enough to change the way things are.

Naturally, this could be allowed in future JLS versions without causing any problems.


