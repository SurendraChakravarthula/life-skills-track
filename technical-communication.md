# SOLID

SOLID is an acronym for the five object-oriented design(OOD) principles that are intended to make software designs more understandable, flexible and maintainable.

SOLID stands for:

* S - Single Responsibility Principle
* O - Open-Closed Principle
* L - Liskov Substitution Principle
* I - Interface Segregation Principle
* D - Dependency Inversion Principle


### Single Responsibility Principle(SRP)
SRP states that "A class should have only one reason to change". It means every class should perform only one functionality.

~~~
 public class Student {
        public void printDetails(){
           //functionality of the method
        }
        pubic void calculatePercentage(){
           //functionality of the method
        }
        public void addStudent() {
          //functionality of the method
        }
    } 
~~~


The above snippet contains three methods of different responsibility which violate the single responsibility principle. Let's refactor the above code.

~~~
//Student class with one responsibility
public class Student {
    public void addStudent(){
      //functionality of the method  
    }
}

//PrintStudentDetails with one responsibility
public class PrintStudentDetails {
    public void printDetails() {
      //functionality of the method  
    }
} 

//Percentage with one responsibility 
public class Percentage {
    public void calculatePercentage(){
      //functionality of the method  
    }
} 
~~~
Achieved SRP by Separating a single class into three classes with each functionality. 


### Open-Closed Principle
The open-Closed principle states that "Software entities should be open for extension, but closed for modification". It means every class should be open for extension but closed for modification. In doing so, we stop ourselves from modifying existing code and causing potential new bugs.

~~~
public class Guitar {
    private String make;
    private String model;
    private int volume;

    public Guitar(String make, String model, int volume) {
        this.make = make;
        this.model = model;
        this.volume = volume;
    }
}
~~~

Suppose we want to add a cool flame pattern to make it look more rock and roll.At this point, it might be tempting to just open up the Guitar class and add a flame pattern — but who knows what errors that might throw up in our application.

Instead, let's stick to the open-closed principle and simply extend our Guitar class

~~~
public class Guitar {

    private String make;
    private String model;
    private int volume;

    public Guitar(String make, String model, int volume) {
        this.make = make;
        this.model = model;
        this.volume = volume;
    }
}

class SuperCoolGuitarWithFlames extends Guitar {

    private String flameColor;

    public SuperCoolGuitarWithFlames(String make, String model, int volume, String flameColor) {
        super(make, model, volume);
        this.flameColor = flameColor;
    }
}
~~~

By extending the Guitar class, we can be sure that our existing class won't be affected.


### Liskov Substitution Principle
Liskov substitution principle states that "Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program". It means derived classes must be completely substitutable for their base classes.

if class A is a subtype of class B, then instances of class B should be substitutable with instances of class A without disrupting the behavior of our program. It extends the open-close principle and also focuses on the behavior of a superclass and its subtypes. 

~~~
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int calculateArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}
~~~

 The Square class violates the LSP because it changes the behavior of the superclass. If we create a Square object and treat it as a Rectangle, unexpected behavior can occur.

 To adhere to the LSP, it would be better to have a separate Square class that is not a subclass of Rectangles or to modify the design to handle the distinction between rectangles and squares appropriately, such as introducing an abstract shape class or interface.

 ### Interface Segregation Principle
The interface segregation principle states that "Many client-specific interfaces are better than one general-purpose interface". It means that larger interfaces should be split into smaller ones. By doing so, we can ensure that implementing classes only needs to be concerned about the methods that are of interest to them.


~~~
public interface BearKeeper {
    void washTheBear();
    void feedTheBear();
    void petTheBear();
}

~~~
Suppose we want to implement only one method of the above interface then the class should be abstract or it should implement other methods too.

~~~
public interface BearCleaner {
    void washTheBear();
}

public interface BearFeeder {
    void feedTheBear();
}

public interface BearPetter {
    void petTheBear();
}
~~~

Now, we can implement only the methods that matter to us.

~~~
public class CrazyPerson implements BearPetter {

    public void petTheBear() {
        //Good luck with that!
    }
}
~~~

### Dependency Inversion Principle
The dependency inversion principle states that One should "depend upon abstractions,not concretions". It means abstractions should not depend on details whereas the details should depend on abstractions. 


~~~
public class Windows98Machine {};
~~~

Let's add one of each to our constructor so that every Windows98Computer we instantiate comes prepacked with a Monitor and a standard keyboard

~~~
public class Windows98Machine {

    private final StandardKeyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine() {
        monitor = new Monitor();
        keyboard = new StandardKeyboard();
    }
}
~~~

By declaring the StandardKeyboard and Monitor with the new keyword, we've tightly coupled these three classes together.

Let's decouple our machine from the StandardKeyboard by adding a more general Keyboard interface and using this in our class

~~~
public interface Keyboard {};

public class Windows98Machine{

    private final Keyboard keyboard;
    private final Monitor monitor;

    public Windows98Machine(Keyboard keyboard, Monitor monitor) {
        this.keyboard = keyboard;
        this.monitor = monitor;
    }
}
~~~

we're using the dependency injection pattern to facilitate adding the Keyboard dependency into the Windows98Machine class.

Let's also modify our StandardKeyboard class to implement the Keyboard interface so that it's suitable for injecting into the Windows98Machine class:

~~~
public class StandardKeyboard implements Keyboard {};
~~~

Now our classes are decoupled and communicate through the Keyboard abstraction. If we want, we can easily switch out the type of keyboard in our machine with a different implementation of the interface.

By applying these principles, the code will be much more clear, testable, and expendable.

## References

* [JavaTpoint](https://www.javatpoint.com/solid-principles-java)
* [Baeldung](https://www.baeldung.com/solid-principles)
* [DigitalOcean](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
* [Solid Design Principles by 
kudvenkat](https://www.youtube.com/playlist?list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme)