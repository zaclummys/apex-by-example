# Classes

Classes are the building blocks of Apex programming. They encapsulate data and behavior, allowing you to create reusable components. In this section, we will explore the structure of classes, their members, and how to use them effectively.

## Table of Contents

## Structure

A class in Apex is defined using the `class` keyword. The class must have a name and can contain a constructor, properties and methods. For example, a simple class to represent a `Person` would look like this:

```apex
public class Person {
    /**
     * The name of the person.
     */
    public String name;

    /**
     * The age of the person.
     */
    public Integer age;

    
    /**
     * Constructor

     * @param name The name of the person.
     * @param age The age of the person.
     */
    public Person (String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    /**
     * Greets the person by printing their name and age.
     */
    public void greet () {
        System.debug('Hello, my name is ' + name + ' and I am ' + age + ' years old.');
    }
}
```

You might notice that the class, properties, and methods are all declared with the `public` access modifier. This means they can be accessed from outside the class. You can also use other access modifiers like `private`, `protected` or `global` to control visibility. See the [Access Modifiers](#access-modifiers) section for more details.

## Constructor

## Instances

To create an instance of a class, you use the `new` keyword followed by the class name and parentheses. This invokes the constructor of the class automatically. For example, to create an instance of the `Person` class, you would do the following:

```apex
Person john = new Person('John Doe', 30);
```

Now, you can call its public methods:

```apex
john.greet(); // Output: Hello, my name is John Doe and I am 30 years old.
```

Aditionally, you can access its public properties:

```apex
System.debug(john.name); // Output: John Doe
System.debug(john.age); // Output: 30
```

## Access Modifiers

Access modifiers control the visibility of class members to other classes. The most common access modifiers in Apex are:

- `private`: The member is accessible only within the class it is defined in. This is the default access level if no modifier is specified.
- `protected`: The member is accessible within the class and its subclasses.
- `public`: The member is accessible from any class.
- `global`: The member is accessible from any class in the same namespace or from any class in a different namespace.

> [!TIP]
> We recommend using `private` access modifiers for class members unless you specifically need to expose them to other classes. This helps encapsulate the class's internal state and behavior, making it easier to maintain and understand.

## Getters

Getters are methods that allow you to retrieve the value of a private property. They are typically named `getPropertyName()`. For example, if you have a private property `age`, you can create a getter method like this:

```apex
private Integer age;

public Integer getAge () {
    return age;
}
```

You can then call this method to get the value of `age`:

```apex
Integer personAge = john.getAge(); // Output: 30
```

> [!WARNING]
> Getters should not have any side effects. They should only return the value of the property without modifying any state or performing any actions.

> [!NOTE]
> Altough you can create getters for every property, it is not always recommended. If a property is private, it is usually because you want to encapsulate its value and behavior. In that case, consider whether a getter is necessary or if it would be better to provide a method that performs some action based on the property value.

> [!TIP]
> Sometimes it makes sense to make the property public and not create a getter method. For example, if `age` is public, you can simply do `john.age` to get the value.

## Setters
Setters are methods that allow you to set the value of a private property. They are typically named `setPropertyName(value)`. For example, if you have a private property `name`, you can create a setter method like this:

```apex
private String name;

public void setName (String name) {
    this.name = name;
}
```

## This
## Static Variables
## Static Methods

## Further reading
