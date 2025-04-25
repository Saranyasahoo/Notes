# To print a message in production using a Spring Boot application

## Step 1: Add Message in a Startup Component
Create a class annotated with @Component and use @EventListener(ApplicationReadyEvent.class) to print the message after the app has started.

```java
package com.yourcompany.yourapp;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class StartupMessagePrinter {

    private static final Logger logger = LoggerFactory.getLogger(StartupMessagePrinter.class);

    @EventListener(ApplicationReadyEvent.class)
    public void onApplicationReady() {
        logger.info("==================================================");
        logger.info("Out of respect for the lives lost in the recent tragedy in Pahalgam,");
        logger.info("we are pausing all activities for the next 24 hours and closing our offices.");
        logger.info("Some silences are sacred.");
        logger.info("");
        logger.info("Thank you for standing with us in this moment of reflection.");
        logger.info("==================================================");
    }
}

```
## Step 2: Logging Configuration (Optional)
Make sure your logging configuration in application.properties (or logback-spring.xml) is set to show INFO level logs.

```java
# application.properties
logging.level.root=INFO

```

# Note
```java
@EventListener(ApplicationReadyEvent.class)
```
This annotation allows you to run a method automatically after the Spring Boot application is fully started — which is the perfect place to log or print a respectful message like the one you shared.
