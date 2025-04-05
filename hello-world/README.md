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