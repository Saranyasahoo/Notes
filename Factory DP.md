# Factory Design Pattern
The Factory Design Pattern is a creational design pattern used to create objects without exposing the instantiation logic to the client. Instead, a factory class is used to create objects based on certain parameters or configuration.

In Spring Boot, the Factory Pattern can be integrated naturally with Spring’s Dependency Injection (DI) mechanism. Let’s walk through an example of how to implement the Factory Pattern in a Spring Boot application.

## 🔧 Use Case: Notification Service (Email, SMS, Push)
**Step 1: Define the Interface**
```java
public interface NotificationService {
    void sendNotification(String message);
}
```
**Step 2: Implement the Interface**
```java
@Service("EMAIL")
public class EmailNotificationService implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending EMAIL: " + message);
    }
}

@Service("SMS")
public class SmsNotificationService implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending SMS: " + message);
    }
}

@Service("PUSH")
public class PushNotificationService implements NotificationService {
    @Override
    public void sendNotification(String message) {
        System.out.println("Sending PUSH notification: " + message);
    }
}
```
**Step 3: Create the Factory Class**
```java
@Component
public class NotificationFactory {

    private final ApplicationContext context;

    @Autowired
    public NotificationFactory(ApplicationContext context) {
        this.context = context;
    }

    public NotificationService getNotificationService(String type) {
        return (NotificationService) context.getBean(type.toUpperCase());
    }
}

```
**Step 4: Use the Factory in a Controller or Service**
```java
@RestController
@RequestMapping("/notify")
public class NotificationController {

    private final NotificationFactory notificationFactory;

    @Autowired
    public NotificationController(NotificationFactory notificationFactory) {
        this.notificationFactory = notificationFactory;
    }

    @PostMapping("/{type}")
    public ResponseEntity<String> send(@PathVariable String type, @RequestBody String message) {
        NotificationService service = notificationFactory.getNotificationService(type);
        service.sendNotification(message);
        return ResponseEntity.ok("Notification sent using: " + type);
    }
}

```
**✅ Advantages of This Approach**
- Easy to add new notification types without changing existing logic.

- Spring's DI handles bean instantiation and management.

- Promotes Open/Closed Principle (open for extension, closed for modification).
