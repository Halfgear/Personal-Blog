---
title: "Design Patterns"
date: 2023-03-27
draft: false
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

```public class HelloWorld{
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



```public abstract class Coffee {
    //abstract method, so subclass must implement this method 
    public abstract int getPrice(); 
    
    @Override
    public String toString(){
        return "Price: "+ this.getPrice();
    }
}
public class Latte extends Coffee { 
    private int price; 
    
    public Latte(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
public class Americano extends Coffee { 
    private int price; 
    
    public Americano(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
```
```public class CoffeeFactory {
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

```public class HelloWorld{ 
     public static void main(String []args){ 
        Coffee latte = CoffeeFactory.getCoffee("Latte", 5);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 4); 
        System.out.println("Factory latte: "+ latte);
        System.out.println("Factory ame: "+ ame); 
     }
} 
```
output:
```
Factory latte: Price: 4000
Factory ame: Price: 3000
```
Factory pattern is a powerful design pattern that allows you to create objects without specifying the exact class of object that will be created. If you are writing a program that needs to create many objects of the same type, you should consider using the factory pattern.

## Strategy Pattern
Strategy pattern is a behavioral design pattern that allows you to define a family of algorithms, put each of them into a separate class, and make their objects interchangeable. The pattern allows you to change the behavior of an object by changing the object's strategy.

Here's a simple example of the strategy pattern in Java: Suppose you are writing a program that performs some operation on two integers. You would first define an interface that defines the operation:
```
public interface Operation {
    public int performOperation(int a, int b);
}
```
This interface is simply defining a method that takes two integers and returns an integer. You would then create two classes that implement this interface for each type of operation you need to perform. For example:
```public class Addition implements Operation {
    public int performOperation(int a, int b) {
        //1+1 = 2
        return a + b;
    }
}
```
```public class Concatenation implements Operation {
    public int performOperation(int a, int b) {
        //1 cat 1 = 11
        return Integer.parseInt(Integer.toString(a) + Integer.toString(b));
    }
}
```
In these examples, Addition performs addition on two integers, while Concatenation concatenates them as strings and then parses the result back to an integer.

Finally, you would create a class that uses these operations. 
For example:
```public class Calculator {
    private Operation operation;
    
    public void setOperation(Operation operation) {
        this.operation = operation;
    }
    
    public int performOperation(int a, int b) {
        return operation.performOperation(a, b);
    }
}
```
This calculator class has a field called operation that is of type Operation. This field is set to an instance of Addition or Concatenation. The performOperation() method calls the performOperation() method on the operation field. This allows you to change the behavior of the calculator by just changing the operation field.

Overall, Strategy pattern allows you to change the behavior of an object by changing the object's strategy. This is useful when you have multiple algorithms that perform the same task and you want to be able to switch between them at runtime.

## Observer Pattern
Observer Pattern is a behavioral design pattern that allows one or more objects to watch an object and be notified when the object state updates. The observer pattern is also known as the publish/subscribe pattern. This pattern is useful when you have a 1-to-many relationship in objects. For example, if you have a relationship between employees and managers, then you can use the observer pattern to notify managers whenever an employee updates their status. 

This is example code of the observer pattern:

```import java.util.ArrayList;
import java.util.List;

interface Subject {
    public void register(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer {
    public void update(); 
}
```
In this example, we have a Subject interface that defines the methods that must be implemented as a subject. The register() method is used to register an observer object. The notifyObservers() method is used to notify all registered observers. The getUpdate() method is a getter method to obtain the most recent message from a observer.

```class Topic implements Subject {
    //list of observers
    private List<Observer> observers;
    //message sent to the topic
    private String message; 

    //constructor
    public Topic() {
        this.observers = new ArrayList<>();
        this.message = "";
    }

    //method to attach an observer to the subject
    @Override
    public void register(Observer obj) {
        //check if observer is already registered
        if (!observers.contains(obj)) {
            observers.add(obj);
        }
        //else do nothing
    }

    @Override
    public void notifyObservers() {
        //for each loop through all observers and notify them 
        this.observers.forEach(Observer::update); 
    }

    //method to get updates from subject
    @Override
    public String getUpdate(Observer obj) {
        //get the message from the observer
        return this.message;
    } 
    
    //method to post message to the topic
    public void postMessage(String msg) {
        //set the message
        this.message = msg;

        //notify all observers to post message
        notifyObservers();
    }
}
```
```class TopicSubscriber implements Observer {
    //name of the observer
    private String name;
    private Subject topic;

    //constructor
    public TopicSubscriber(String name, Subject topic) {
        this.name = name;
        this.topic = topic;
    }

    //method to print out the update. used by Subject class.
    @Override
    public void update() {
        String msg = topic.getUpdate(this); 
        System.out.println(name + " >> " + msg); 
    } 
}
```
In the example, we have a Topic class that creates a topic that observers can subscribe to. The TopicSubscriber class creates an observer that can subscribe to a topic. 

Finally, we have a main class that creates a topic and two observers: a and b. The observers are then registered to the topic. The topic then posts a message, which notifies all observers to print out the message.

```public class HelloWorld { 
    public static void main(String[] args) {
        Topic topic = new Topic(); 
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        topic.register(a);
        topic.register(b);
   
        topic.postMessage("Hello World"); 
    }
}
```
output:
```
a >> Hello World
b >> Hello World
```
In this example, the topic is the subject and the observers are the objects that are watching the subject. The observers are notified whenever the subject updates its state. It demonstarted how the observer pattern can create a 1-to-many relationship between objects. Object pattern is used in many applications, such as in the Java Swing library to build Graphic User Interface.

## Conclusion
In this post, we learned about the 4 common design patterns: Singleton pattern, the Factory pattern, the Strategy pattern, and the Observer pattern. A wise use of these patterns will create more flexible and reusable code for your program (they are very useful for OOD class as well). I will be writing more posts about design patterns such as Adaptor and MCV in the future, so stay tuned!