# OOPS Quick Notes for PHP Backend Developer Interview (2+ Years)

## 1. What is OOP?

Object-Oriented Programming (OOP) is a programming paradigm where code is organized using:

- Classes
- Objects
- Properties
- Methods

### Benefits

- Reusability
- Maintainability
- Scalability
- Better code organization
- Easier testing

---

# 2. Class and Object

## Class

A blueprint/template for creating objects.

## Object

An instance of a class.

### Example

```php
class User
{
    public $name;

    public function greet()
    {
        return "Hello " . $this->name;
    }
}

$user = new User();
$user->name = "John";

echo $user->greet();
```

### Interview Answer

> Class is a blueprint and Object is the actual instance created from that blueprint.

---

# 3. Constructor

A special method automatically called when an object is created.

### Example

```php
class User
{
    private $name;

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }
}

$user = new User("John");

echo $user->getName();
```

### Interview Question

Why use constructor?

### Answer

To initialize object properties during object creation.

---

# 4. Encapsulation

## Definition

Binding data and methods together and restricting direct access to data.

### Access Modifiers

| Modifier | Accessible |
|-----------|------------|
| public | Everywhere |
| protected | Class + Child Class |
| private | Same Class Only |

### Example

```php
class BankAccount
{
    private $balance = 0;

    public function deposit($amount)
    {
        $this->balance += $amount;
    }

    public function getBalance()
    {
        return $this->balance;
    }
}

$account = new BankAccount();
$account->deposit(500);

echo $account->getBalance();
```

### Interview Answer

> Encapsulation hides internal data and exposes only necessary operations.

---

# 5. Abstraction

## Definition

Showing only essential information and hiding implementation details.

### Example

```php
abstract class Payment
{
    abstract public function pay($amount);
}

class CreditCardPayment extends Payment
{
    public function pay($amount)
    {
        echo "Paid $amount using Credit Card";
    }
}
```

### Real Life Example

- ATM Machine
- Car Driving

You know how to use it, not the internal implementation.

### Interview Answer

> Abstraction focuses on what an object does rather than how it does it.

---

# 6. Inheritance

## Definition

One class acquires properties and methods of another class.

### Example

```php
class Employee
{
    public function work()
    {
        return "Working";
    }
}

class Developer extends Employee
{
    public function code()
    {
        return "Writing Code";
    }
}

$dev = new Developer();

echo $dev->work();
echo $dev->code();
```

### Benefits

- Code Reusability
- Reduced Duplication

### Interview Answer

> Inheritance allows child classes to reuse functionality from parent classes.

---

# 7. Polymorphism

## Definition

Same method name behaves differently depending on the object.

### Example

```php
interface Payment
{
    public function pay($amount);
}

class PaypalPayment implements Payment
{
    public function pay($amount)
    {
        echo "Paid via Paypal";
    }
}

class StripePayment implements Payment
{
    public function pay($amount)
    {
        echo "Paid via Stripe";
    }
}
```

### Usage

```php
function processPayment(Payment $payment)
{
    $payment->pay(100);
}
```

### Interview Answer

> Polymorphism allows one interface to have multiple implementations.

---

# 8. Method Overriding

## Definition

Child class provides its own implementation of parent method.

### Example

```php
class Animal
{
    public function sound()
    {
        return "Animal Sound";
    }
}

class Dog extends Animal
{
    public function sound()
    {
        return "Bark";
    }
}
```

### Interview Answer

> Overriding changes parent behavior in child class.

---

# 9. Interface

## Definition

A contract that classes must implement.

### Example

```php
interface Logger
{
    public function log($message);
}

class FileLogger implements Logger
{
    public function log($message)
    {
        echo $message;
    }
}
```

### Interview Answer

> Interface defines what a class must do, not how.

---

# 10. Abstract Class vs Interface

| Abstract Class | Interface |
|---------------|------------|
| Can have implementation | No implementation (mostly) |
| Can have properties | Cannot have normal properties |
| Single inheritance | Multiple interfaces |
| Use for common behavior | Use for contract |

### Example

```php
abstract class Animal
{
    public function eat()
    {
        echo "Eating";
    }

    abstract public function sound();
}
```

### Common Interview Question

When to use Interface?

### Answer

When multiple classes must follow the same contract.

---

# 11. Static Keyword

## Definition

Belongs to class, not object.

### Example

```php
class MathHelper
{
    public static function add($a, $b)
    {
        return $a + $b;
    }
}

echo MathHelper::add(10, 20);
```

### Interview Answer

