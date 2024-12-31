> **Software entities (classes, modules, functions) should be open for extension but closed for modification.**

**This means:**
`Open for Extension:` You should be able to add new functionality to an existing module or class without altering its existing code.

`Closed for Modification:` Once a class is written and tested, you should not need to modify it to add new functionality.

### Why is OCP Important?

- **Preserves Stability:** Existing code remains untouched, reducing the risk of introducing new bugs.

- **Enhances Flexibility:** Makes the system adaptable to new requirements without rewriting or breaking existing logic.

- **Supports Scalability:** New behaviors can be added with minimal effort.

- **Aligns with Agile Practices:** Easily adapts to changing requirements, a common scenario in software development.

---

### How Violates OCP?
OCP is violated when:
- Adding new behavior requires modifying existing classes or methods.
- This leads to tight coupling, making it harder to maintain or extend the system.

**Example of OCP Violation:**
Imagine a `PaymentProcessor `class that handles different payment methods:

```csharp
public class PaymentProcessor
{
    public void ProcessPayment(string paymentType, decimal amount)
    {
        if (paymentType == "CreditCard")
        {
            Console.WriteLine($"Processing credit card payment of {amount:C}.");
        }
        else if (paymentType == "Cash")
        {
            Console.WriteLine($"Processing cash payment of {amount:C}.");
        }
    }
}
```

**Problems:**
- Adding a new payment type requires modifying the `ProcessPayment` method.
- Every change increases the likelihood of bugs in existing functionality.
- This violates OCP because the class is not closed for modification.

---

### How to Implement Class-Level OCP
OCP can be implemented using techniques like polymorphism, composition, and strategy patterns. Below are steps for implementation using class-level OCP with execution flow.

**Step 1: Define an Abstraction**
Create an abstract class or interface to define common behavior.

```csharp
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}
```

**Step 2: Create Concrete Implementations**
Implement the interface for each payment type.

```csharp
public class CreditCardProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

public class CashPaymentProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount)
    {
        if (amount > 1000)
        {
            Console.WriteLine("Cash payment cannot exceed $1000.");
            return;
        }
        Console.WriteLine($"Processing cash payment of {amount:C}.");
    }
}
```

**Step 3: Use Abstraction in the Client**
The client depends on the abstraction (`IPaymentProcessor`) and can dynamically work with any implementation.

```csharp
public class PaymentHandler
{
    private readonly IPaymentProcessor _paymentProcessor;

    public PaymentHandler(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public void HandlePayment(decimal amount)
    {
        _paymentProcessor.ProcessPayment(amount);
    }
}
```

**Step 4: Call `PaymentHandler`**
Create and use the appropriate payment processor at runtime.

```csharp
var creditCardProcessor = new CreditCardProcessor();
var paymentHandler = new PaymentHandler(creditCardProcessor);
paymentHandler.HandlePayment(500);

var cashProcessor = new CashPaymentProcessor();
var cashPaymentHandler = new PaymentHandler(cashProcessor);
cashPaymentHandler.HandlePayment(1200);
```

#### Execution Flow
- The `PaymentHandler `relies on the `IPaymentProcessor `interface.
- Adding a new payment type (e.g., `DigitalWalletProcessor`) requires creating a new class that implements `IPaymentProcessor`.
- No changes are made to the existing `PaymentHandler `or other classes.

---

### How to Implement Method-Level OCP
For finer-grained control (specific method behavior), we can use strategies or composition to inject new logic into a method.

Example: Discount Calculation

```csharp
public interface IDiscountStrategy
{
    decimal CalculateDiscount(decimal price);
}

public class RegularDiscount : IDiscountStrategy
{
    public decimal CalculateDiscount(decimal price) => price * 0.1m; // 10% discount
}

public class VIPDiscount : IDiscountStrategy
{
    public decimal CalculateDiscount(decimal price) => price * 0.2m; // 20% discount
}

public class DiscountCalculator
{
    public decimal Calculate(decimal price, IDiscountStrategy discountStrategy)
    {
        return discountStrategy.CalculateDiscount(price);
    }
}

```

**Usage:**

```csharp
var calculator = new DiscountCalculator();
decimal discountedPrice = calculator.Calculate(1000, new VIPDiscount());
Console.WriteLine($"Discounted Price: {discountedPrice:C}");

```

**Flow:**
New discount types can be added without modifying the `DiscountCalculator`.

By following OCP, you build systems that are easier to extend and maintain while reducing risks of breaking existing functionality.