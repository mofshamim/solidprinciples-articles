> **Each class should have only one responsibility or reason to change.**

This means each class should focus on a single functionality or responsibility and not combine unrelated tasks.

### Why is SRP Important?

- **Simplifies Maintenance :** Changes to a specific responsibility are isolated, reducing the risk of impacting unrelated code.

- **Improves Code Readability :** Classes are smaller and easier to understand.

- **Enhances Reusability :** Class can be reused in different contexts without carrying unnecessary responsibilities.

- **Eases Testing :** Testing a single responsibility is straightforward, as the class has fewer dependencies.

- **Reduces Coupling :** Avoids tightly coupling unrelated functionalities, making the codebase more flexible.

### How to Apply SRP
Here’s an example of implementing SRP for a ticket booking scenario. The solution demonstrates how to handle booking logic, database persistence, email notifications, and error logging using separate classes.

**Classes Overview**
`BookingService:` Core business logic for booking tickets.
`DatabaseService:` Handles database interactions.
`EmailService:` Sends email notifications.
`LoggerService:` Logs errors or other information.

#### Code Implementation
- `LoggerService` Handles logging operations.

```csharp
public class LoggerService
{
    public void LogError(string message)
    {
        // Log error to a file or monitoring system
        Console.WriteLine($"Error: {message}");
    }

    public void LogInfo(string message)
    {
        // Log general information
        Console.WriteLine($"Info: {message}");
    }
}
```

- `EmailService` Handles sending emails.

```csharp
public class EmailService
{
    public void SendEmail(string to, string subject, string body)
    {
        // Simulated email sending logic
        Console.WriteLine($"Email sent to {to} with subject: {subject}");
    }
}
```

- `DatabaseService` Handles database operations.

```csharp
public class DatabaseService
{
    public void SaveBooking(string bookingDetails)
    {
        // Simulated database save logic
        Console.WriteLine($"Booking saved: {bookingDetails}");
    }
}
```

- `BookingService` Core business logic for ticket booking. It orchestrates the other services.


```csharp
public class BookingService
{
    private readonly DatabaseService _databaseService;
    private readonly EmailService _emailService;
    private readonly LoggerService _loggerService;

    public BookingService(DatabaseService databaseService, EmailService emailService, LoggerService loggerService)
    {
        _databaseService = databaseService;
        _emailService = emailService;
        _loggerService = loggerService;
    }

    public void BookTicket(string userEmail, string bookingDetails)
    {
        try
        {
            // Step 1: Business logic for booking (e.g., validation, seat availability)
            _loggerService.LogInfo("Booking process started.");

            // Step 2: Save booking to the database
            _databaseService.SaveBooking(bookingDetails);

            // Step 3: Send confirmation email
            _emailService.SendEmail(userEmail, "Booking Confirmation", "Your booking is confirmed!");

            _loggerService.LogInfo("Booking process completed successfully.");
        }
        catch (Exception ex)
        {
            // Step 4: Log any errors
            _loggerService.LogError($"Error during booking: {ex.Message}");
        }
    }
}
```

- `Program Execution` Tie everything together and simulate the booking process.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        // Dependency setup
        var databaseService = new DatabaseService();
        var emailService = new EmailService();
        var loggerService = new LoggerService();

        var bookingService = new BookingService(databaseService, emailService, loggerService);

        // Simulated booking details
        string userEmail = "user@example.com";
        string bookingDetails = "Flight: XYZ123, Date: 2024-12-01, Seat: 12A";

        // Book the ticket
        bookingService.BookTicket(userEmail, bookingDetails);
    }
}
```

### Execution Flow
- **Business Logic Execution:** Booking process begins with validation and checks.
- **Database Save:** The booking details are stored using `DatabaseService`.
- **Email Notification:** A confirmation email is sent using - `EmailService`.
- **Error Handling:** Any exceptions during the process are logged by `LoggerService`.

This approach ensures a clean, modular design while adhering to SOLID principles.