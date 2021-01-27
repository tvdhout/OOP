# Tutorial 1 java concepts

[Link to tutorial lecture on youtube](https://www.youtube.com/watch?v=aZFG6jy6OsE&list=PLYAJmxblYnQOzL98qTMwYgqlQhYXSim_l&index=1)

[Link to tutorial slides on Brightspace](https://brightspace.ru.nl/d2l/le/content/163566/viewContent/1098070/View)

_____

## Assignment test classes
**First finish the assignment before working on these tests! They are not mandatory, but give you an idea whether your program works in the way we expect it to.**

The assignments contain Test classes to test your solution. In the `Tester` class (not the `Test`) class, you can link the methods described there to your implementation of those methods. These methods are called by the `Test` class when ran. You will learn about tests in java in a later lecture.

_____

## What is a class?
A Class can be seen as a blueprint that gives instructions on how a _thing_ should behave. This _thing_ can be anything, but let's say it's a car.
Creating cars in java means to create _instances_ of the class Car. The class Car includes information about how a car should behave, how to create a car, and the attributes of a car.
Let's implement the blueprint for cars in java:

```java
class Car {
    private int serialNr;
 
    public Car(int serialNr) {
        this.serialNr = serialNr;
    }
 
    public int getSerialNr() {
        return serialNr;
    }
 
    public void setSerialNr(int serialNr) {
        this.serialNr = serialNr;
    }
}
```

This class is of course very minimal, but it tells us quite a lot:
* Every car that we create has a serial number, which is an integer. 
* This `serialNr` is a _private_ attribute, meaning we can only access it directly from the code that is written inside of this class.
* We have defined a _constructor_ for this class, which tells us how to create instances of this class and what it needs to do so. The constructor is the method with the same name as the class (`public Car(int serialNr){...}`), and it takes in arguments which are necessary to create a car. In this case, its serial number.
* Because the serial number is private and cannot be accessed from code outside this class, we define _getter_ and _setter_ methods to be able to interact with this attribute.

Creating an instance of this class is done as follows:

```java
Car myCar = new Car(95);
```

`new Car(95)` creates a new car object by calling the constructor method. The constructor method requires us to give it an integer - the serial number of the car we are creating. `myCar` is an _instance_ of Car: an object of type "Car". 

_____

## Private attributes

We can interact with this object using the attributes and methods of the class, so let's see how to get the serialNr of a car.

```java
System.out.println(myCar.getSerialNr());
//Output: 95
```
We have to interact with the private attribute `serialNr` via the getter and setter methods. If we would try to access the attribute directly using `myCar.serialNr` java would complain to us because the attribute is private, so what we are trying to do is illegal. In object oriented programming, attributes are (almost) always private, to make sure only the class itself may change its attributes. This way, we can limit how and when an attribute can be used and changed, right there where it is defined.

To change the serial number of a `Car`, we need to use the _setter_ method. `myCar.serialNr = 10;` is illegal, because the attribute is private (if it was public, it would be allowed). Instead, we use the setter method:

```java
myCar.setSerialNr(10);
```

This is useful because now we can define exactly how the user may change the serial number of a car. Take this alternative setter method:

```java
public void setSerialNr(int serialNr){
    if(serialNr >= 0){
        this.serialNr = serialNr;
    }
}
```

This allows the serialNr to change ONLY if the new value is positive. With this new setter method, there is no way that our car will ever have a negative serialNr, and this logic is encapsulated all in the Car class, right where it belongs.

_____

## `this` and `static` keywords
In java, the `this` keyword is used to refer to the instance of that class you are currently manipulating. When we call `myCar.setSerialNr(10);` the `this` in the method `setSerialNr` refers to `myCar`. So we set the attribute serialNr of myCar to the value we pass it. This keyword is optional if there is no confusion about the fact you are referring to the attribute. In this case, there _is_ confusion, because the parameter name is the same as the attribute name, so we have to be specific.

When a resource (attribute / method) is tagged with the `static` keyword, it means that that resource is independent of any instance of the class it belongs to. Another way to think of it is that it's shared between all instances of a class. In contrast to the `serialNr` each instance of a car has, we could define a static attribute which is the same in _all_ instances of car. It is rarely used in object oriented programming, because most of what we do has to do with instances of classes. An example of a static method is the main method which is used as entrypoint for your code. It is defined as static becuase we are not making instances of the class Main and calling the main method on one of those instances. Another use for static methods is a library class like `Math`. The Math library contains many useful functions, like `pow(double a, double b)` which computes a^b. This function has no business with any class attributes, we just give it two numbers and want to do something with those two numbers. In that case, the method can be made static. Static methods are called on the Class name, instead of an instance of that class:
```java
Math.pow(2, 5);
```
and not:
```java
Math myMath = new Math();
myMath.pow(2, 5);
```
If you find yourself using the static keyword during the first few assignments, something is likely wrong.

____

## Method vs Function
The difference between a method and a function is usually defined by the `static` keyword. It is called a method when it belongs to a class and performs operations on that class (like myCar.setSerialnr) and it is called a function if it independent of the class and just encapsulates some functionality in itself (like Math.pow).
____

## Naming conventions
In java, there exist naming conventions for how you should name your resources. Be sure to follow these guidelines.

* Class names are named in `CamelCase`: with first capital letter and every following word starting with a capital letter).
* Method and functions are named in `camelCase`: starting with a lowercase letter, and a capital letter for each following word
* Variable names (and class attributes) are named in `camelCase`: starting with a lowercase letter, and a capital letter for each following word
* Constants are `TRUMP_CASE`: all caps with underscores between words

____

## Scanner and System.in

Scanner is a built-in java class that can read from an input stream. It is a class with class methods, meaning we have to create an instance of that class to use it. The constructor expects an input from which to read; this can be many different things, including the console where the user can input text.
```java
Scanner myScanner = new Scanner(System.in);
```
`System.in` is the input stream from the console, much like `System.out` (used in System.out.println()) is the _output_ stream to the console.

With the scanner object `myScanner` we can start reading user input from the console. The scanner class has a couple methods that will be useful in this: `Scanner::next`, `Scanner::nextInt`, and `Scanner::nextLine`. By the way, the notation to reference a method is `ClassName::methodName`, of course, to call it you use a `.` instead of `::`. 

* `String x = myScanner.next();` takes the next piece of input from the console up to the first whitespace it sees (space, tab, etc.) and stores it in variable x.
* `String x = myScanner.nextLine();` takes the entire line of the user input and stops _after_ the next newline (<enter>).
* `int x = myScanner.nextInt();` much like `Scanner::next` but only accepts integers. If the next token is not an integer it will throw an excpetion (error).

After you are done with the scanner you can close it with `myScanner.close()`. Note that if you close a scanner that is using the `System.in` stream, you close that stream too! So only close it if your program no longer needs any user input.

____

## `==` vs. `.equals()`

Java objects (instances of classes) are stored in memory and have a memory address (aka pointer / reference). The `==` equality operator, when used on objects (like myCar, myScanner), will compare the reference to that object, and _not_ its contents! == should only be used to compare _primitive types_ such as `int`, `double`, `boolean`, etc. NOT any types (classes) you create yourself. For this, the `.equals()` method exists. By default, the .equals will still compare the references of the objects, but you can change this method to compare whatever you like. Let's say two `Car`s are equal when their serial numbers are equal. We can define the .equals() method in Car to reflect that:

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Car car = (Car) o;
    return serialNr == car.serialNr;
}
```
Let's dissect that method:

* The `@Override` annotation above the method indicates that we are overriding an already existant implementation of that method (the default one, which resides in the Object class). 
* We want to be able to compare the car to any object, so the parameter for this method is an `Object`. Note that every class you will make is always also an instance of the `Object` class, more on this when you learn about inheritance. 
* We first check if the reference of the car (`this`) is the same as the object we are comparing it to. If so, that's easy! It's definitely equal. 
* If not, we'll check if the object we are comparing it to is actually an instance of the `Car` class, if not, it can't be equal to this care, so we return false. 
* Finally, if it _is_ an instance of Car, we "cast" this foreign object to a Car (basically saying I want to see this object as a Car now). We then compare the serialNr of this car (serialNr) with the serialNr of the other car we are comparing it to (car.serialNr). If they are equal, return true!

____

## Exception handling

If anything goed wrong in your code, it will "throw" an "exception". What that means it the piece of code that fails will stop your program and give an error message. Excpetions can be _caught_, to ensure your program can continue after an excpetion is throws. For now, this is not needed. You can assume all user input is exectly how it is supposed to be (e.g. that the user will actually give an integer response when asked for a stundent number). 
