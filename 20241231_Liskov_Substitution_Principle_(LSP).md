> **Subtypes must be substitutable for their base types without altering the correctness of the program.**

This means, if you have a class B that extends a class A, you should be able to replace A with B without breaking the functionality of your application. Apply LSP in inheritance scenarios and when using polymorphism.

### Why is LSP Important?
- **Ensures Reliable Substitution:** Promotes a design where derived classes work seamlessly in place of their base class.

- **Improves Maintainability:** Ensures that new derived classes won’t break the existing code.

- **Enhances Reusability:** Enables the use of polymorphism effectively, as derived classes adhere to the behavior of their base class.

- **Prevents Violations of Expected Behavior:** Protects against surprising behaviors caused by derived classes altering functionality unexpectedly.

- **Extensibility:** Adding a new payment method (e.g., `DigitalWalletProcessor`) requires implementing the same base class, ensuring consistency.

---

### How to Implement SRP

**Scenario: Payment System**
You’re developing a system to process payments. Initially, there’s a base class `PaymentProcessor `and a derived class `CreditCardProcessor`. Later, a new requirement arises to handle cash payments.

**Without LSP (Violation):**

```csharp
public class PaymentProcessor
{
    public virtual void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing payment of {amount:C}.");
    }
}

public class CreditCardProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

public class CashPaymentProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        if (amount > 1000)
        {
            throw new InvalidOperationException("Cash payments cannot exceed $1000.");
        }
        Console.WriteLine($"Processing cash payment of {amount:C}.");
    }
}
```

**Here’s the issue:**

- The base class `PaymentProcessor `assumes any amount can be processed, but the `CashPaymentProcessor `adds a restriction (amount <= 1000), violating the principle.

- If you use the `CashPaymentProcessor `where a `PaymentProcessor `is expected, the client might encounter unexpected exceptions.


---

**With LSP (Solution):**
Refactor the code to separate concerns and ensure substitutability. Use interfaces or base classes that clearly define the responsibilities of each payment type.


```csharp
public abstract class PaymentProcessor
{
    public abstract void ProcessPayment(decimal amount);
}

public class CreditCardProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        Console.WriteLine($"Processing credit card payment of {amount:C}.");
    }
}

public class CashPaymentProcessor : PaymentProcessor
{
    public override void ProcessPayment(decimal amount)
    {
        if (amount > 1000)
        {
            Console.WriteLine("Payment failed: Cash payments cannot exceed $1000.");
            return;
        }
        Console.WriteLine($"Processing cash payment of {amount:C}.");
    }
}
```

**Here’s how it works with LSP:**

```csharp
public void ProcessTransaction(PaymentProcessor paymentProcessor, decimal amount)
{
    paymentProcessor.ProcessPayment(amount);
}
```

- You can pass either `CreditCardProcessor` or `CashPaymentProcessor `to `ProcessTransaction`, and the behavior will always align with the expectations of the base class.

- The `CashPaymentProcessor` does not throw unexpected exceptions; it gracefully handles cases exceeding its limit.
