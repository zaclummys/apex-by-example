# Primitives

Apex supports a variety of primitive data types, which are the building blocks for more complex data structures. Understanding these types is crucial for effective programming in Apex.

These types are used to define variables, parameters, and return values in your Apex code. Below is a summary of the primitive types available in Apex, along with their characteristics and usage.

## Primitives Data Types

| Type      | Description                                                       |
|-----------|-------------------------------------------------------------------|
| Boolean   | `true` or `false`                                                 |
| Integer   | 32-bit whole number (no decimal)                                  |
| Long      | 64-bit whole number (no decimal)                                  |
| Double    | 64-bit number with decimal support                                |
| Decimal   | High-precision decimal number (great for currency/finance)        |
| String    | Sequence of characters (text)                                     |
| Date      | Indicates a particular day (e.g. 2025-04-05)                      |
| Time      | Indicates a particular time (e.g. 14:30:00)                       |
| Datetime  | Indicates a particular day and time (e.g. 2025-04-05T14:30:00Z)   |
| ID        | 18-character unique Salesforce record identifier                  |
| Blob      | Binary data, used for files, encryption, etc                      |

## Usage
Now let's look at some examples of how to use these primitive types in Apex.

### Boolean
```apex
Boolean isActive = true;

if (isActive) {
    System.debug('The record is active.');
} else {
    System.debug('The record is inactive.');
}
```

### Integer
```apex
Integer count = 10;

System.debug('Count: ' + count); // Output: Count: 10
```

### Long
```apex
Long largeNumber = 1234567890123456789L;

System.debug('Large Number: ' + largeNumber); // Output: Large Number: 1234567890123456789
```

### Double
```apex
Double pi = 3.14159

System.debug('Value of Pi: ' + pi); // Output: Value of Pi: 3.14159
```

### Decimal
```apex
Decimal price = 19.99;

System.debug('Price: ' + price); // Output: Price: 19.99
```

### String
```apex
String greeting = 'Hi, Mr. Smith';

System.debug(greeting); // Output: Hi, Mr. Smith
```

### Date
```apex
Date today = Date.today();

System.debug('Today\'s date: ' + today); // Output: Today's date: 2025-04-05
```

### Time
```apex
Time currentTime = Time.now();

System.debug('Current time: ' + currentTime); // Output: Current time: 14:30:00
```

### Datetime
```apex
Datetime now = Datetime.now();

System.debug('Current date and time: ' + now); // Output: Current date and time: 2025-04-05T14:30:00Z
```

### ID
```apex
ID recordId = '0012w00000Xyz123ABC';

System.debug('Record ID: ' + recordId); // Output: Record ID: 0012w00000Xyz123ABC
```

### Blob
```apex
// Example of a Base64 encoded string
String base64Data = 'JVBERi0xLjQKJeLjz9MKMyAwIG9iago8PC9UeXBlIC9QYWdl...';

// Convert Base64 string to Blob
Blob fileBlob = EncodingUtil.base64Decode(base64Data);

System.debug(fileBlob); // Output: Blob[12345]
```