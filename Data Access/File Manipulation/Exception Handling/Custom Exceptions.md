---
tags:
  - FileManipulation
  - JavaIO
  - CustomExceptions
  - ExceptionHandling
---

Java allows developers to create their own exception types to handle specific error conditions tailored to their application's needs.

### Defining Custom Exception Classes
Custom exceptions can enhance the readability and maintainability of your code by providing more meaningful error messages.
#### **Steps to Create a Custom Exception:**
1. **Extend an Existing Exception Class:** Typically, custom exceptions extend `Exception` (for checked exceptions) or `RuntimeException` (for unchecked exceptions).
2. **Provide Constructors:** At minimum, include constructors that mirror those in the superclass.
#### **Example: Creating a Custom Checked Exception**
```java
// InsufficientFundsException.java
public class InsufficientFundsException extends Exception {
    // Default constructor
    public InsufficientFundsException() {
        super("Insufficient funds for the transaction.");
    }

    // Constructor that accepts a custom message
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```
Example: Creating a Custom Unchecked Exception
```java
// InvalidUserInputException.java
public class InvalidUserInputException extends RuntimeException {
    // Default constructor
    public InvalidUserInputException() {
        super("Invalid input provided by the user.");
    }

    // Constructor that accepts a custom message
    public InvalidUserInputException(String message) {
        super(message);
    }
}
```
### Throwing Custom Exceptions
Use the `throw` keyword to explicitly throw your custom exceptions when a specific error condition occurs.
#### **Example: Throwing a Custom Checked Exception**
```java
public class BankAccount {
    private double balance;

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    // Method to withdraw money
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException("Attempted to withdraw " + amount + ", but only " + balance + " available.");
        }
        balance -= amount;
        System.out.println("Withdrawal successful. New balance: " + balance);
    }
}
```
### Catching Custom Exceptions
Handle custom exceptions in the same way as standard exceptions using `try-catch` blocks.
#### **Example: Catching a Custom Checked Exception**
```java
public class BankApp {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(100.0);

        try {
            account.withdraw(150.0);
        } catch (InsufficientFundsException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
**Example: Catching a Custom Unchecked Exception**
```java
public class UserInputHandler {
    public void processInput(String input) {
        if (input == null || input.isEmpty()) {
            throw new InvalidUserInputException("User input cannot be null or empty.");
        }
        // Process the input
        System.out.println("Processing input: " + input);
    }

    public static void main(String[] args) {
        UserInputHandler handler = new UserInputHandler();

        try {
            handler.processInput(""); // Empty input
        } catch (InvalidUserInputException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```