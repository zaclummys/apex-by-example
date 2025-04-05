# Hello World

This is a simple example of Apex code that prints "Hello, World!" to the Debug Log.

```Apex
public class HelloWorld {
    public static void execute () {
        System.debug('Hello, World!');
    }
}
```

# Usage
You can execute this code in the Salesforce Developer Console or any Apex execution context. To run the code, simply call the `execute` method of the `HelloWorld` class:

```Apex
HelloWorld.execute();
```

# Further Reading
- [Apex Developer Guide | Salesforce Developers](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_dev_guide.htm)
- [System Class | Apex Reference Guide | Salesforce Developers](https://developer.salesforce.com/docs/atlas.en-us.apexref.meta/apexref/apex_methods_system_system.htm)