> Static methods can be called without creating objects.

---

# 12. Final Keyword

Prevents overriding or inheritance.

### Final Method

```php
class User
{
    final public function login()
    {
        echo "Login";
    }
}
```

### Final Class

```php
final class Config
{
}
```

---

# 13. Traits

## Definition

Used to reuse code across multiple classes.

### Example

```php
trait LoggerTrait
{
    public function log($message)
    {
        echo $message;
    }
}

class UserService
{
    use LoggerTrait;
}

class OrderService
{
    use LoggerTrait;
}
```

### Interview Answer

> Traits solve code reuse problems when multiple inheritance is not available.

---

# 14. Dependency Injection (Very Important)

## Definition

Providing dependencies from outside rather than creating them inside.

### Bad Example

```php
class UserService
{
    private $db;

    public function __construct()
    {
        $this->db = new Database();
    }
}
```

### Good Example

```php
class UserService
{
    private $db;

    public function __construct(Database $db)
    {
        $this->db = $db;
    }
}
```

### Laravel Relation

Laravel Service Container automatically resolves dependencies.

### Interview Answer

> Dependency Injection reduces tight coupling and improves testability.

---

# 15. Composition vs Inheritance

## Composition

"Has-A" Relationship

```php
class Engine
{
}

class Car
{
    private $engine;

    public function __construct(Engine $engine)
    {
        $this->engine = $engine;
    }
}
```

## Inheritance

"Is-A" Relationship

```php
class Vehicle
{
}

class Car extends Vehicle
{
}
```

### Interview Answer

> Prefer composition over inheritance because it provides better flexibility.

---

# 16. Magic Methods (PHP)

Frequently Asked

### Constructor

```php
__construct()
```

### Destructor

```php
__destruct()
```

### Getter

```php
__get()
```

### Setter

```php
__set()
```

### Call

```php
__call()
```

### ToString

```php
__toString()
```

### Interview Question

What are magic methods?

### Answer

Special predefined methods automatically invoked by PHP in specific situations.

---

# 17. SOLID Principles (Most Asked)

## S - Single Responsibility Principle

One class should have only one reason to change.

```php
UserService
EmailService
```

Separate responsibilities.

---

## O - Open Closed Principle

Open for extension, closed for modification.

```php
interface Payment
{
    public function pay();
}
```

Add new payment methods without changing old code.

---

## L - Liskov Substitution Principle

Child class should replace parent without breaking behavior.

---

## I - Interface Segregation Principle

Avoid large interfaces.

Bad:

```php
interface Worker
{
    work();
    eat();
}
```

Good:

```php
interface Workable
{
    work();
}

interface Eatable
{
    eat();
}
```

---

## D - Dependency Inversion Principle

Depend on abstractions, not concrete classes.

```php
interface Logger
{
}

class FileLogger implements Logger
{
}
```

---

# Laravel Specific OOP Questions

## Service Container

Used for Dependency Injection.

```php
app(UserService::class);
```

---

## Service Provider

Registers services into container.

```php
public function register()
{
}
```

---

## Repository Pattern

Separates database logic from business logic.

```php
UserController
    ↓
UserService
    ↓
UserRepository
```

---

## Why use Interfaces in Laravel?

- Loose Coupling
- Easy Testing
- Better Maintainability

Example:

```php
UserRepositoryInterface
```

can have:

```php
MysqlUserRepository
MongoUserRepository
```

without changing service code.

---

# Most Common Interview Questions

### What are the 4 pillars of OOP?

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

---

### Difference between Abstract Class and Interface?

Abstract class can have implementation and state.
Interface only defines contract.

---

### Difference between Composition and Inheritance?

Composition = Has-A

Inheritance = Is-A

Prefer Composition.

---

### Why Dependency Injection?

- Loose Coupling
- Better Testing
- Easier Maintenance

---

### What is Polymorphism?

One interface, multiple implementations.

---

### What is Encapsulation?

Hiding data and exposing controlled access.

---

### What is Method Overriding?

Changing parent method behavior in child class.

---

### What are Traits?

Reusable code blocks shared across classes.

---

# One-Line Revision Before Interview

Class → Blueprint

Object → Instance

Encapsulation → Hide data

Abstraction → Hide implementation

Inheritance → Reuse code

Polymorphism → Many forms

Interface → Contract

Abstract Class → Partial implementation

Trait → Reusable methods

Dependency Injection → Inject dependency

Composition → Has-A

Inheritance → Is-A

SOLID → Clean architecture principles
