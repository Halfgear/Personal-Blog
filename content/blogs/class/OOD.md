---
title: "Design Patterns"
date: 2023-02-08
draft: true
author: "HwiJoon Lee"
tags:
  - Object Oriented Design
image: /posts/OOD/java.png
description: "Explanation of Design Patterns for beginners"
---
## Introduction

Design patterns are essential in development as they provide a structured approach to solve common problems. Object-oriented design patterns are the most widely used patterns, and they form the backbone of most software applications. 
<img src="/posts/OOD/mypic.PNG" style="display: block; margin-left: auto; margin-right: auto; width: 80%; height: 80%;"/>
In this post, I'll share my experience as a OOD TA and insights on how these patterns work and provide you with some sample code to help you understand them better. We will explore Singleton, Factory, Strategy, and Observer patterns, and how these patterns can be used to improve the quality of your designs and provide you with code samples to illustrate their implementation.

## Singleton Pattern
Singleton pattern helps in creating only one instance of a class and ensures that this instance is globally accessible throughout the program. The pattern works by defining a class that has only one instance and provide global point of access to that instance.

```class Singleton{
    private static class singleInstanceHolder{
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return singleInstanceHolder.INSTANCE;
    }
}
```
The 'INSTANCE' variable is the only instance of Singleton class. By declaring 'INSTANCE' as 'final', it ensures that there is only one instance of the 'Singleton' class created becuase final keyword prevents the 'INSTANCE' variable from being reassigned after it is initialized. 

The 'getInstance()' method returns the single instance of the class as a getter. 'public' keywords allow the method to be accessed from outside the Singleton class. It provides the global access point to our instance throught the program.

```
public class HelloWorld{
    public static void main(String[] args) {
        
        Singleton a = Singleton.getInstance();
        Singleton b = Singleton.getInstance();
        System.out.println(a.hashCode());
        System.out.println(b.hashCode());
        if (a==b) {
            System.out.print("output: true");
        }
    }
}
```
```
output: true
```
Both 'a' and 'b' refers to the same instance of the Singleton class. Therefore, 'a.hashCode()'  and 'b.hashCode()' will always be equal. If you understand the concept of singleton pattern, you will be able to use it to create a database connection, a thread pool, or a cache.

## Factory Pattern
The factory pattern is a creational pattern that allows you to create objects without specifying the exact class of object that will be created. Instead you delegate the creation of smaller objects to a separate class called a factory. This pattern is useful when you need to create many objects of the same type, but with different values or properties.



```abstract class Coffee {
    //abstract method, so subclass must implement this method 
    public abstract int getPrice(); 
    
    @Override
    public String toString(){
        return "Hi this coffee is "+ this.getPrice();
    }
}
class Latte extends Coffee { 
    private int price; 
    
    public Latte(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
class Americano extends Coffee { 
    private int price; 
    
    public Americano(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
class CoffeeFactory {
    //factory method to create object of type Coffee
    public static Coffee getCoffee(String type, int price){
        if("Latte".equalsIgnoreCase(type)) {
            return new Latte(price)
        };
        else if("Americano".equalsIgnoreCase(type)) {
            return new Americano(price)
        };
        else {
            System.out.println("Invalid Coffee Type");
        } 
    }
}
```
We have an abstract class Coffee with an abstract method getPrice(). We then have two concrete classes that extend Coffee: Latte and Americano. Latte and Americano have a constructor that takes in a price and initialize the private field.

There could be more concrete classes that extend Coffee, such as Espresso, Mocha, etc. If we create a new coffee object, we would have to write many lines of code to create the object with field initialization. This is where the factory pattern comes in!

We have a CoffeeFactory class that has a static method getCoffee(). This method takes in a type and a price and returns a Coffee object. The getCoffee() method uses the type parameter to determine which concrete class to instantiate. The price parameter is passed to the constructor of the concrete class. This way, we can create a coffee object without having to write a lot of code. We can create new coffee obejcts just by calling the getCoffee(type, price) method. The output of the code below is as follows:

```
public class HelloWorld{ 
     public static void main(String []args){ 
        Coffee latte = CoffeeFactory.getCoffee("Latte", 5);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 4); 
        System.out.println("Factory latte: "+ latte);
        System.out.println("Factory ame: "+ ame); 
     }
} 
/*
Factory latte: Hi this coffee is 4000
Factory ame: Hi this coffee is 3000
*/
```
Factory pattern is a powerful design pattern that allows you to create objects without specifying the exact class of object that will be created. If you are writing a program that needs to create many objects of the same type, you should consider using the factory pattern.

## Strategy Pattern
Here's a simple example of the strategy pattern in Java: let's say you have a program that needs to perform different operations on data depending on the data's type. For example, you might need to perform addition on numbers, but concatenate strings instead of adding them. You could use the strategy pattern to accomplish this.
```
public interface Operation {
    public int performOperation(int a, int b);
}
```
This interface defines a single method, performOperation, that takes two integer arguments and returns an integer result.

Next, you would create classes that implement this interface for each type of operation you need to perform. For example:

```
public class Addition implements Operation {
    public int performOperation(int a, int b) {
        return a + b;
    }
}

public class Concatenation implements Operation {
    public int performOperation(int a, int b) {
        return Integer.parseInt(Integer.toString(a) + Integer.toString(b));
    }
}
```
In this example, Addition performs addition on two integers, while Concatenation concatenates them as strings and then converts the result back to an integer.

Finally, you would use these classes in your program to perform the appropriate operation based on the data type. For example:

```
public class Calculator {
    private Operation operation;
    
    public void setOperation(Operation operation) {
        this.operation = operation;
    }
    
    public int performOperation(int a, int b) {
        return operation.performOperation(a, b);
    }
}

```
This Calculator class takes an Operation object as input and uses it to perform the appropriate operation on two integers.

Overall, the strategy pattern allows you to define a family of algorithms (in this case, operations on data) and encapsulate each one in a separate class. This makes it easy to switch between algorithms at runtime or reuse them in different contexts.




## Observer Pattern
```import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}

class Topic implements Subject {
    private List<Observer> observers;
    private String message; 

    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    @Override
    public void register(Observer obj) {
        if (!observers.contains(obj)) observers.add(obj); 
    }

    @Override
    public void unregister(Observer obj) {
        observers.remove(obj); 
    }

    @Override
    public void notifyObservers() {   
        this.observers.forEach(Observer::update); 
    }

    @Override
    public Object getUpdate(Observer obj) {
        return this.message;
    } 
    
    public void postMessage(String msg) {
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg; 
        notifyObservers();
    }
}

class TopicSubscriber implements Observer {
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    @Override
    public void update() {
        String msg = (String) topic.getUpdate(this); 
        System.out.println(name + ":: got message >> " + msg); 
    } 
}

public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c); 
   
        topic.postMessage("amumu is op champion!!"); 
    }
}
/*
Message sended to Topic: amumu is op champion!!
a:: got message >> amumu is op champion!!
b:: got message >> amumu is op champion!!
c:: got message >> amumu is op champion!!
*/ 
```
## MCV Pattern


## MCP Pattern

## MVVM Pattern