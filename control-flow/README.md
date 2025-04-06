# Control Flow

Apex provides several control flow statements that allow you to manage the execution of your code based on certain conditions. This includes conditional statements, loops, and matches. Below are some examples of how to use these control flow statements in Apex.

## Conditional Statements
Conditional statements allow you to execute different blocks of code based on certain conditions. The most common conditional statements in Apex are `if`, `else if`, `else`.

### Example: If-Else
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

### Example: If-ElseIf-Else
```apex
public class NumberRangeClassifier {
    public static String classify(Integer number) {
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
