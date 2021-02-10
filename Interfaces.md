# All about interfaces!

## What? <sub><sup>(📖⏳ < 1 minute)</sup></sub>
In java, an interface is used to make abstractions in your code. Generally speaking, abstraction is used when you want to group things together without having to deal with the details of those different _things_. An interface can achieve this by defining the commonalities between _things_, after which we can treat the _things_ with those commonalities as the same _thing_. In a later lecture you will learn about _abstract classes_, another form of abstraction. They will not be discussed here.

## Intuition / example use case <sub><sup>(📖⏳ 5 minutes)</sup></sub>

That was a very abstract (haha) description; let's think of an example.
\
We'll make a program for an animal shelter to keep track of their animals. The shelter houses dogs and cats, so to keep track of all the dogs and cats we'll create the classes `Dog` and `Cat`. 
```java
public class Cat {
    private String name;
    
    public Cat (String name){
        this.name = name;
    }
    
    public void makeNoise() {
        System.out.println("Meow!");
    }
}

public class Dog {
    private String name;

    public Dog (String name){
        this.name = name;
    }

    public void makeNoise() {
        System.out.println("Woof!");
    }
}
```

Now we could make an array for both dogs and cats in which all dogs and cats that are currently in the shelter should be represented.  
```java
Dog[] dogs = new Dog[25];
Cat[] cats = new Cat[25];
```

The shelter has room for 50 furry companions, so the above defined array sizes kind of make sense. But what if at some point there are 30 cats and 5 dogs? or 45 dogs and 1 cat? Trouble. Since the overflow of `Dog` instances don't fit in the `Cat[] cats`; only cats can be in that array. We also can't create a single class `Animal` to replace `Dog` and `Cat`, because dogs and cats make a different noise; by using a class `Animal` we need to make the concession of not knowing if it's a cat or a dog. This is a good use case for abstraction. We will abstract the types `Cat` and `Dog` into the more general type: `Animal` using an interface. In this interface we have to define the common methods between all the things that will _implement_ this interface.
#### The interface
```java
interface Animal {
    void makeNoise();
    String niceString();
}
```

The interface `Animal` tells us that every class that wants to implement this _must_ define the methods `public String niceString()` and `public void makeNoise()`. Knowing that _all_ classes that implement this interface are sure to have implemented those methods allows us to think of all animals in terms of those methods. Some code will clarify this:
First, let the classes `Dog` and `Cat` "implement" the `Animal` interface:
```java
public class Cat implements Animal{
    private String name;
    
    public Cat (String name){
        this.name = name;
    }
    
    public void makeNoise() {
        System.out.println("Meow!");
    }
    
    public String niceString(){
        return name + " the cat";
    }
}

public class Dog implements Animal{
    private String name;

    public Dog (String name){
        this.name = name;
    }

    public void makeNoise() {
        System.out.println("Woof!");
    }
    
    public String niceString(){
        return name + " the dog";
    }
}
```
Note that we now _have to_ implement the `niceString` method in `Cat` and `Dog`, because the interface says so. If we didn't implement it, the code wouldn't even compile.

It's important to understand that it's illegal to create an instance of an interface directly, like so:
```java
Animal muppet = new Animal(); // illegal!
```
An interface is not like a class in the sense that we can create instances of it (although it often looks like it). It will behalve like a class, and you can have variables of the type (`Animal muppet;` is perfectly fine), but you _cannot_ create an instance of an interface.

Now let's use the abstraction we just made!
```java
Animal[] animals = new Animal[2];
Animal myAnimal = new Dog("Kliko");
animals[0] = myAnimal;
animals[1] = new Cat("Lucifer");
```
As you see, the type of `animals` is an array of `Animal`s. This means that instances of any class implementing the `Animal` interface are allowed to be in that array. On line 2, an instance of `Dog` is created (`new Dog`), but we save it in a variable of type `Animal`. This is allowed, because a dog is an animal. We add a cat and a dog to the same array!

What we lose by doing this are the details of the specific classes. We know that the elements in the `Animal` array are animals, but which type of animal we don't know. This means we can only interact with these objects using the methods defined in the interface, since we know that all objects that are an `Animal` will for sure have implemented these methods. Image the class `Dog` has another method `public void fetch();`, what we _can't_ do is `animals[0].fetch()` because we don't know that that element is a dog (even though we explicitly added a dog to the array). It's an `Animal[]`, so all we know is that it's an animal. That means we _can_ do `animal[0].makeNoise();` because `void makeNoise()` is defined in the interface, therefore all animals are able to do it. 
```java
for(Animal a : animals){
    System.out.print(a.niceString());
    System.out.print(" says: ");
    a.makeNoise();
}
```
will print:
```
Kliko the dog says: Woof!
Lucifer the cat says: Meow!
```

