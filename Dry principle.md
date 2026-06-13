# DRY Principle (Don't Repeat Yourself)

## What is DRY?

**DRY (Don't Repeat Yourself)** is one of the fundamental software design principles.

The idea is simple:

> Every piece of knowledge or logic should have a single, authoritative representation within a system.

Instead of writing the same code multiple times, we should create reusable methods, classes, or components.

### Why is DRY Important?

Without DRY:

* Code becomes harder to maintain.
* Bug fixes must be applied in multiple places.
* Development becomes slower.
* Code readability decreases.

With DRY:

* Less code duplication.
* Easier maintenance.
* Better readability.
* Improved scalability.

---

# Example Without DRY

Suppose we need to calculate the total price after adding GST.

```java
public class OrderService {

    public void processLaptopOrder() {
        double price = 50000;
        double finalPrice = price + (price * 0.18);

        System.out.println("Laptop Price: " + finalPrice);
    }

    public void processMobileOrder() {
        double price = 30000;
        double finalPrice = price + (price * 0.18);

        System.out.println("Mobile Price: " + finalPrice);
    }

    public void processTabletOrder() {
        double price = 20000;
        double finalPrice = price + (price * 0.18);

        System.out.println("Tablet Price: " + finalPrice);
    }
}
```

### Problem

The GST calculation logic is repeated multiple times:

```java
price + (price * 0.18)
```

If GST changes from 18% to 20%, we must update every occurrence.

---

# Applying DRY

Extract the common logic into a reusable method.

```java
public class PriceCalculator {

    public double calculateFinalPrice(double price) {
        return price + (price * 0.18);
    }
}
```

```java
public class OrderService {

    private PriceCalculator calculator = new PriceCalculator();

    public void processLaptopOrder() {
        System.out.println(
            "Laptop Price: " +
            calculator.calculateFinalPrice(50000)
        );
    }

    public void processMobileOrder() {
        System.out.println(
            "Mobile Price: " +
            calculator.calculateFinalPrice(30000)
        );
    }

    public void processTabletOrder() {
        System.out.println(
            "Tablet Price: " +
            calculator.calculateFinalPrice(20000)
        );
    }
}
```

### Benefits

* GST logic exists in one place.
* Easier maintenance.
* Reduced chances of bugs.
* Better code organization.

---

# Real-World Example in Spring Boot

### Bad Practice

```java
@RestController
public class UserController {

    @GetMapping("/users")
    public List<User> getUsers() {

        System.out.println("Request Started");

        List<User> users = userService.getUsers();

        System.out.println("Request Completed");

        return users;
    }
}
```

```java
@RestController
public class ProductController {

    @GetMapping("/products")
    public List<Product> getProducts() {

        System.out.println("Request Started");

        List<Product> products = productService.getProducts();

        System.out.println("Request Completed");

        return products;
    }
}
```

Logging code is duplicated.

---

### DRY Solution Using Spring Interceptor

```java
@Component
public class LoggingInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(
            HttpServletRequest request,
            HttpServletResponse response,
            Object handler) {

        System.out.println("Request Started");
        return true;
    }

    @Override
    public void afterCompletion(
            HttpServletRequest request,
            HttpServletResponse response,
            Object handler,
            Exception ex) {

        System.out.println("Request Completed");
    }
}
```

Now every controller automatically gets logging behavior without duplication.

---

# DRY vs Over-Engineering

DRY does not mean creating abstractions for everything.

Bad Example:

```java
public class StringUtils {
    public String getUserName(User user) {
        return user.getName();
    }
}
```

Creating a utility method for a single line used only once adds unnecessary complexity.

### Rule of Thumb

Follow DRY when:

* Logic is repeated multiple times.
* Future changes are likely.
* Reusability improves maintainability.

Avoid DRY when:

* The abstraction makes code harder to understand.
* The logic is used only once.
* You are guessing future requirements.

---

# Key Takeaways

* DRY = Don't Repeat Yourself.
* Avoid duplicating business logic.
* Extract common behavior into reusable methods/classes.
* Makes code easier to maintain and scale.
* DRY is a core principle used in Spring Boot, Microservices, and Low-Level Design (LLD).

> "Every piece of knowledge should have one source of truth."
