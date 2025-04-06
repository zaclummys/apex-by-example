# Control Flow

Control flow statements are essential in programming as they allow you to control the execution of your code based on certain conditions. In Apex, you can use conditional statements, loops, and switch statements to manage the flow of your program.

## Table of Contents
- [Conditional Statements](#conditional-statements)
  - [If-Else](#if-else)
  - [If-ElseIf-Else](#if-elseif-else)
- [Loops](#loops)
    - [Traditional For Loop](#traditional-for-loop)
    - [For Each Loop](#for-each-loop)
    - [While Loop](#while-loop)
    - [Do-While Loop](#do-while-loop)
- [Switch Statements](#switch-statements)
    - [Switch With Literals](#switch-with-literals)
    - [Switch With Enums](#switch-with-enums)
    - [Switch With SObjects](#switch-with-sobjects)
- [Further reading](#further-reading)


## Conditional Statements
Conditional statements allow you to execute different blocks of code based on certain conditions. The most common conditional statements in Apex are `if`, `else if`, `else`.

### If-Else

If-Else statements are used to execute a block of code if a specified condition is true, and optionally execute another block of code if the condition is false.

```apex
public class NumberParityChecker {
    public static String check (Integer number) {
        if (number % 2 == 0) {
            return 'Even';
        } else {
            return 'Odd';
        }
    }
}
```

### If-ElseIf-Else
Similar to If-Else, the If-ElseIf-Else statement allows you to check multiple conditions in a single block of code. You can chain multiple `else if` statements to check for different conditions.

```apex
public class NumberRangeClassifier {
    public static String classify (Integer number) {
        if (number < 0) {
            return 'Negative';
        } else if (number >= 0 && number <= 10) {
            return 'Small';
        } else if (number > 10 && number <= 100) {
            return 'Medium';
        } else if (number > 100 && number <= 1000) {
            return 'Large';
        } else {
            return 'Very Large';
        }
    }
}
```

## Loops
Loops are used to execute a block of code multiple times. Apex supports several types of loops, including `for`, `for each`, `while`, and `do while`.

### Traditional For Loop

```apex
public class NumberPrinter {
    public static void printNumbers () {
        for (Integer number = 1; number <= 10; number++) {
            System.debug(number); // Print numbers from 1 to 10
        }
    }
}
```

### For Each Loop

The `for each` loop is used to iterate over collections like lists and sets. It simplifies the syntax for iterating through elements.

```apex
public class NamePrinter {
    public static void printNames () {
        List<String> names = new List<String> {
            'Alice',
            'Bob',
            'Charlie'
        };

        for (String name : names) {
            System.debug(name); // Print each name in the list
        }
    }
}
```

### While Loop

The `while` loop continues to execute a block of code as long as a specified condition is true.

```apex
public class Countdown {
    public static void countdownFromTen () {
        Integer number = 10;

        while (number > 0) {
            System.debug(number); // Print 10 to 1
            number--;
        }
    }
}
```

### Do-While Loop

The `do while` loop is similar to the `while` loop, but it guarantees that the block of code will execute at least once before checking the condition.

```apex
public class DoWhileExample {
    public static void runOnceAtLeast () {
        Integer number = 0;
        do {
            System.debug('Number is: ' + number); 
            number++;
        } while (number < 5);
    }
}
```

## Switch Statements
Switch statements allow you to execute different blocks of code based on the value of a variable. They are often more readable than multiple `if-else` statements when checking the same variable against different values.

### Switch With Literals
You can use switch statements with literals to handle different cases based on the value of a variable.
```apex
public class DayOfWeek {
    public static String getDayName (Integer day) {
        switch on day {
            when 1 {
                return 'Monday';
            }
            when 2 {
                return 'Tuesday';
            }
            when 3 {
                return 'Wednesday';
            }
            when 4 {
                return 'Thursday';
            }
            when 5 {
                return 'Friday';
            }
            when 6 {
                return 'Saturday';
            }
            when 7 {
                return 'Sunday';
            }
            when else {
                return null;
            }
        }
    }
}
```

### Switch With Enums
Using enums in switch statements can make your code cleaner and more maintainable. Enums are a special type of class that represents a fixed set of constants.

```apex
public enum Season { SPRING, SUMMER, FALL, WINTER }

public class SeasonDescriber {
    public static String describe (Season season) {
        switch on season {
            when SPRING {
                return 'Flowers are blooming.';
            }
            when SUMMER {
                return 'Time for the beach.';
            }
            when FALL {
                return 'Leaves are falling.';
            }
            when WINTER {
                return 'Snow is coming.';
            }
        }
    }
}
```

### Switch With SObjects

You can also use switch statements with SObjects to handle different types of records. This is particularly useful when you want to perform different actions based on the type of SObject.

```apex
public class AccountNameExtractor {
    public static String extractAccountName (SObject accountLike) { 
        switch on accountLike {
            when Account account {
                System.debug(account.Name);
            }
            when Contact contact {
                System.debug(contact.Account.Name);
            }
        }
    }
}
```

## Further reading
- [Control Flow Statements | Apex Developer Guide | Salesforce Developers](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_control_flow.htm)
- [Collections | Apex Developer Guide | Salesforce Developers](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_collections.htm)