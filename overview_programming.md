# Patterns by Type

### Creational

Creational patterns are ones that create objects, rather than having to instantiate objects directly. This gives the program more flexibility in deciding which objects need to be created for a given case.

- **Abstract factory** groups object factories that have a common theme.
- **Builder** constructs complex objects by separating construction and representation.
- **Factory method** creates objects without specifying the exact class to create.
- **Prototype** creates objects by cloning an existing object.
- **Singleton** restricts object creation for a class to only one instance.

### Structural

These concern class and object composition. They use inheritance to compose interfaces and define ways to compose objects to obtain new functionality.

- **Adapter** allows classes with incompatible interfaces to work together by wrapping its own interface around that of an already existing class.
- **Bridge** decouples an abstraction from its implementation so that the two can vary independently.
- **Composite** composes zero-or-more similar objects so that they can be manipulated as one object.
- **Decorator** dynamically adds/overrides behaviour in an existing method of an object.
- **Facade** provides a simplified interface to a large body of code.
- **Flyweight** reduces the cost of creating and manipulating a large number of similar objects.
- **Proxy** provides a placeholder for another object to control access, reduce cost, and reduce complexity.

### Behavioral

Most of these design patterns are specifically concerned with communication between objects.

- **Chain of responsibility** delegates commands to a chain of processing objects.
- **Command** creates objects which encapsulate actions and parameters.
- **Interpreter** implements a specialized language.
- **Iterator** accesses the elements of an object sequentially without exposing its underlying representation.
- **Mediator** allows loose coupling between classes by being the only class that has detailed knowledge of their methods.
- **Memento** provides the ability to restore an object to its previous state (undo).
- **Observer** is a publish/subscribe pattern which allows a number of observer objects to see an event.
- **State** allows an object to alter its behavior when its internal state changes.
- **Strategy** allows one of a family of algorithms to be selected on-the-fly at runtime.
- **Template method** defines the skeleton of an algorithm as an abstract class, allowing its subclasses to provide concrete behavior.
- **Visitor** separates an algorithm from an object structure by moving the hierarchy of methods into one object.

# The Most Important Design Patterns

### Singleton

Singleton pattern is one of the simplest design patterns in Java. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

This pattern involves a single class which is responsible to create an object while making sure that only single object gets created. This class provides a way to access its only object which can be accessed directly without need to instantiate the object of the class.

#### Implementation

We're going to create a *SingleObject* class. *SingleObject* class have its constructor as private and have a static instance of itself.

*SingleObject* class provides a static method to get its static instance to outside world. *SingletonPatternDemo*, our demo class will use *SingleObject* class to get a *SingleObject* object.

