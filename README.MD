# 😃 Android Interview Question
With LUCK for all of us




1. **What is Context?**

Putting it simply:
As the name suggests, it's the context of the current state of the application/object. It lets newly created objects understand what has been going on. Typically you call it to get information regarding another part of your program (activity and package/application).
You can get the context by invoking `getApplicationContext()`, `getContext()`, `getBaseContext(),` or `this` (when in a class that extends from `Context` , such as the Application, Activity, Service, and IntentService classes).

Typical uses of context:

- **Creating new objects**: Creating new views, adapters, listeners:
     TextView tv = new TextView(getContext());
     ListAdapter adapter = new SimpleCursorAdapter(getApplicationContext(), ...);
- **Accessing standard common resources**: Services like LAYOUT_INFLATER_SERVICE, SharedPreferences:
     context.getSystemService(LAYOUT_INFLATER_SERVICE)
     getApplicationContext().getSharedPreferences(*name*, *mode*);
- **Accessing components implicitly**: Regarding content providers, broadcasts, intent
     getApplicationContext().getContentResolver().query(uri, ...);


----------

2. **What is Singleton? What are the advantages and disadvantages?**

Singleton classes are a design pattern used to restrict the instantiation of a class to a single instance. In other words, **a singleton class is a class that can only be instantiated once**, and any subsequent attempts to instantiate the class will return the same instance that was created previously.
The need for singleton classes arises when it is necessary to maintain a single instance of a class for the entire lifetime of an application. For example, we might use a singleton class to represent a database connection, a configuration manager, or a logging service. By restricting the number of instances of these classes to one, we can ensure that the application's state remains consistent and that there are no conflicts between multiple instances of the same class.


    class Singleton private constructor() {
        companion object {
    
            @Volatile
            private var instance: Singleton? = null
    
            fun getInstance() =
                instance ?: synchronized(this) {
                    instance ?: Singleton().also { instance = it }
                }
        }
    
        fun doSomething() = "Doing something"
    }
----------
3. **What is dependency injection?**

Classes often require references to other classes. For example, a `Car` class might need a reference to an `Engine` class. These required classes are called *dependencies*, and in this example the `Car` class is dependent on having an instance of the `Engine` class to run.
There are **three** ways for a class to get an object it needs:

1. The class constructs the dependency it needs. In the example above, `Car` would create and initialize its instance of `Engine`.
2. Grab it from somewhere else. Some Android APIs, such as `Context` getters and `getSystemService()`, work this way.
3. Have it supplied as a parameter. The app can provide these dependencies when the class is constructed or pass them into the functions that need each dependency. In the example above, the `Car` constructor would receive `Engine` as a parameter.
    class Car {
    
        private final Engine engine;
    
        public Car(Engine engine) {
            this.engine = engine;
        }
    
        public void start() {
            engine.start();
        }
    }
    
    
    class MyApp {
        public static void main(String[] args) {
            Engine engine = new Engine();
            Car car = new Car(engine);
            car.start();
        }
    }

 **The benefits of this DI-based approach are:**
 

- Reusability of `Car`. You can pass in different implementations of `Engine` to `Car`. For example, you might define a new subclass of `Engine` called `ElectricEngine` that you want `Car` to use. If you use DI, all you need to do is pass in an instance of the updated `ElectricEngine` subclass, and `Car` still works without any further changes.
- Easy testing of `Car`. You can pass in test doubles to test your different scenarios. For example, you might create a test double of `Engine` called `FakeEngine` and configure it for different tests.
----------
4. **When should Throwable be used instead of new Exception?**
In the Java documentation, the Throwable class is defined as “the super-class of all errors and exceptions in the Java language“.The Throwable class has two direct sub-classes – namely, the Error and Exception classes.

Error and its sub-classes are unchecked exceptions, while the sub-classes of Exception can be either checked or unchecked exceptions.
 Recoverable Situations
There are situations where recovery is generally possible and can be handled with either checked or unchecked sub-classes of the Exception class.

For example, a program might want to use a file that happens to not exist at the specified location, resulting in a checked FileNotFoundException being thrown.

Another example is the program attempting to access a system resource without having permission to do that, resulting in an unchecked AccessControlException being thrown.

As per the Java documentation, the Exception class “indicates conditions that a reasonable application might want to catch“.

 Irrecoverable Situations
There are cases where a program can get in a state where recovery is impossible in an event of a failure. Common examples of this are when a stack overflow occurs or the JVM runs out of memory.

In these situations, the JVM throws StackOverflowError and OutOfMemoryError, respectively. As suggested by their names, these are sub-classes of the Error class.

According to the Java documentation, the Error class “indicates serious problems that a reasonable application should not try to catch“.
