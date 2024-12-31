> **Clients should not be forced to depend on interfaces or methods they do not use.**

This means, interfaces should be small and specific, containing only methods that are relevant to the client. This avoids the problem of having a single large interface with methods that may not be applicable to all its clients.

### Why Use the Interface Segregation Principle?

- **Avoids Code Bloat:** Large interfaces force clients to implement methods they don’t need, leading to unnecessary complexity and maintenance challenges.

- **Enhances Flexibility:** Smaller, focused interfaces are easier to use, modify, and extend without impacting unrelated parts of the code.

- **Improves Code Readability and Maintenance:** Segregated interfaces are easier to understand because they focus on specific behaviors.

- **Reduces Coupling:** Ensures that classes are dependent only on the functionality they need, making the system more modular and robust to change.

- **Prevents Unused Implementations:** Classes implementing a large interface with irrelevant methods often include placeholder or empty implementations, which can confuse developers and lead to potential bugs.

### How to Implement the Interface Segregation Principle?

- **Identify Responsibilities:** Break down large, multi-purpose interfaces into smaller, specific ones based on distinct behaviors.

- **Design Focused Interfaces:** Each interface should define methods related to a single responsibility or closely related functionality.

- **Use Composition Over Inheritance:** Compose classes using multiple smaller interfaces rather than inheriting from a large interface.

---
How to Implement ISP



**Without ISP (Violating the Principle):**
Here’s an example of a booking system where an INotification interface is used to handle different types of notifications.

```csharp
public interface INotification
{
    void SendEmail(string message);
    void SendSMS(string message);
    void SendPushNotification(string message);
}

public class EmailNotification : INotification
{
    public void SendEmail(string message)
    {
        Console.WriteLine("Email sent: " + message);
    }

    public void SendSMS(string message)
    {
        throw new NotImplementedException(); // Not needed
    }

    public void SendPushNotification(string message)
    {
        throw new NotImplementedException(); // Not needed
    }
}
```

**Problems:**
`EmailNotification` has to implement methods it doesn’t use (`SendSMS` and `SendPushNotification`).
Adding new notification types or methods will affect all classes implementing `INotification`, even if they don’t use them.


**With ISP (Following the Principle):**
Refactor the INotification interface into smaller, specific interfaces.






