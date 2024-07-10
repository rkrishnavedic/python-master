### Evolving Unstructured Code through SOLID Principles

#### Initial Unstructured Code

Without applying any principles, the code has multiple responsibilities, making it hard to maintain and extend:

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_details(self):
        return f"User: {self.name}, Email: {self.email}"

    def save(self):
        # Code to save user to a database
        print(f"User {self.name} saved to database")

    def send_email(self, message):
        # Code to send an email
        print(f"Sending email to {self.email} with message: {message}")

# Usage
user = User("Rahul Krishna", "rk@example.com")
print(user.get_details())
user.save()
user.send_email("Welcome!")
```

### Step 1: Single Responsibility Principle (SRP)

**Without SRP:**
- The `User` class handles user details, saving to a database, and sending emails, which mixes different responsibilities.

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_details(self):
        return f"User: {self.name}, Email: {self.email}"

    def save(self):
        # Code to save user to a database
        print(f"User {self.name} saved to database")

    def send_email(self, message):
        # Code to send an email
        print(f"Sending email to {self.email} with message: {message}")
```

**With SRP:**
- Separate responsibilities into different classes.

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_details(self):
        return f"User: {self.name}, Email: {self.email}"

class UserRepository:
    def save(self, user):
        # Code to save user to a database
        print(f"User {user.name} saved to database")

class EmailService:
    def send_email(self, user, message):
        # Code to send an email
        print(f"Sending email to {user.email} with message: {message}")
```

**Usage in our example:**

```python
user = User("Rahul Krishna", "rk@example.com")
print(user.get_details())

repo = UserRepository()
repo.save(user)

email_service = EmailService()
email_service.send_email(user, "Welcome!")
```

### Step 2: Open/Closed Principle (OCP)

**Without OCP:**
- Modifying the `EmailService` class to add new notification methods violates the open/closed principle.

```python
class EmailService:
    def send_notification(self, user, message):
        # Code to send an email
        print(f"Sending email to {user.email} with message: {message}")

class SMSService:
    def send_notification(self, user, message):
        # Code to send an SMS
        print(f"Sending SMS to {user.name} with message: {message}")
```

**With OCP:**
- Extend the functionality by creating new classes without modifying existing ones.

```python
from abc import ABC, abstractmethod

class NotificationService(ABC):
    @abstractmethod
    def send_notification(self, user, message):
        pass

class EmailService(NotificationService):
    def send_notification(self, user, message):
        # Code to send an email
        print(f"Sending email to {user.email} with message: {message}")

class SMSService(NotificationService):
    def send_notification(self, user, message):
        # Code to send an SMS
        print(f"Sending SMS to {user.name} with message: {message}")
```

**Usage in our example:**

```python
user = User("Rahul Krishna", "rk@example.com")

email_service = EmailService()
email_service.send_notification(user, "Welcome via Email!")

sms_service = SMSService()
sms_service.send_notification(user, "Welcome via SMS!")
```

### Step 3: Liskov Substitution Principle (LSP)

**Without LSP:**
- Subclasses might not be usable in place of their base classes without causing issues.

```python
class NotificationService:
    def send_notification(self, user, message):
        pass

class EmailService(NotificationService):
    def send_notification(self, user, message):
        print(f"Sending email to {user.email} with message: {message}")

class SMSService(NotificationService):
    def send_notification(self, user, message):
        print(f"Sending SMS to {user.name} with message: {message}")
```

**With LSP:**
- Ensure subclasses can replace base classes without altering the expected behavior.

```python
from abc import ABC, abstractmethod

class NotificationService(ABC):
    @abstractmethod
    def send_notification(self, user, message):
        pass

class EmailService(NotificationService):
    def send_notification(self, user, message):
        print(f"Sending email to {user.email} with message: {message}")

class SMSService(NotificationService):
    def send_notification(self, user, message):
        print(f"Sending SMS to {user.name} with message: {message}")
```

**Usage in our example:**

```python
def notify_user(notification_service, user, message):
    notification_service.send_notification(user, message)

user = User("Rahul Krishna", "rk@example.com")

email_service = EmailService()
sms_service = SMSService()