![](https://www.tutorialspoint.com/design_pattern/images/singleton_pattern_uml_diagram.jpg)

##### Step 1

Create a Singleton Class.

***SingleObject.java***

```java
public class SingleObject {

   //create an object of SingleObject
   private static SingleObject instance = new SingleObject();

   //make the constructor private so that this class cannot be
   //instantiated
   private SingleObject(){}

   //Get the only object available
   public static SingleObject getInstance(){
      return instance;
   }

   public void showMessage(){
      System.out.println("Hello World!");
   }
}
```

##### Step 2

Get the only object from the singleton class.

***SingletonPatternDemo.java***

```java
public class SingletonPatternDemo {
   public static void main(String[] args) {

      //illegal construct
      //Compile Time Error: The constructor SingleObject() is not visible
      //SingleObject object = new SingleObject();

      //Get the only object available
      SingleObject object = SingleObject.getInstance();

      //show the message
      object.showMessage();
   }
}
```

##### Step 3

Verify the output.

```
Hello World!
```

The singleton pattern is used to limit creation of a class to only one object. This is beneficial when one (and only one) object is needed to coordinate actions across the system. There are several examples of where only a single instance of a class should exist, including caches, thread pools, and registries.

It’s trivial to initiate an object of a class — but how do we ensure that only one object ever gets created? The answer is to make the constructor ‘private’ to the class we intend to define as a singleton. That way, only the members of the class can access the private constructor and no one else.

> **Important consideration:** It’s possible to subclass a singleton by making the constructor protected instead of private. This might be suitable under some circumstances. One approach taken in these scenarios is to create a register of singletons of the subclasses and the getInstance method can take in a parameter or use an environment variable to return the desired singleton. The registry then maintains a mapping of string names to singleton objects, which can be accessed as needed.

### Factory Method

Factory pattern is one of the most used design patterns in Java. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

In Factory pattern, we create object without exposing the creation logic to the client and refer to newly created object using a common interface.

#### Implementation

We're going to create a *Shape* interface and concrete classes implementing the *Shape* interface. A factory class *ShapeFactory* is defined as a next step.

*FactoryPatternDemo*, our demo class will use *ShapeFactory* to get a *Shape* object. It will pass information (*CIRCLE / RECTANGLE / SQUARE*) to *ShapeFactory* to get the type of object it needs.

![](https://www.tutorialspoint.com/design_pattern/images/factory_pattern_uml_diagram.jpg)

##### Step 1

Create an interface.

***Shape.java***

```java
public interface Shape {
   void draw();
}
```

##### Step 2

Create concrete classes implementing the same interface.

***Rectangle.java***

```java
public class Rectangle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}
```

***Square.java**

```java
public class Square implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Square::draw() method.");
   }
}
```

***Circle.java***

```java
public class Circle implements Shape {

   @Override
   public void draw() {
      System.out.println("Inside Circle::draw() method.");
   }
}
```

##### Step 3

Create a Factory to generate object of concrete class based on given information.

***ShapeFactory.java***

```java
public class ShapeFactory {
	
   //use getShape method to get object of type shape 
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }		
      if(shapeType.equalsIgnoreCase("CIRCLE")){
         return new Circle();
         
      } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
         
      } else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      
      return null;
   }
}
```

##### Step 4

Use the Factory to get object of concrete class by passing an information such as type.

***FactoryPatternDemo.java***

```java
public class FactoryPatternDemo {

   public static void main(String[] args) {
      ShapeFactory shapeFactory = new ShapeFactory();

      //get an object of Circle and call its draw method.
      Shape shape1 = shapeFactory.getShape("CIRCLE");

      //call draw method of Circle
      shape1.draw();

      //get an object of Rectangle and call its draw method.
      Shape shape2 = shapeFactory.getShape("RECTANGLE");

      //call draw method of Rectangle
      shape2.draw();

      //get an object of Square and call its draw method.
      Shape shape3 = shapeFactory.getShape("SQUARE");

      //call draw method of square
      shape3.draw();
   }
}
```

##### Step 5

Verify the output.

````
Inside Circle::draw() method.
Inside Rectangle::draw() method.
Inside Square::draw() method.
````

A normal factory produces goods; a software factory produces objects. And not just that — it does so without specifying the exact class of the object to be created. To accomplish this, objects are created by calling a factory method instead of calling a constructor.

Usually, object creation in Java takes place like so:

```java
SomeClass someClassObject = new SomeClass();
```

The problem with the above approach is that the code using the SomeClass’s object, suddenly now becomes dependent on the concrete implementation of SomeClass. There’s nothing wrong with using new to create objects but it comes with the baggage of tightly coupling our code to the concrete implementation class, which can occasionally be problematic.

### Strategy

In Strategy pattern, a class behavior or its algorithm can be changed at run time. This type of design pattern comes under behavior pattern.

In Strategy pattern, we create objects which represent various strategies and a context object whose behavior varies as per its strategy object. The strategy object changes the executing algorithm of the context object.

Implementation

We are going to create a *Strategy* interface defining an action and concrete strategy classes implementing the *Strategy* interface. *Context* is a class which uses a Strategy.

*StrategyPatternDemo*, our demo class, will use *Context* and strategy objects to demonstrate change in Context behaviour based on strategy it deploys or uses.

![](https://www.tutorialspoint.com/design_pattern/images/strategy_pattern_uml_diagram.jpg)

##### Step 1

Create an interface.

***Strategy.java***

```java
public interface Strategy {
   public int doOperation(int num1, int num2);
}
```

##### Step 2

Create concrete classes implementing the same interface.

***OperationAdd.java***

```java
public class OperationAdd implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 + num2;
   }
}
```

***OperationSubstract.java***

```java
public class OperationSubstract implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 - num2;
   }
}
```

***OperationMultiply.java***

```java
public class OperationMultiply implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 * num2;
   }
}
```

##### Step 3

Create Context Class.

***Context.java***

```java
public class Context {
   private Strategy strategy;

   public Context(Strategy strategy){
      this.strategy = strategy;
   }

   public int executeStrategy(int num1, int num2){
      return strategy.doOperation(num1, num2);
   }
}
```

##### Step 4

Use the Context to see change in behaviour when it changes its Strategy.

***StrategyPatternDemo.java**

```java
public class StrategyPatternDemo {
   public static void main(String[] args) {
      Context context = new Context(new OperationAdd());		
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationSubstract());		
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationMultiply());		
      System.out.println("10 * 5 = " + context.executeStrategy(10, 5));
   }
}
```

##### Step 5

Verify the output.

```
10 + 5 = 15
10 - 5 = 5
10 * 5 = 50
```

The strategy pattern allows grouping related algorithms under an abstraction, which allows switching out one algorithm or policy for another without modifying the client. Instead of directly implementing a single algorithm, the code receives runtime instructions specifying which of the group of algorithms to run.

### Observer

Observer pattern is used when there is one-to-many relationship between objects such as if one object is modified, its depenedent objects are to be notified automatically. Observer pattern falls under behavioral pattern category.

#### Implementation

Observer pattern uses three actor classes. Subject, Observer and Client. Subject is an object having methods to attach and detach observers to a client object. We have created an abstract class *Observer* and a concrete class *Subject* that is extending class Observer.

*ObserverPatternDemo*, our demo class, will use *Subject* and concrete class object to show observer pattern in action.

![](https://www.tutorialspoint.com/design_pattern/images/observer_pattern_uml_diagram.jpg)

##### Step 1

Create Subject class.

***Subject.java***

```java
import java.util.ArrayList;
import java.util.List;

public class Subject {
	
   private List<Observer> observers = new ArrayList<Observer>();
   private int state;

   public int getState() {
      return state;
   }

   public void setState(int state) {
      this.state = state;
      notifyAllObservers();
   }

   public void attach(Observer observer){
      observers.add(observer);		
   }

   public void notifyAllObservers(){
      for (Observer observer : observers) {
         observer.update();
      }
   } 	
}
```

##### Step 2

Create Observer class.

***Observer.java***

```java
public abstract class Observer {
   protected Subject subject;
   public abstract void update();
}
```

##### Step 3

Create concrete observer classes

***BinaryObserver.java***

```java
public class BinaryObserver extends Observer{

   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Binary String: " + Integer.toBinaryString( subject.getState() ) ); 
   }
}
```

***OctalObserver.java***

```java
public class OctalObserver extends Observer{

   public OctalObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
     System.out.println( "Octal String: " + Integer.toOctalString( subject.getState() ) ); 
   }
}
```

***HexaObserver.java***

```java
public class HexaObserver extends Observer{

   public HexaObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }

   @Override
   public void update() {
      System.out.println( "Hex String: " + Integer.toHexString( subject.getState() ).toUpperCase() ); 
   }
}
```

##### Step 4

Use Subject and concrete observer objects.

***ObserverPatternDemo.java***

```java
public class ObserverPatternDemo {
   public static void main(String[] args) {
      Subject subject = new Subject();

      new HexaObserver(subject);
      new OctalObserver(subject);
      new BinaryObserver(subject);

      System.out.println("First state change: 15");	
      subject.setState(15);
      System.out.println("Second state change: 10");	
      subject.setState(10);
   }
}
```

##### Step 5

Verify the output.

```
First state change: 15
Hex String: F
Octal String: 17
Bnary String: 1111
Second state change: 10
Hex String: A
Octal String: 12
Binary String: 1010
```

This pattern is a one-to-many dependency between objects so that when one object changes state, all its dependents are notified. This is typically done by calling one of their methods.

For the sake of simplicity, think about what happens when you follow someone on Twitter. You are essentially asking Twitter to send you (the observer) tweet updates of the person (the subject) you followed. The pattern consists of two actors, the observer who is interested in the updates and the subject who generates the updates.

A subject can have many observers and is a one to many relationship. However, an observer is free to subscribe to updates from other subjects too. You can subscribe to news feed from a Facebook page, which would be the subject and whenever the page has a new post, the subscriber would see the new post.

Key consideration: In case of many subjects and few observers, if each subject stores its observers separately, it’ll increase the storage costs as some subjects will be storing the same observer multiple times.

### Builder

Builder pattern builds a complex object using simple objects and using a step by step approach. This type of design pattern comes under creational pattern as this pattern provides one of the best ways to create an object.

A Builder class builds the final object step by step. This builder is independent of other objects.

#### Implementation

We have considered a business case of fast-food restaurant where a typical meal could be a burger and a cold drink. Burger could be either a Veg Burger or Chicken Burger and will be packed by a wrapper. Cold drink could be either a coke or pepsi and will be packed in a bottle.

We are going to create an *Item* interface representing food items such as burgers and cold drinks and concrete classes implementing the *Item* interface and a *Packing* interface representing packaging of food items and concrete classes implementing the *Packing* interface as burger would be packed in wrapper and cold drink would be packed as bottle.

We then create a *Meal* class having *ArrayList* of *Item* and a *MealBuilder* to build different types of *Meal* objects by combining *Item*. *BuilderPatternDemo*, our demo class will use *MealBuilder* to build a *Meal*.

![](https://www.tutorialspoint.com/design_pattern/images/builder_pattern_uml_diagram.jpg)

##### Step 1

Create an interface Item representing food item and packing.

***Item.java***

```java
public interface Item {
   public String name();
   public Packing packing();
   public float price();	
}
```

***Packing.java***

```java
public interface Packing {
   public String pack();
}
```

##### Step 2

Create concrete classes implementing the Packing interface.

***Wrapper.java***

```java
public class Wrapper implements Packing {

   @Override
   public String pack() {
      return "Wrapper";
   }
}
```

***Bottle.java***

```java
public class Bottle implements Packing {

   @Override
   public String pack() {
      return "Bottle";
   }
}
```

##### Step 3

Create abstract classes implementing the item interface providing default functionalities.

***Burger.java***

```java
public abstract class Burger implements Item {

   @Override
   public Packing packing() {
      return new Wrapper();
   }

   @Override
   public abstract float price();
}
```

***ColdDrink.java***

```java
public abstract class ColdDrink implements Item {

	@Override
	public Packing packing() {
       return new Bottle();
	}

	@Override
	public abstract float price();
}
```

##### Step 4

Create concrete classes extending Burger and ColdDrink classes

***VegBurger.java***

```java
public class VegBurger extends Burger {

   @Override
   public float price() {
      return 25.0f;
   }

   @Override
   public String name() {
      return "Veg Burger";
   }
}
```

***ChickenBurger.java***

```java
public class ChickenBurger extends Burger {

   @Override
   public float price() {
      return 50.5f;
   }

   @Override
   public String name() {
      return "Chicken Burger";
   }
}
```

***Coke.java***

```java
public class Coke extends ColdDrink {

   @Override
   public float price() {
      return 30.0f;
   }

   @Override
   public String name() {
      return "Coke";
   }
}
```

***Pepsi.java***

```java
public class Pepsi extends ColdDrink {

   @Override
   public float price() {
      return 35.0f;
   }

   @Override
   public String name() {
      return "Pepsi";
   }
}
```

##### Step 5

Create a Meal class having Item objects defined above.

***Meal.java***

```java
import java.util.ArrayList;
import java.util.List;

public class Meal {
   private List<Item> items = new ArrayList<Item>();	

   public void addItem(Item item){
      items.add(item);
   }

   public float getCost(){
      float cost = 0.0f;
      
      for (Item item : items) {
         cost += item.price();
      }		
      return cost;
   }

   public void showItems(){
   
      for (Item item : items) {
         System.out.print("Item : " + item.name());
         System.out.print(", Packing : " + item.packing().pack());
         System.out.println(", Price : " + item.price());
      }		
   }	
}
```

##### Step 6

Create a MealBuilder class, the actual builder class responsible to create Meal objects.

***MealBuilder.java***

```java
public class MealBuilder {

   public Meal prepareVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new VegBurger());
      meal.addItem(new Coke());
      return meal;
   }   

   public Meal prepareNonVegMeal (){
      Meal meal = new Meal();
      meal.addItem(new ChickenBurger());
      meal.addItem(new Pepsi());
      return meal;
   }
}
```

##### Step 7

BuiderPatternDemo uses MealBuider to demonstrate builder pattern.

***BuilderPatternDemo.java***

```java
public class BuilderPatternDemo {
   public static void main(String[] args) {
   
      MealBuilder mealBuilder = new MealBuilder();

      Meal vegMeal = mealBuilder.prepareVegMeal();
      System.out.println("Veg Meal");
      vegMeal.showItems();
      System.out.println("Total Cost: " + vegMeal.getCost());

      Meal nonVegMeal = mealBuilder.prepareNonVegMeal();
      System.out.println("\n\nNon-Veg Meal");
      nonVegMeal.showItems();
      System.out.println("Total Cost: " + nonVegMeal.getCost());
   }
}
```

##### Step 8

Verify the output.

```
Veg Meal
Item : Veg Burger, Packing : Wrapper, Price : 25.0
Item : Coke, Packing : Bottle, Price : 30.0
Total Cost: 55.0


Non-Veg Meal
Item : Chicken Burger, Packing : Wrapper, Price : 50.5
Item : Pepsi, Packing : Bottle, Price : 35.0
Total Cost: 85.5
```

As the name implies, a builder pattern is used to build objects. Sometimes, the objects we create can be complex, made up of several sub-objects or require an elaborate construction process. The exercise of creating complex types can be simplified by using the builder pattern. A composite or an aggregate object is what a builder generally builds.

Key consideration: The builder pattern might seem similar to the ‘abstract factory’ pattern but one difference is that the builder pattern creates an object step by step whereas the abstract factory pattern returns the object in one go.

### Adapter

Adapter pattern works as a bridge between two incompatible interfaces. This type of design pattern comes under structural pattern as this pattern combines the capability of two independent interfaces.

This pattern involves a single class which is responsible to join functionalities of independent or incompatible interfaces. A real life example could be a case of card reader which acts as an adapter between memory card and a laptop. You plugin the memory card into card reader and card reader into the laptop so that memory card can be read via laptop.

We are demonstrating use of Adapter pattern via following example in which an audio player device can play mp3 files only and wants to use an advanced audio player capable of playing vlc and mp4 files. 

#### Implementation

We have a *MediaPlayer* interface and a concrete class *AudioPlayer* implementing the *MediaPlayer* interface. *AudioPlayer* can play mp3 format audio files by default.

We are having another interface *AdvancedMediaPlayer* and concrete classes implementing the *AdvancedMediaPlayer* interface. These classes can play vlc and mp4 format files.

We want to make *AudioPlayer* to play other formats as well. To attain this, we have created an adapter class *MediaAdapter* which implements the *MediaPlayer* interface and uses *AdvancedMediaPlayer* objects to play the required format.

*AudioPlayer* uses the adapter class *MediaAdapter* passing it the desired audio type without knowing the actual class which can play the desired format. *AdapterPatternDemo*, our demo class will use *AudioPlayer* class to play various formats.

![](https://www.tutorialspoint.com/design_pattern/images/adapter_pattern_uml_diagram.jpg)

##### Step 1

Create interfaces for Media Player and Advanced Media Player.

***MediaPlayer.java***

```java
public interface MediaPlayer {
   public void play(String audioType, String fileName);
}
```

***AdvancedMediaPlayer.java***

```java
public interface AdvancedMediaPlayer {	
   public void playVlc(String fileName);
   public void playMp4(String fileName);
}
```

##### Step 2

Create concrete classes implementing the AdvancedMediaPlayer interface.

***VlcPlayer.java***

```java
public class VlcPlayer implements AdvancedMediaPlayer{
   @Override
   public void playVlc(String fileName) {
      System.out.println("Playing vlc file. Name: "+ fileName);		
   }

   @Override
   public void playMp4(String fileName) {
      //do nothing
   }
}
```

***Mp4Player.java***

```java
public class Mp4Player implements AdvancedMediaPlayer{

   @Override
   public void playVlc(String fileName) {
      //do nothing
   }

   @Override
   public void playMp4(String fileName) {
      System.out.println("Playing mp4 file. Name: "+ fileName);		
   }
}
```

##### Step 3

Create adapter class implementing the MediaPlayer interface.

***MediaAdapter.java***

```java
public class MediaAdapter implements MediaPlayer {

   AdvancedMediaPlayer advancedMusicPlayer;

   public MediaAdapter(String audioType){
   
      if(audioType.equalsIgnoreCase("vlc") ){
         advancedMusicPlayer = new VlcPlayer();			
         
      }else if (audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer = new Mp4Player();
      }	
   }

   @Override
   public void play(String audioType, String fileName) {
   
      if(audioType.equalsIgnoreCase("vlc")){
         advancedMusicPlayer.playVlc(fileName);
      }
      else if(audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer.playMp4(fileName);
      }
   }
}
```

##### Step 4

Create concrete class implementing the MediaPlayer interface.

***AudioPlayer.java***

```java
public class AudioPlayer implements MediaPlayer {
   MediaAdapter mediaAdapter; 

   @Override
   public void play(String audioType, String fileName) {		

      //inbuilt support to play mp3 music files
      if(audioType.equalsIgnoreCase("mp3")){
         System.out.println("Playing mp3 file. Name: " + fileName);			
      } 
      
      //mediaAdapter is providing support to play other file formats
      else if(audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")){
         mediaAdapter = new MediaAdapter(audioType);
         mediaAdapter.play(audioType, fileName);
      }
      
      else{
         System.out.println("Invalid media. " + audioType + " format not supported");
      }
   }   
}
```

##### Step 5

Use the AudioPlayer to play different types of audio formats.

***AdapterPatternDemo.java***

```java
public class AdapterPatternDemo {
   public static void main(String[] args) {
      AudioPlayer audioPlayer = new AudioPlayer();

      audioPlayer.play("mp3", "beyond the horizon.mp3");
      audioPlayer.play("mp4", "alone.mp4");
      audioPlayer.play("vlc", "far far away.vlc");
      audioPlayer.play("avi", "mind me.avi");
   }
}
```

##### Step 6

Verify the output.

```
Playing mp3 file. Name: beyond the horizon.mp3
Playing mp4 file. Name: alone.mp4
Playing vlc file. Name: far far away.vlc
Invalid media. avi format not supported
```

This allows incompatible classes to work together by converting the interface of one class into another. Think of it as a sort of translator: when two heads of states who don’t speak a common language meet, usually an interpreter sits between the two and translates the conversation, thus enabling communication.

If you have two applications, with one spitting out output as XML with the other requiring JSON input, then you’ll need an adapter between the two to make them work seamlessly.

### State

In State pattern a class behavior changes based on its state. This type of design pattern comes under behavior pattern.

In State pattern, we create objects which represent various states and a context object whose behavior varies as its state object changes.

#### Implementation

We are going to create a *State* interface defining an action and concrete state classes implementing the *State* interface. *Context* is a class which carries a State.

*StatePatternDemo*, our demo class, will use *Context* and state objects to demonstrate change in Context behavior based on type of state it is in.

![](https://www.tutorialspoint.com/design_pattern/images/state_pattern_uml_diagram.jpg)

##### Step 1

Create an interface.

***State.java***

```java
public interface State {
   public void doAction(Context context);
}
```

##### Step 2

Create concrete classes implementing the same interface.

***StartState.java***

```java
public class StartState implements State {

   public void doAction(Context context) {
      System.out.println("Player is in start state");
      context.setState(this);	
   }

   public String toString(){
      return "Start State";
   }
}
```

***StopState.java***

```java
public class StopState implements State {

   public void doAction(Context context) {
      System.out.println("Player is in stop state");
      context.setState(this);	
   }

   public String toString(){
      return "Stop State";
   }
}
```

##### Step 3

Create Context Class.

***Context.java***

```java
public class Context {
   private State state;

   public Context(){
      state = null;
   }

   public void setState(State state){
      this.state = state;		
   }

   public State getState(){
      return state;
   }
}
```

##### Step 4

Use the Context to see change in behaviour when State changes.

***StatePatternDemo.java***

```java
public class StatePatternDemo {
   public static void main(String[] args) {
      Context context = new Context();

      StartState startState = new StartState();
      startState.doAction(context);

      System.out.println(context.getState().toString());

      StopState stopState = new StopState();
      stopState.doAction(context);

      System.out.println(context.getState().toString());
   }
}
```

##### Step 5

Verify the output.

```
Player is in start state
Start State
Player is in stop state
Stop State
```

The state pattern encapsulates the various states a machine can be in, and allows an object to alter its behavior when its internal state changes. The machine or the context, as it is called in pattern-speak, can have actions taken on it that propel it into different states. Without the use of the pattern, the code becomes inflexible and littered with if-else conditionals.

# KISS 

## What does KISS stand for?

The KISS is an abbreviation of Keep It Stupid Simple or Keep It Simple, Stupid

## What does that mean?

This principle has been a key, and a huge success in my years of software engineering. A common problem among software engineers and developers today is that they tend to over complicate problems.

Typically when a developer is faced with a problem, they break it down into smaller pieces that they think they understand and then try to implement the solution in code. I would say 8 or 9 out of 10 developers make the mistake that they don't break down the problem into small enough or understandable enough pieces. This results in very complex implementations of even the most simple problems, another side effect is spagetthi code, something we tought only BASIC would do with its goto statements, but in Java this results in classes with 500-1000 lines of code, methods that each have several hundreds of lines.

This code clutter is a result of the developer realizing exception cases to his original solution while he is typing in code. These exception cases would have solved if the developer had broken down the problem further. 

## How will I benefit from KISS

- You will be able to solve more problems, faster.
- You will be able to produce code to solve complex problems in fewer lines of code
- You will be able to produce higher quality code
- You will be able to build larger systems, easier to maintain
- You're code base will be more flexible, easier to extend, modify or refactor when new requirements arrive
- You will be able to achieve more than you ever imagined

- You will be able to work in large development groups and large projects since all the code is stupid simple

## How can I apply the KISS principle to my work

There are several steps to take, very simple, but could be challenging for some. As easy as it sounds, keeping it simple, is a matter of patience, mostly with yourself.

- Be Humble, don't think of yourself as a super genius, this is your first mistake
- By being humble, you will eventually achieve super genius status =), and even if you don't, who cares! your code is stupid simple, so you don't have to be a genius to work with it.
- Break down your tasks into sub tasks that you think should take no longer than 4-12 hours to code
- Break down your problems into many small problems. Each problem should be able to be solved within one or a very few classes
- Keep your methods small, each method should never be more than 30-40 lines. Each method should only solve one little problem, not many uses cases
- If you have a lot of conditions in your method, break these out into smaller methods.
- Not only will this be easier to read and maintain, but you will find bugs a lot faster.
- You will learn to love Right ```Click+Refactor``` in your editor.
- Keep your classes small, same methodology applies here as we described for methods.
- Solve the problem, then code it. Not the other way around
- Many developers solve their problem while they are coding, and there is nothing wrong doing that. As a matter of fact, you can do that and still adhere to the above statement.
- If you have the ability to mentally break down things into very small pieces, then by all means, do that while you are coding. But don't be afraid of refactor your code over and over and over and over again. Its the end result that counts, and number of lines is not a measurement, unless you measure that fewer is better of course.
- Don't be afraid to throw away code. Refactoring and recoding are two very important areas. As you come across requirements that didn't exist, or you weren't aware of when you wrote the code to begin with you might be able to solve the old and the new problems with an even better solution.
- If you had followed the advice above, the amount of code to rewrite would have been minimal, and if you hadn't followed the advice above, then the code should probably be rewritten anyway.

- And for all other scenarios, try to keep it as simple as possible, this is the hardest behavior pattern to apply to, but once you have it, you'll look back and will say, I can't imagine how I was doing work before.

## Are there any examples of the KISS principle

There are many, and I will look for some really great one to post here. But I will leave you with the following thought:

Some of the world's greatest algorithms are always the ones with the fewest lines of code. And when we go through the lines of code, we can easily understand them. The innovator of that algorithm, broke down the problem until it was so easy to understand that he/she could implement it.

**Many great problem solvers were not great coders, but yet they produced great code!**

## Does KISS only apply to java coding

Absolutely not, it applies to many other programming languages and extends to many other areas in your life.

The areas that the principle doesn't apply to are: emotions, love and most importantly, your marriage :) 

# YAGNI

YAGNI, or “You Ain’t Gonna Need It” (or “You Aren’t Gonna Need It”), emerged as one of the key principles of Extreme Programming.  Put another way, the principle states:

“Always implement things when you actually need them, never when you just foresee that you may need them.”

There are a number of reasons why this principle exists.  Firstly, it maximizes the amount of unnecessary work that is left undone, which is a great way to improve developer productivity and product simplicity.  Remember, features are expensive, both to develop and maintain, and for users to learn and navigate around.  Features that aren’t actually necessary are a huge source of waste.

In some ways, you can think of YAGNI as being similar to Just-In-Time manufacturing.  Rather than ordering a bunch of parts based on what you think you might need, wait for actual customer orders and ensure your process is lean enough that you can pull orders through you supply chain quickly enough to satisfy your customers.  Now, just as JIT only works well in manufacturing organizations that are disciplined enough to follow lean principles, the case can be made that YAGNI works best in environments that ensure their code is kept in a malleable, changeable state.  If, in the future, adding new functionality will be difficult because the code has turned into a steaming pile of Spaghetti Code, then you might be able to argue that adding the feature before the code gets to that state would be a good practice.

It follows that YAGNI is closely related to te Keep It Simple, Stupid principle.  By avoiding adding features and complexity until it’s actually needed, the overall design of the system can remain simpler, longer.  And that feature you thought you would need?  Well, usually it it turns out that you either didn’t actually need it, or that when you do need it, your understanding of how best to design it will be better than if you’d tried to divine how it would be used earlier in the project.

## Quotes

> Every line of code we don’t write is dollars we didn’t spend, and time on the calendar we get back for free. – Tim Evans-Ariyeh

> The cheapest, fastest, and most reliable components of a computer system are those that aren’t there. – Gordon Bell

# DRY

Don't repeat yourself (DRY, or sometimes do not repeat yourself) is a principle of software development aimed at reducing repetition of software patterns, replacing it with abstractions or using data normalization to avoid redundancy.

The DRY principle is stated as "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system". The principle has been formulated by Andy Hunt and Dave Thomas in their book The Pragmatic Programmer. They apply it quite broadly to include "database schemas, test plans, the build system, even documentation". When the DRY principle is applied successfully, a modification of any single element of a system does not require a change in other logically unrelated elements. Additionally, elements that are logically related all change predictably and uniformly, and are thus kept in sync. Besides using methods and subroutines in their code, Thomas and Hunt rely on code generators, automatic build systems, and scripting languages to observe the DRY principle across layers. 

# SOLID

In object-oriented computer programming, SOLID is a mnemonic acronym for five design principles intended to make software designs more understandable, flexible and maintainable. It is not related to the GRASP software design principles. The principles are a subset of many principles promoted by American software engineer and instructor Robert C. Martin. Though they apply to any object-oriented design, the SOLID principles can also form a core philosophy for methodologies such as agile development or adaptive software development. The theory of SOLID principles was introduced by Martin in his 2000 paper Design Principles and Design Patterns, although the SOLID acronym was introduced later by Michael Feathers.

## Concepts

### Single-responsibility principle
> A class should only have a single responsibility, that is, only changes to one part of the software's specification should be able to affect the specification of the class.

The single-responsibility principle (SRP) is a computer-programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class, module or function. All its services should be narrowly aligned with that responsibility. Robert C. Martin expresses the principle as, "A class should have only one reason to change," although, because of confusion around the word "reason" he more recently stated "This principle is about people."

#### Example

Martin defines a responsibility as a reason to change, and concludes that a class or module should have one, and only one, reason to be changed (i.e. rewritten). As an example, consider a module that compiles and prints a report. Imagine such a module can be changed for two reasons. First, the content of the report could change. Second, the format of the report could change. These two things change for very different causes; one substantive, and one cosmetic. The single-responsibility principle says that these two aspects of the problem are really two separate responsibilities, and should therefore be in separate classes or modules. It would be a bad design to couple two things that change for different reasons at different times.

The reason it is important to keep a class focused on a single concern is that it makes the class more robust. Continuing with the foregoing example, if there is a change to the report compilation process, there is greater danger that the printing code will break if it is part of the same class. 

### Open–closed principle
> "Software entities ... should be open for extension, but closed for modification."

In object-oriented programming, the open/closed principle states "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification"; that is, such an entity can allow its behaviour to be extended without modifying its source code.

The name open/closed principle has been used in two ways. Both ways use generalizations (for instance, inheritance or delegate functions) to resolve the apparent dilemma, but the goals, techniques, and results are different.

Open-closed principle is one of the five SOLID principles of object-oriented design. 

     

#### Meyer's open/closed principle

Bertrand Meyer is generally credited for having originated the term open/closed principle, which appeared in his 1988 book Object Oriented Software Construction.

- A module will be said to be open if it is still available for extension. For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.

- A module will be said to be closed if [it] is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).

At the time Meyer was writing, adding fields or functions to a library inevitably required changes to any programs depending on that library. Meyer's proposed solution to this dilemma relied on the notion of object-oriented inheritance (specifically implementation inheritance):

A class is closed, since it may be compiled, stored in a library, baselined, and used by client classes. But it is also open, since any new class may use it as parent, adding new features. When a descendant class is defined, there is no need to change the original or to disturb its clients.

#### Polymorphic open/closed principle

During the 1990s, the open/closed principle became popularly redefined to refer to the use of abstracted interfaces, where the implementations can be changed and multiple implementations could be created and polymorphically substituted for each other.

In contrast to Meyer's usage, this definition advocates inheritance from abstract base classes. Interface specifications can be reused through inheritance but implementation need not be. The existing interface is closed to modifications and new implementations must, at a minimum, implement that interface.

Robert C. Martin's 1996 article "The Open-Closed Principle" was one of the seminal writings to take this approach. In 2001 Craig Larman related the open/closed principle to the pattern by Alistair Cockburn called Protected Variations, and to the David Parnas discussion of information hiding.

### Liskov substitution principle
> "Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program." See also design by contract.

#### Behavioral subtyping

In object-oriented programming, behavioral subtyping is the principle that subclasses should satisfy the expectations of clients accessing subclass objects through references of superclass type, not just as regards syntactic safety (absence of method-not-found errors and such) but also as regards behavioral correctness. Specifically, properties that clients can prove using the specification of an object's presumed type should hold even though the object is actually a member of a subtype of that type.

For example, consider a type Stack and a type Queue, that both have a put method to add an element and a get method to remove one. Suppose the documentation associated with these types specifies that type Stack's methods shall behave as expected for stacks (i.e. they shall exhibit LIFO behavior), and that type Queue's methods shall behave as expected for queues (i.e. they shall exhibit FIFO behavior). Suppose, now, that type Stack were declared as a subclass of type Queue. Most programming language compilers ignore documentation and perform only the checks that are necessary to preserve type safety. Since, for each method of type Queue, type Stack provides a method with a matching name and signature, this check would succeed. However, clients accessing a Stack object through a reference of type Queue would, based on Queue's documentation, expect FIFO behavior but observe LIFO behavior, invalidating these clients' correctness proofs and potentially leading to incorrect behavior of the program as a whole.

This example violates behavioral subtyping because type Stack is not a behavioral subtype of type Queue: it is not the case that the behaviors allowed by Stack are also allowed by Queue.

In contrast, a program where both Stack and Queue are subclasses of a type Bag, whose specification for get is merely that it removes some element, does satisfy behavioral subtyping and allows clients to safely reason about correctness based on the presumed types of the objects they interact with. Indeed, any object that satisfies the Stack or Queue specification also satisfies the Bag specification.

It is important to stress that whether a type S is a behavioral subtype of a type T depends only on the specification of type T; the implementation of type T, if it has any, is completely irrelevant to this question. Indeed, type T need not even have an implementation; it might be a purely abstract class. As another case in point, type Stack above is a behavioral subtype of type Bag even if type Bag's implementation exhibits FIFO behavior: what matters is that type Bag's specification does not specify which element is removed by method get. This also means that behavioral subtyping can be discussed only with respect to a particular (behavioral) specification for each type involved, and that if the types involved have no well-defined behavioral specification, behavioral subtyping cannot be discussed meaningfully. 

### Interface segregation principle
> "Many client-specific interfaces are better than one general-purpose interface."

In the field of software engineering, the interface-segregation principle (ISP) states that no client should be forced to depend on methods it does not use. ISP splits interfaces that are very large into smaller and more specific ones so that clients will only have to know about the methods that are of interest to them. Such shrunken interfaces are also called role interfaces. ISP is intended to keep a system decoupled and thus easier to refactor, change, and redeploy. ISP is one of the five SOLID principles of object-oriented design, similar to the High Cohesion Principle of GRASP.

#### Importance in object-oriented design

Within object-oriented design, interfaces provide layers of abstraction that simplify code and create a barrier preventing coupling to dependencies.

According to many software experts who have signed the Manifesto for Software Craftsmanship, writing well-crafted and self-explanatory software is almost as important as writing working software. Using interfaces to further describe the intent of the software is often a good idea.

A system may become so coupled at multiple levels that it is no longer possible to make a change in one place without necessitating many additional changes. Using an interface or an abstract class can prevent this side effect. 

#### Origin

The ISP was first used and formulated by Robert C. Martin while consulting for Xerox. Xerox had created a new printer system that could perform a variety of tasks such as stapling and faxing. The software for this system was created from the ground up. As the software grew, making modifications became more and more difficult so that even the smallest change would take a redeployment cycle of an hour, which made development nearly impossible.

The design problem was that a single Job class was used by almost all of the tasks. Whenever a print job or a stapling job needed to be performed, a call was made to the Job class. This resulted in a 'fat' class with multitudes of methods specific to a variety of different clients. Because of this design, a staple job would know about all the methods of the print job, even though there was no use for them.

The solution suggested by Martin utilized what is today called the Interface Segregation Principle. Applied to the Xerox software, an interface layer between the Job class and its clients was added using the Dependency Inversion Principle. Instead of having one large Job class, a Staple Job interface or a Print Job interface was created that would be used by the Staple or Print classes, respectively, calling methods of the Job class. Therefore, one interface was created for each job type, which was all implemented by the Job class. 

#### Typical violation

A typical violation of the Interface Segregation Principle is given in Agile Software Development: Principles, Patterns, and Practices in 'ATM Transaction example' and in an article also written by Robert C. Martin specifically about the ISP. This example discusses the User Interface for an ATM, which handles all requests such as a deposit request, or a withdrawal request, and how this interface needs to be segregated into individual and more specific interfaces. 

### Dependency inversion principle
> One should "depend upon abstractions, [not] concretions."

In object-oriented design, the dependency inversion principle is a specific form of decoupling software modules. When following this principle, the conventional dependency relationships established from high-level, policy-setting modules to low-level, dependency modules are reversed, thus rendering high-level modules independent of the low-level module implementation details. The principle states:

- High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).

- Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

By dictating that both high-level and low-level objects must depend on the same abstraction, this design principle inverts the way some people may think about object-oriented programming.

The idea behind points A and B of this principle is that when designing the interaction between a high-level module and a low-level one, the interaction should be thought of as an abstract interaction between them. This not only has implications on the design of the high-level module, but also on the low-level one: the low-level one should be designed with the interaction in mind and it may be necessary to change its usage interface.

In many cases, thinking about the interaction in itself as an abstract concept allows the coupling of the components to be reduced without introducing additional coding patterns, allowing only a lighter and less implementation dependent interaction schema.

When the discovered abstract interaction schema(s) between two modules is/are generic and generalization makes sense, this design principle also leads to the following dependency inversion coding pattern. 

#### Traditional layers pattern

In conventional application architecture, lower-level components (e.g. Utility Layer) are designed to be consumed by higher-level components (e.g. Policy Layer) which enable increasingly complex systems to be built. In this composition, higher-level components depend directly upon lower-level components to achieve some task. This dependency upon lower-level components limits the reuse opportunities of the higher-level components.

![](https://upload.wikimedia.org/wikipedia/commons/4/42/Traditional_Layers_Pattern.png)

The goal of the dependency inversion pattern is to avoid this highly coupled distribution with the mediation of an abstract layer, and to increase the re-usability of higher/policy layers.

#### Dependency inversion pattern

With the addition of an abstract layer, both high- and lower-level layers reduce the traditional dependencies from top to bottom. Nevertheless, the "inversion" concept does not mean that lower-level layers depend on higher-level layers. Both layers should depend on abstractions that draw the behavior needed by higher-level layers. 

![](https://upload.wikimedia.org/wikipedia/commons/8/8d/DIPLayersPattern.png)

In a direct application of dependency inversion, the abstracts are owned by the upper/policy layers. This architecture groups the higher/policy components and the abstractions that define lower services together in the same package. The lower-level layers are created by inheritance/implementation of these abstract classes or interfaces.

The inversion of the dependencies and ownership encourages the re-usability of the higher/policy layers. Upper layers could use other implementations of the lower services. When the lower-level layer components are closed or when the application requires the reuse of existing services, it is common that an Adapter mediates between the services and the abstractions. 

#### Dependency inversion pattern generalization

In many projects the dependency inversion principle and pattern are considered as a single concept that should be generalized, i.e., applied to all interfaces between software modules. There are at least two reasons for that:

- It is simpler to see a good thinking principle as a coding pattern. Once an abstract class or an interface has been coded, the programmer may say: "I have done the job of abstraction".
- Because many unit testing tools rely on inheritance to accomplish mocking, the usage of generic interfaces between classes (not only between modules when it makes sense to use generality) became the rule.

If the mocking tool used relies only on inheritance, it may become necessary to widely apply the dependency inversion pattern. This has major drawbacks:

- Merely implementing an interface over a class isn't sufficient to reduce coupling; only thinking about the potential abstraction of interactions can lead to a less coupled design.
- Implementing generic interfaces everywhere in a project makes it harder to understand and maintain. At each step the reader will ask themself what are the other implementations of this interface and the response is generally: only mocks.
- The interface generalization requires more plumbing code, in particular factories that generally rely on a dependency-injection framework.
- Interface generalization also restricts the usage of the programming language.

#### Generalization restrictions

The presence of interfaces to accomplish the Dependency Inversion Pattern (DIP) has other design implications in an object-oriented program:

- All member variables in a class must be interfaces or abstracts.
- All concrete class packages must connect only through interface or abstract class packages.
- No class should derive from a concrete class.
- No method should override an implemented method.
- All variable instantiation requires the implementation of a creational pattern such as the factory method or the factory pattern, or the use of a dependency-injection framework.

#### Interface mocking restrictions

Using inheritance-based mocking tools also introduces restrictions:

- Static externally visible members should systematically rely on dependency injection making them far harder to implement.
- All testable methods should become an interface implementation or an override of an abstract definition.

#### Future directions

Principles are ways of thinking. Patterns are common ways to solve problems. Coding patterns may be missing programming language features.

- Programming languages will continue to evolve to allow them to enforce stronger and more precise usage contracts in at least two directions: enforcing usage conditions (pre-, post- and invariant conditions) and state-based interfaces. This will probably encourage and potentially simplify a stronger application of the dependency inversion pattern in many situations.
- More and more mocking tools now use dependency-injection to solve the problem of replacing static and non virtual members. Programming languages will probably evolve to generate mocking-compatible bytecode. One direction will be to restrict the usage of non-virtual members. The other one will be to generate, at least in test situations, bytecode allowing non-inheritance based mocking.

#### Implementations
     
Two common implementations of DIP use similar logical architecture but with different implications.
     
A direct implementation packages the policy classes with service abstracts classes in one library. In this implementation high-level components and low-level components are distributed into separate packages/libraries, where the interfaces defining the behavior/services required by the high-level component are owned by, and exist within the high-level component's library. The implementation of the high-level component's interface by the low-level component requires that the low-level component package depend upon the high-level component for compilation, thus inverting the conventional dependency relationship. 

![](https://upload.wikimedia.org/wikipedia/commons/9/96/Dependency_inversion.png)

Figures 1 and 2 illustrate code with the same functionality, however in Figure 2, an interface has been used to invert the dependency. The direction of dependency can be chosen to maximize policy code reuse, and eliminate cyclic dependencies.

In this version of DIP, the lower layer component's dependency on the interfaces/abstracts in the higher-level layers makes re-utilization of the lower layer components difficult. This implementation instead ″inverts″ the traditional dependency from top-to-bottom to the opposite, from bottom-to-top.

A more flexible solution extracts the abstract components into an independent set of packages/libraries: 

![](https://upload.wikimedia.org/wikipedia/commons/a/ab/DIPLayersPattern_v2.png)

The separation of each layer into its own package encourages re-utilization of any layer, providing robustness and mobility.

#### Examples

##### Genealogical module

A genealogical system may represent relationships between people as a graph of direct relationships between them (father-son, father-daughter, mother-son, mother-daughter, husband-wife, wife-husband, etc.). This is very efficient and extensible, as it is easy to add an ex-husband or a legal guardian.

But some higher-level modules may require a simpler way to browse the system: any person may have children, parents, siblings (including half-brothers and -sisters or not), grandparents, cousins, and so on.

Depending on the usage of the genealogical module, presenting common relationships as distinct direct properties (hiding the graph) will make the coupling between a higher-level module and the genealogical module much lighter and allow one to change the internal representation of the direct relationships completely without any effect on the modules using them. It also permits embedding exact definitions of siblings or uncles in the genealogical module, thus enforcing the single responsibility principle.

Finally, if the first extensible generalized graph approach seems the most extensible, the usage of the genealogical module may show that a more specialized and simpler relationship implementation is sufficient for the application(s) and helps create a more efficient system.

In this example, abstracting the interaction between the modules leads to a simplified interface of the lower-level module and may lead to a simpler implementation of it. 

##### Remote file server client

Imagine you have to implement a client to a remote file server (FTP, cloud storage ...). You may think of it as a set of abstract interfaces:

- Connection/Disconnection (a connection persistence layer may be needed)
- Folder/tags creation/rename/delete/list interface
- File creation/replacement/rename/delete/read interface
- File searching
- Concurrent replacement or delete resolution
- File history management ...

If both local files and remote files offers the same abstract interfaces, any high-level module using local files and fully implementing the dependency inversion pattern will be able to access local and remote files indiscriminately.

Local disk will generally use folder, remote storage may use folder and/or tags. You have to decide how to unify them if possible.

On remote file we may have to use only create or replace: remote files update do not necessarily make sense because random update is too slow comparing local file random update and may be very complicated to implement). On remote file we may need partial read and write (at least inside the remote file module to allow download or upload to resume after a communication interruption), but random read isn't adapted (except if a local cache is used).

File searching may be pluggable : file searching can rely on the OS or in particular for tag or full text search, be implemented with distinct systems (OS embedded, or available separately).

Concurrent replacement or delete resolution detection may impact the other abstract interfaces.

When designing the remote file server client for each conceptual interface you have to ask yourself the level of service your high level modules require (not necessary all of them) and not only how to implement the remote file server functionalities but maybe how to make the file services in your application compatible between already implemented file services (local files, existing cloud clients) and your new remote file server client.

Once you have designed the abstract interfaces required, your remote file server client should implement these interfaces. And because you probably restricted some local functionalities existing on local file (for example file update), you may have to write adapters for local or other existing used remote file access modules each offering the same abstract interfaces. You also have to write your own file access enumerator allowing to retrieve all file compatible systems available and configured on your computer.

Once you do that, your application will be able to save its documents locally or remotely transparently. Or simpler, the high level module using the new file access interfaces can be used indistinctly in local or remote file access scenarios making it reusable.

Remark : many OSes have started to implement these kind of functionalities and your work may be limited to adapt your new client to this already abstracted models.

In this example, thinking of the module as a set of abstract interfaces, and adapting other modules to this set of interfaces, allows you to provide a common interface for many file storage systems. 

##### Model View Controller

![](https://upload.wikimedia.org/wikipedia/commons/2/2f/DIP_concrete_example.png)

UI and ApplicationLayer packages contains mainly concrete classes. Controllers contains abstracts/interface types. UI has an instance of ICustomerHandler. All packages are physically separated. In the ApplicationLayer there is a concrete implementation that Page class will use. Instances of this interface are created dynamically by a Factory (possibly in the same Controllers package). The concrete types, Page and CustomerHandler, don't depend on each other; both depend on ICustomerHandler.

The direct effect is that the UI doesn't need to reference the ApplicationLayer or any concrete package that implements the ICustomerHandler. The concrete class will be loaded using reflection. At any moment the concrete implementation could be replaced by another concrete implementation without changing the UI class. Another interesting possibility is that the Page class implements an interface IPageViewer that could be passed as an argument to ICustomerHandler methods. Then the concrete implementation could communicate with UI without a concrete dependency. Again, both are linked by interfaces. 

# Internationalization and localization

In computing, internationalization and localization (AmE) or internationalisation and localisation (BrE) are means of adapting computer software to different languages, regional peculiarities and technical requirements of a target locale. Internationalization is the process of designing a software application so that it can be adapted to various languages and regions without engineering changes. Localization is the process of adapting internationalized software for a specific region or language by translating text and adding locale-specific components. Localization (which is potentially performed multiple times, for different locales) uses the infrastructure or flexibility provided by internationalization (which is ideally performed only once before localization, or as an integral part of ongoing development).

## Naming

The terms are frequently abbreviated to the numeronyms i18n (where 18 stands for the number of letters between the first i and the last n in the word internationalization, a usage coined at Digital Equipment Corporation in the 1970s or 1980s) and L10n for localization, due to the length of the words. Some writers have the latter acronym capitalized to help distinguish the two.

Some companies, like IBM and Oracle, use the term globalization, g11n, for the combination of internationalization and localization. Also known as "glocalization" (a portmanteau of globalization and localization). 