#### Default methods
Interfaces can also contain default methods: methods that are implemented in the interface itself as a default for all implementing classes. The implementing classes don't _have_ to implement default methods from the interface, but they _can_. If they do, they override the default implementation. Note that default methods cannot make use of any attributes: interfaces can't define attributes themselves, and the interface doesn't know about any attributes its implementing classes may have defined. As example, let's create a default method `void excitedNoise()`, which simply lets the animal make a noise three times in a row:
```java
interface Animal {
    void makeNoise();
    String niceString();
    default void excitedNoise(){
        makeNoise(); 
        makeNoise(); 
        makeNoise();
    }
}
```
Simple as that!

## Interfaces as part of a hierarchy <sub><sup>(📖⏳ 2.5 minutes)</sup></sub>
But abstraction doesn't end there! What if we have the program we just made, introducing the interface `Animal`, as well as another interface `Plant`, which has a whole bunch of classes implementing _it_. Both animals and plants are organisms... see where this is going? Let's do it!
```java
public interface Organism {
    String sequenceGenome();
}
```

We can sequence the genome of any organism. Now imagine we are a _very_ versatile laboratory that is planning on sequencing the genome of Kliko the dog, as well as Frank the [fern](https://en.wikipedia.org/wiki/Fern). You can imagine the lab keeps track of something like this:
```java
Dog kliko = new Dog("Kliko");
Plant frank = new Fern("Frank");
Organism[] organisms = new Organism[] {kliko, frank};
for(Organism o : organisms){
    System.out.print(o.sequenceGenome());
}
```

For this to work, both `Plant`s as `Animal`s should be recognized as `Organism`s. We can do this by _extending_ the interface:
```java
interface Animal extends Organism{
    void makeNoise();
    String niceString();
}

interface Plant extends Organism { ... }
```
As explained before, classes that implement an interface must implement all the methods defined in that interface. This responsibility also holds for the methods defined in the interface which is extended. Concretely, this means we _must_ now implement the method `String sequenceGenome()` in the classes `Dog` and `Cat` (and `Fern`, etc.)

You might already (unknowingly) have used such a hierarchy before. The beloved `ArrayList<T>` class implements the interface `List<T>`, which extends the interface `Collection<T>`, which in turn extends `Iterable<T>`. You can have some fun browsing the documentation and checking out all the different classes and interfaces that implement `Collection<T>` for example. (Or maybe my idea of fun is different from yours).

## Extra: creating instances of an interface  <sub><sup>(📖⏳ 3 minutes)</sup></sub>
**Warning: if you're not yet comfortable with interfaces and/or get confused easily, don't read this.**

Huh...? I just told you you can't create an instance of an interface, right? Well, you can _kind of_ do this (or at least make it look like you're doing it). Here I show the three ways to create an instance of an interface type. I'll be using the `Organism` interface from my previous example.

#### Method 1
Using a class that implements the interface (this method you know)
```java
public class Baboon implements Organism {

    ...
    
    public String sequenceGenome(){
        return "GATCCGTATGCCATGTCGTACGCGTACTGACGTC"; // yes, every baboon has this exact genome
    }
}

Organism bjorn = new Baboon();
```

#### Method 2
Using an anonymous class. In this method you create an instance of a class that you define at the moment you make the instance. The class you define has no name and can't be used again to create another instance. In this case you must implement all methods of the interface in the body. This looks more like you create an instance of the interface:
```java
Organism jessie = new Organism() {
    public String sequenceGenome(){
        return "GTACGTACACTGCGTACCTGGA";
    }
}
```

#### Method 3
Using a lambda expression. This can only be done for _functional interfaces_. Functional interfaces are interfaces that contain only one method (like Organism). When creating an instance of a functional interface you can simply assign the lambda function that implements that one method to the variable. A lambda function is a short-hand notation of a function that is written as follows: `(paramters) -> {function body}`. For example `() -> { System.out.println("Hello"); }` is a lambda function that doesn't have any parameters and prints Hello. (If the function body is just one line you may omit the curly brackets `{}`). Here goes:

```java
Organism lemming = () -> "GACGATCGTCATCGATCTAGC";
```

## End
That's it! I explained what interfaces are, how they are used to create useful abstractions in your code, how interfaces can extend other interfaces, and some ways of creating instances of an interface. I hope this made interfaces more intuitive and I hope you appreciate how powerful interfaces can be.

Good luck with the assignments!
