# Classes

Classes are the building blocks of Apex programming. They encapsulate data and behavior, allowing you to create reusable components. In this section, we will explore the structure of classes, their members, and how to use them effectively.

## Table of Contents
- [Overview](#overview)
- [Instances](#instances)
  - [Constructor](#constructor)
  - [Instance Variables](#instance-variables)
    - [Public Instance Variables](#public-instance-variables)
    - [Private Instance Variables](#private-instance-variables)
  - [Instance Methods](#instance-methods)
    - [Public Instance Methods](#public-instance-methods)
    - [Private Instance Methods](#private-instance-methods)
- [Access Modifiers](#access-modifiers)
- [Getters](#getters)
- [Setters](#setters)
- [This](#this)
- [Static](#static)
  - [Static Methods](#static-methods)
  - [Static Variables](#static-variables)
  - [Static Blocks](#static-blocks)

## Overview

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

## Instances
An instance of a class is an object created from that class. Each instance has its own set of properties and methods. You can create multiple instances of the same class, each with different values for their properties. Using the `Person` class defined above, you can create instances like this:

```apex
Person john = new Person('John Doe', 30);
Person jane = new Person('Jane Smith', 25);
```
In this example, `john` and `jane` are two instances of the `Person` class, each with their own `name` and `age` properties.

### Constructor

A constructor is a special method that is called whenever an instance of a class is created. It is used to initialize the instance variables of the class. The constructor has the same name as the class and does not have a return type.

In the `Person` class, the constructor was used to initialize the `name` and `age` instance variables using the parameters passed to it:

```apex
public Person (String name, Integer age) {
    this.name = name;
    this.age = age;
}
```

You can do a lot of things in a constructor, such as:
- ✅ Initialize properties.
- ✅ Perform validations.
- ✅ Set default values.
- ✅ Call other methods.

However, it is best to keep the constructor simple and focused on initializing the instance correctly. Here's a few things to avoid in a constructor:
- ❌ Avoid performing complex calculations or logic.
- ❌ Avoid doing I/O such as SOQLs, DMLs and Callouts.

### Instance Variables
Instance variables are variables that belong to an instance of a class. They are used to store the internal state of the instance. They are typically defined at the top of the class and can be either public or private.

#### Public Instance Variables

Public instance variables are instance variables that can be accessed from inside or outside the class. They are typically used to expose data that should be accessible to other classes. They are defined using the `public` access modifier. For example:

```apex
public class OrderItemDTO {
    /**
     * The product code of the order item.
     */
    public String productCode;

    /**
     * The quantity of the order item.
     */
    public Integer quantity;
}
```

In this example, `productCode` and `quantity` are public instance variables of the `OrderItemDTO` class. They can be accessed from outside the class like this:

```apex
OrderItemDTO orderItem = new OrderItemDTO();

orderItem.productCode = 'ABC123';
orderItem.quantity = 2;

System.debug(orderItem.productCode); // Output: ABC123
System.debug(orderItem.quantity); // Output: 2
```

They can calso also be accessed from within the class. For example, you can create a instance method to update the product code and quantity of the order item:

```apex
public void updateProductCode (String newProductCode) {
    productCode = newProductCode;
}
```

```apex
public void updateQuantity (Integer newQuantity) {
    quantity = newQuantity;
}
```

> [!TIP]
> Use public instance variables for simple data structures or Data Transfer Objects (DTOs) where you want to expose the data directly. For more complex classes, consider using private instance variables with indirect access through methods.

#### Private Instance Variables

Private instance variables are instance variables that can only be accessed from within the class. They are typically used to encapsulate data and prevent external access. They are defined using the `private` access modifier. For example:

```apex
public class Order {
    /**
     * The address of the order.
     */
    private Address address;

    /**
     * The discount applied to the order.
     */
    private Decimal discount;

    /**
     * The order items in the order.
     */
    private List<OrderItem> items;
}
```

In this example, `address`, `discount`, and `items` are private instance variables of the `Order` class. They can only be accessed from within the class. For example, you can create a instance method to add an `OrderItem` to the `Order`:

```apex
public class Order {
    /// Other code...

    /**
     * Add an order item to the order.
     */
    public void addItem (OrderItem item) {
        items.add(item);
    }
}
```

Since `items` is a private instance variable, the `Order` class can guarantee that it is only modified through its own methods. This helps maintain the integrity of the class's state. So, if you try to access `items` from outside the class, you will get an error:

```apex
System.debug(order.items); // Error: items cannot be accessed from outside the class
```

> [!TIP]
> Use private instance variables for encapsulating data and preventing external access. This helps maintain the integrity of the class's state and allows you to control how the data is accessed and modified.

> [!WARNING]
> Although you can use getters and setters to access private instance variables, it is not always recommended. If a variable is private, it is usually because you want to encapsulate its value and behavior. In that case, consider whether a getter or setter is necessary or if it would be better to provide a method that performs some action based on the variable value.

### Instance Methods

Similar to instance variables, instance methods are methods that belong to an instance of a class. They are used to define the behavior of the instance. Each instance method must have a return type, a name, and a body. Instance methods can be public or private.

#### Public Instance Methods
Public instance methods are instance methods that can be accessed from inside or outside the class. They are typically used to expose functionality that should be accessible to other classes. They are defined using the `public` access modifier. For example:

```apex
public void greet () {
    System.debug('Hello, my name is ' + name + ' and I am ' + age + ' years old.');
}
```
In this example, `greet()` is a public instance method of the `Person` class. It can be called from outside the class like this:

```apex
john.greet(); // Output: Hello, my name is John Doe and I am 30 years old.
```

#### Private Instance Methods

Private instance methods are instance methods that can only be accessed from within the class. They are typically used to provide encapsulated functionality that should not be exposed to other classes. They are defined using the `private` access modifier. For example, in `Order` class, you could have a private method that gives a 100% discount to the `Order`:

```apex
private void applyFullDiscount () {
    discount = 1.0;
}
```

Notably, you do not want to expose this method to other classes, because it could be used wrongly. So, you can keep it private. This way, you can ensure that the method is only called from within the class itself. If someone tries to call it from outside the class, they will get an error:

```apex
order.applyFullDiscount(); // Error: applyFullDiscount cannot be accessed from outside the class
```

It still can be called from other instance methods of the class, like this:

```apex
public void applyDiscount (Decimal discount) {
    if (customer.isVIP()) {
        applyFullDiscount();
    } else {
        this.discount = discount;
    }
}
```

### This Keyword

The `this` keyword is used to refer to the current instance of a class. It is often used to disambiguate between instance variables and method parameters or local variables that have the same name. For example:

```apex
public void updateName (String name) {
    this.name = name;
}
```

In this example, `this.name` refers to the instance variable `name`, while `name` refers to the parameter passed to the method.

## Access Modifiers

Access modifiers control the visibility of class members to other classes. The most common access modifiers in Apex are:

- `private`: The member is accessible only within the class it is defined in. This is the default access level if no modifier is specified.
- `protected`: The member is accessible within the class and its subclasses.
- `public`: The member is accessible from any class.
- `global`: The member is accessible from any class in the same namespace or from any class in a different namespace.

> [!TIP]
> We recommend using `private` access modifiers for class members unless you specifically need to expose them to other classes. This helps encapsulate the class's internal state and behavior, making it easier to maintain and understand.

> [!CAUTION]
> We do not recommend using `protected` access modifiers, even if you are extending a class. This is because it can lead to tight coupling between classes and make it harder to maintain the code. Instead, consider using other approaches to achieve the desired behavior.

## Getters

The getters section goes here...

## Setters

The setters sections goes here...

## Static

Static properties and methods are associated with the class itself, not with any instance. You can define static members using the `static` keyword and they are allowed only in outer classes.

Static members are static only inside of a single transaction. It means that if you have a static variable or a static block in a class, it will be reset when the transaction ends. For that reason, they are not suitable for storing data that needs to persist across transactions.

### Static Methods
Static methods are methods that belong to the class itself rather than to any instance of the class. They can be called without creating an instance of the class. For example:

```apex
public class Calculator {
    public static Integer add (Integer a, Integer b) {
        return a + b;
    }
}
```

You can call the static method like this:

```apex
Integer sum = Calculator.add(5, 10); // Output: 15
```

### Static Variables
Static variables are variables that belong to the class itself rather than to any instance of the class. They can be accessed without creating an instance of the class. For example:

```apex
public class Counter {
    public static Integer count = 0;
}
```

You can access the static variable like this:

```apex
Counter.count = 5; // Set the static variable

System.debug(Counter.count); // Output: 5
```

### Static Blocks

Static blocks are used to initialize static variables or perform any setup that needs to be done when the class is loaded. They are executed only once when the class is loaded. For example, you can use a static block to initialize a static variable, like this:

```apex
public class Product {
    private static Map<String, Decimal> discountsPerFamily;

    static {
        discountsPerFamily = new Map<String, Decimal>();

        discountsPerFamily.put('Electronics', 0.1);
        discountsPerFamily.put('Clothing', 0.2);
        discountsPerFamily.put('Books', 0.05);
    }
}
```

Once the `Product` class is loaded, the static block will be executed and the `discountsPerFamily` map will be initialized with the values.