notify_user(email_service, user, "Welcome via Email!")  # No changes needed
notify_user(sms_service, user, "Welcome via SMS!")      # No changes needed
```

### Interface Segregation Principle (ISP)

The Interface Segregation Principle (ISP) states that no client should be forced to depend on methods it does not use. This principle aims to split large interfaces into smaller, more specific ones so that clients only need to know about the methods that are of interest to them.

In the context of our `User` example, let's assume we have multiple responsibilities like logging and reporting. According to ISP, we should create separate interfaces for logging and reporting rather than having a single, large interface.

### Detailed Explanation with Examples

#### Without ISP: A Large Interface

Imagine we have an interface that combines logging, reporting, and notification:

```python
from abc import ABC, abstractmethod

class UserService(ABC):
    @abstractmethod
    def log(self, message):
        pass

    @abstractmethod
    def generate_report(self, user):
        pass

    @abstractmethod
    def send_notification(self, user, message):
        pass
```

Any class that implements `UserService` would have to implement all these methods, even if it only needs to perform logging or reporting, which violates ISP.

#### With ISP: Segregated Interfaces

We can segregate the responsibilities into smaller, more focused interfaces:

```python
from abc import ABC, abstractmethod

class Logger(ABC):
    @abstractmethod
    def log(self, message):
        pass

class Reporter(ABC):
    @abstractmethod
    def generate_report(self, user):
        pass

class NotificationService(ABC):
    @abstractmethod
    def send_notification(self, user, message):
        pass
```

Each interface now has a single responsibility, adhering to ISP. Classes implementing these interfaces will only implement the methods relevant to their functionality.

### Applying ISP to Our Example

Let's enhance our `User` example with logging and reporting responsibilities:

#### Define Interfaces

```python
from abc import ABC, abstractmethod

class Logger(ABC):
    @abstractmethod
    def log(self, message):
        pass

class Reporter(ABC):
    @abstractmethod
    def generate_report(self, user):
        pass
```

#### Implement Interfaces

```python
class ConsoleLogger(Logger):
    def log(self, message):
        print(f"Log: {message}")

class HTMLReporter(Reporter):
    def generate_report(self, user):
        return f"<html><body>User Report: {user.name}</body></html>"
```

#### Use Interfaces in the User Class

```python
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_details(self):
        return f"User: {self.name}, Email: {self.email}"
```

#### Usage Example

```python
# Create user
user = User("Rahul Krishna", "rk@example.com")

# Create logger and reporter
logger = ConsoleLogger()
reporter = HTMLReporter()

# Use logger and reporter
logger.log(user.get_details())  # Log: User: Rahul Krishna, Email: rk@example.com
print(reporter.generate_report(user))  # <html><body>User Report: Rahul Krishna</body></html>
```

In this example, the `User` class remains simple, and the responsibilities of logging and reporting are handled by classes implementing the `Logger` and `Reporter` interfaces, respectively. This approach adheres to the Interface Segregation Principle by ensuring that each class only knows about and depends on the interfaces that are relevant to it.

### Step 5: Dependency Inversion Principle (DIP)

**Without DIP:**
- High-level modules depend on low-level modules, making the code less flexible and harder to maintain.

```python
class EmailService:
    def send_notification(self, user, message):
        # Code to send an email
        print(f"Sending email to {user.email} with message: {message}")

class SMSService:
    def send_notification(self, user, message):
        # Code to send an SMS
        print(f"Sending SMS to {user.name} with message: {message}")

class NotificationSender:
    def __init__(self):
        self.email_service = EmailService()
        self.sms_service = SMSService()
    
    def send(self, user, message):
        self.email_service.send_notification(user, message)
        self.sms_service.send_notification(user, message)
```

**With DIP:**
- Both high-level and low-level modules depend on abstractions, improving flexibility and maintainability.

```python
class NotificationSender:
    def __init__(self, service: NotificationService):
        self.service = service
    
    def send(self, user, message):
        self.service.send_notification(user, message)
```

**Usage in our example:**

```python
user = User("Rahul Krishna", "rk@example.com")

email_service = EmailService()
sms_service = SMSService()

notification_sender = NotificationSender(email_service)
notification_sender.send(user, "Welcome via Email!")  # Sending email to rk@example.com with message: Welcome via Email!

notification_sender.service = sms_service
notification_sender.send(user, "Welcome via SMS!")    # Sending SMS to Rahul Krishna with message: Welcome via SMS!
```
