# PHP & Laravel Interview Notes (2+ Years Backend Developer)

> Quick Revision Notes for Interviews
> Experience Level: 2+ Years Laravel Backend Developer

---

# PHP QUESTIONS

## 1. What is PHP?

PHP (Hypertext Preprocessor) is a server-side scripting language primarily used for web development. It executes on the server and generates dynamic HTML content before sending it to the browser.

---

## 2. Difference between `==` and `===` in PHP?

`==` compares values after type conversion, whereas `===` compares both value and datatype.

### Example

```php
var_dump(5 == "5");   // true
var_dump(5 === "5");  // false
```

Use `===` whenever possible to avoid unexpected type coercion issues.

---

## 3. What are Traits in PHP?

Traits allow code reuse across multiple classes without using inheritance. They help reduce duplication when multiple classes need the same functionality.

### Example

```php
trait Logger
{
    public function log($message)
    {
        echo $message;
    }
}

class User
{
    use Logger;
}
```

Traits are especially useful when a class already extends another class.

---

## 4. What is the difference between Abstract Class and Interface?

An abstract class can contain both implemented and abstract methods, while an interface only defines method contracts.

A class can implement multiple interfaces but can extend only one abstract class.

---

## 5. What are Magic Methods in PHP?

Magic methods are predefined methods starting with double underscores.

Common examples:

```php
__construct()
__destruct()
__get()
__set()
__call()
__toString()
```

These methods provide hooks into object behavior.

---

## 6. Explain Dependency Injection.

Dependency Injection means passing dependencies to a class rather than creating them inside the class.

### Example

```php
class UserService
{
    public function __construct(UserRepository $repo)
    {
        $this->repo = $repo;
    }
}
```

It improves testability, maintainability, and loose coupling.

---

## 7. What is Composer?

Composer is PHP's dependency management tool. It installs and manages third-party packages.

### Commands

```bash
composer install
composer update
composer require package-name
```

Composer also supports PSR-4 autoloading.

---

## 8. What is PSR?

PSR stands for PHP Standards Recommendation. These standards ensure consistency among PHP projects.

Popular standards:

* PSR-4 (Autoloading)
* PSR-12 (Coding Style)
* PSR-7 (HTTP Messages)

---

## 9. Explain Namespaces.

Namespaces prevent class name conflicts by grouping related classes.

### Example

```php
namespace App\Services;

class UserService
{
}
```

They are heavily used in Laravel applications.

---

## 10. What is Autoloading?

Autoloading automatically loads classes when required without manually including files.

Composer uses PSR-4 autoloading:

```json
"autoload": {
    "psr-4": {
        "App\\": "app/"
    }
}
```

---

# LARAVEL QUESTIONS

---

## 11. What is Laravel?

Laravel is a PHP framework following the MVC architecture. It provides tools for routing, authentication, ORM, queues, caching, and API development.

It helps developers build scalable and maintainable web applications quickly.

---

## 12. Explain Laravel Request Lifecycle.

1. Request enters through `public/index.php`
2. Loads Composer autoloader
3. Bootstraps application
4. Service Providers are loaded
5. Middleware executes
6. Route is matched
7. Controller action executes
8. Response returned

Understanding this lifecycle helps during debugging.

---

## 13. What is Service Container?

The Service Container is Laravel's dependency injection container.

### Example

```php
app(UserService::class);
```

It automatically resolves dependencies and manages object creation.

---

## 14. What are Service Providers?

Service Providers are the central place for configuring services in Laravel.

Common methods:

```php
register()
boot()
```

They bind services into the container and bootstrap application functionality.

---

## 15. What is Middleware?

Middleware filters HTTP requests before they reach controllers.

Examples:

* Authentication
* Logging
* CORS
* Rate Limiting

### Create Middleware

```bash
php artisan make:middleware CheckUser
```

---

## 16. Difference Between GET and POST Routes?

GET is generally used for retrieving data, while POST is used for creating or modifying data.

```php
Route::get('/users', ...);

Route::post('/users', ...);
```

POST requests are generally protected using CSRF tokens.

---

## 17. What is Route Model Binding?

Laravel automatically resolves model instances from route parameters.

### Example

```php
Route::get('/users/{user}', function(User $user){
    return $user;
});
```

It reduces repetitive database queries.

---

## 18. What is Eloquent ORM?

Eloquent is Laravel's Object Relational Mapper.

Instead of writing SQL directly, developers interact with models.

```php
User::all();
User::find(1);
```

It simplifies database operations.

---

## 19. Difference Between `find()` and `first()`?

```php
User::find(1);
```

Searches using primary key.

```php
User::where('email', $email)->first();
```

Returns first matching record based on conditions.

---

## 20. What is Mass Assignment?

Mass assignment allows assigning multiple attributes at once.

```php
User::create($request->all());
```

To prevent vulnerabilities:

```php
protected $fillable = [
    'name',
    'email'
];
```

---

## 21. What is N+1 Query Problem?

N+1 occurs when related data is loaded separately for each record.

Bad:

```php
$users = User::all();

foreach ($users as $user) {
    echo $user->posts;
}
```

Good:

```php
$users = User::with('posts')->get();
```

Eager loading reduces database queries.

---

## 22. Difference Between Eager Loading and Lazy Loading?

Lazy Loading loads relations when accessed.

```php
$user->posts;
```

Eager Loading fetches relations beforehand.

```php
User::with('posts')->get();
```

Eager loading improves performance.

---

## 23. Explain Laravel Relationships.

### One To One

```php
hasOne()
belongsTo()
```

### One To Many

```php
hasMany()
belongsTo()
```

### Many To Many

```php
belongsToMany()
```

Relationships simplify querying associated records.

---

## 24. What are Accessors and Mutators?

Accessors modify values when retrieving data.

```php
public function getNameAttribute($value)
{
    return strtoupper($value);
}
```

Mutators modify values before saving.

```php
public function setNameAttribute($value)
{
    $this->attributes['name'] = ucfirst($value);
}
```

---

## 25. What are Laravel Events and Listeners?

Events represent actions happening in the application.

Listeners respond to those actions.

Example:

```php
UserRegistered
SendWelcomeEmail
```

This promotes loose coupling.

---

## 26. What are Queues?

Queues execute time-consuming tasks asynchronously.

Examples:

* Email Sending
* PDF Generation
* Notifications

```bash
php artisan queue:work
```

Queues improve application responsiveness.

---

## 27. Difference Between Jobs and Queues?

A Job is a task definition.

A Queue is the mechanism that processes jobs.

Jobs are dispatched to queues for background execution.

---

## 28. What is Laravel Scheduler?

Scheduler replaces multiple cron jobs.

### Example

```php
$schedule->command('emails:send')
         ->daily();
```

Only one cron entry is required.

---

## 29. What is Caching?

Caching stores frequently used data temporarily.

```php
Cache::put('users', $data, 60);
```

Supported drivers:

* Redis
* Memcached
* File
* Database

Caching reduces database load.

---

## 30. Why Redis is Used?

Redis is an in-memory datastore used for:

* Caching
* Queues
* Sessions
* Rate Limiting

It provides very high performance compared to database storage.

---

## 31. What is API Resource?

API Resources transform model data into consistent JSON responses.

```php
return new UserResource($user);
```

They provide better control over API output.

---

## 32. What is Authentication?

Authentication verifies user identity.

Laravel supports:

* Session Auth
* Sanctum
* Passport
* JWT

Authentication ensures only valid users access protected resources.

---

## 33. Difference Between Sanctum and Passport?

### Sanctum

* Lightweight
* SPA authentication
* Simple API tokens

### Passport

* OAuth2 implementation
* Third-party authentication
* More complex authorization

Sanctum is preferred for most modern APIs.

---

## 34. What is Rate Limiting?

Rate limiting restricts API requests.

Example:

```php
Route::middleware('throttle:60,1');
```

This prevents abuse and protects resources.

---

## 35. What is Database Migration?

Migrations are version-controlled database schema changes.

```bash
php artisan make:migration
```

They allow database changes to be tracked in code.

---

## 36. What are Seeders?

Seeders populate sample or default data.

```bash
php artisan db:seed
```

Useful for development and testing environments.

---

## 37. What is Factory?

Factories generate fake model data.

```php
User::factory()->count(10)->create();
```

Often used with testing and seeding.

---

## 38. What is Laravel Policy?

Policies manage authorization logic for models.

```php
UserPolicy
PostPolicy
```

They centralize access-control rules.

---

## 39. Difference Between Authentication and Authorization?

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

Both are critical for application security.

---

## 40. What are Common Laravel Performance Optimizations?

### Cache Config

```bash
php artisan config:cache
```

### Cache Routes

```bash
php artisan route:cache
```

### Cache Views

```bash
php artisan view:cache
```

Also use eager loading, Redis caching, indexing, and queues to improve performance.

---

# SQL QUESTIONS FOR LARAVEL DEVELOPERS

## 41. Difference Between INNER JOIN and LEFT JOIN?

INNER JOIN returns matching records from both tables.

LEFT JOIN returns all records from the left table and matching records from the right table.

Understanding joins is essential for API and reporting queries.

---

## 42. What is Indexing?

Indexes improve query performance by reducing table scans.

Example:

```sql
CREATE INDEX idx_email
ON users(email);
```

However, excessive indexes increase write overhead.

---

## 43. What is Database Transaction?

Transactions ensure atomic operations.

```php
DB::transaction(function () {
    // operations
});
```

If one query fails, all changes are rolled back.

---

## 44. Explain ACID Properties.

* Atomicity
* Consistency
* Isolation
* Durability

These properties ensure reliable database transactions.

---

## 45. What is Deadlock?

A deadlock occurs when two transactions wait indefinitely for each other's resources.

It can be reduced using proper locking strategies and shorter transactions.

---

# REAL PROJECT QUESTIONS

## 46. How did you optimize a slow API?

I identified N+1 query issues using Laravel Debugbar, implemented eager loading, added database indexes, and cached frequently requested data using Redis. These improvements significantly reduced response times.

---

## 47. How do you secure Laravel APIs?

I use authentication (Sanctum/JWT), request validation, rate limiting, HTTPS, CSRF protection where applicable, and proper authorization policies to secure API endpoints.

---

## 48. How do you handle large file uploads?

I validate file types and sizes, store files using Laravel Storage, process uploads asynchronously with queues when necessary, and use cloud storage such as S3 for scalability.

---

## 49. Explain Repository Pattern.

The Repository Pattern abstracts database logic from business logic.

Controllers interact with repositories instead of directly querying models, making the code easier to test and maintain.

---

## 50. Explain SOLID Principles.

SOLID consists of five object-oriented design principles:

* Single Responsibility
* Open/Closed
* Liskov Substitution
* Interface Segregation
* Dependency Inversion

Following SOLID principles improves scalability, maintainability, and testability.

---

# FINAL INTERVIEW REVISION

Must Know Topics:

✅ PHP OOP
✅ Traits
✅ Dependency Injection
✅ Composer
✅ Laravel Lifecycle
✅ Service Container
✅ Service Providers
✅ Middleware
✅ Routing
✅ Eloquent ORM
✅ Relationships
✅ Accessors & Mutators
✅ API Resources
✅ Authentication
✅ Sanctum
✅ Queues
✅ Jobs
✅ Scheduler
✅ Redis
✅ Caching
✅ Migrations
✅ Seeders
✅ Factories
✅ Policies
✅ SQL Joins
✅ Transactions
✅ Indexing
✅ Repository Pattern
✅ SOLID Principles
✅ API Optimization
✅ Security Best Practices

# ADVANCED LARAVEL INTERVIEW QUESTIONS (51-100)

---

## 51. What is Inversion of Control (IoC)?

Inversion of Control is a design principle where object creation and dependency management are delegated to a container instead of being handled manually inside classes.

Laravel implements IoC through the Service Container, which automatically resolves dependencies and promotes loose coupling.

---

## 52. Difference Between IoC Container and Service Container?

The IoC Container is the concept of managing dependencies, while Laravel's Service Container is the actual implementation of that concept.

The Service Container resolves classes, injects dependencies, and manages object lifecycles.

---

## 53. What is Dependency Inversion Principle?

Dependency Inversion states that high-level modules should depend on abstractions rather than concrete implementations.

### Example

```php
interface PaymentGateway
{
    public function pay();
}

class StripeGateway implements PaymentGateway
{
    public function pay()
    {
    }
}
```

This makes applications easier to extend and test.

---

## 54. How do you bind a class in Laravel Service Container?

You can bind services in a service provider.

```php
$this->app->bind(
    PaymentGateway::class,
    StripeGateway::class
);
```

Whenever Laravel needs `PaymentGateway`, it resolves `StripeGateway`.

---

## 55. Difference Between bind() and singleton()?

`bind()` creates a new instance every time it is resolved.

`singleton()` creates only one instance and reuses it throughout the application lifecycle.

```php
$this->app->singleton(
    UserService::class,
    function () {
        return new UserService();
    }
);
```

---

## 56. What is Contextual Binding?

Contextual binding allows different implementations of the same interface to be injected depending on where they are needed.

This is useful when multiple classes require different implementations of a common contract.

---

## 57. What are Facades in Laravel?

Facades provide a static interface to Laravel services stored in the service container.

Example:

```php
Cache::get('users');
```

Behind the scenes, facades resolve instances from the service container.

---

## 58. What are the disadvantages of Facades?

Heavy use of facades can hide dependencies and make unit testing harder.

Constructor dependency injection is often preferred because dependencies become explicit.

---

## 59. Difference Between Facade and Helper Function?

Facades access services through the container and can be mocked in tests.

Helpers are simple global functions and do not provide dependency resolution capabilities.

---

## 60. What is a Custom Facade?

A custom facade provides a static interface for your own services.

It helps expose business logic in a clean and Laravel-friendly way.

---

## 61. What is Laravel Observer?

Observers group model event listeners into a dedicated class.

Example events:

```php
created
updated
deleted
saving
saved
```

Observers help keep models clean and maintainable.

---

## 62. When would you use an Observer?

Observers are useful for actions triggered by model events, such as sending notifications, generating logs, or updating related records.

This prevents business logic from cluttering controllers or models.

---

## 63. Difference Between Events and Observers?

Observers listen specifically to model events.

Events are broader and can represent any business action occurring in the application.

---

## 64. What are Laravel Notifications?

Notifications provide a unified API for sending messages through multiple channels.

Supported channels include:

* Email
* Database
* Slack
* SMS

They simplify user communication workflows.

---

## 65. What are Notification Channels?

Channels define where notifications are delivered.

Example:

```php
public function via($notifiable)
{
    return ['mail', 'database'];
}
```

A single notification can be delivered through multiple channels.

---

## 66. What is Broadcasting?

Broadcasting allows Laravel events to be pushed to client-side applications in real time.

Common use cases include chat systems, live notifications, and dashboards.

---

## 67. What is Laravel Echo?

Laravel Echo is a JavaScript library used for listening to broadcast events.

It works with WebSockets and integrates well with Laravel broadcasting.

---

## 68. What are WebSockets?

WebSockets provide full-duplex communication between client and server.

Unlike HTTP requests, they maintain a persistent connection for real-time applications.

---

## 69. What is Laravel Horizon?

Horizon provides a dashboard for monitoring Redis queues.

It offers insights into failed jobs, throughput, queue performance, and worker status.

---

## 70. Why use Horizon instead of queue:work?

Horizon provides monitoring, metrics, balancing, and failure tracking.

It is particularly useful in production environments handling large job volumes.

---

## 71. What are Failed Jobs?

Failed jobs are queued jobs that throw exceptions during execution.

Laravel stores them for later inspection and retrying.

```bash
php artisan queue:failed
```

---

## 72. How do you retry failed jobs?

```bash
php artisan queue:retry all
```

Retrying failed jobs prevents permanent data loss from temporary failures.

---

## 73. What is Job Chaining?

Job chaining allows multiple jobs to execute sequentially.

```php
Bus::chain([
    new ProcessOrder(),
    new GenerateInvoice(),
    new SendEmail(),
])->dispatch();
```

Each job executes only if the previous one succeeds.

---

## 74. What is Batch Processing?

Batch processing allows groups of jobs to be managed together.

Laravel provides progress tracking, cancellation, and monitoring capabilities for batches.

---

## 75. What is Rate Limiting in APIs?

Rate limiting controls how many requests a client can make within a specific period.

This protects APIs from abuse and excessive resource consumption.

---

## 76. What is API Versioning?

API versioning allows backward compatibility while introducing new features.

Example:

```php
/api/v1/users
/api/v2/users
```

Clients can migrate gradually without breaking existing integrations.

---

## 77. What is API Resource Collection?

Resource collections transform multiple records into standardized API responses.

```php
return UserResource::collection($users);
```

This ensures consistency across endpoints.

---

## 78. Difference Between Resource and Resource Collection?

A Resource transforms a single model.

A Resource Collection transforms multiple models while maintaining a consistent response structure.

---

## 79. What is API Pagination?

Pagination divides large datasets into manageable chunks.

```php
User::paginate(15);
```

It improves performance and user experience.

---

## 80. Difference Between paginate() and simplePaginate()?

`paginate()` performs a count query.

`simplePaginate()` skips the count query and is faster for large datasets.

---

## 81. What is Cursor Pagination?

Cursor pagination uses unique identifiers instead of offsets.

```php
User::cursorPaginate(10);
```

It performs better on large datasets than offset-based pagination.

---

## 82. What is Soft Delete?

Soft deletes mark records as deleted without removing them from the database.

```php
use SoftDeletes;
```

This allows data recovery and auditing.

---

## 83. How do you retrieve soft-deleted records?

```php
User::withTrashed()->get();
```

Laravel excludes soft-deleted records by default.

---

## 84. Difference Between delete() and forceDelete()?

`delete()` performs a soft delete.

`forceDelete()` permanently removes the record from the database.

---

## 85. What is Laravel Scout?

Laravel Scout provides full-text search functionality.

It integrates with search engines such as:

* Meilisearch
* Algolia

Search performance is significantly better than SQL LIKE queries.

---

## 86. What is Laravel Telescope?

Telescope is Laravel's debugging and monitoring tool.

It tracks requests, exceptions, jobs, database queries, cache operations, and more.

---

## 87. Why should Telescope not be enabled in production?

Telescope records large amounts of application data.

Keeping it enabled in production may affect performance and expose sensitive information.

---

## 88. What is Lazy Collection?

Lazy Collections process large datasets using generators.

```php
User::cursor();
```

This reduces memory consumption significantly.

---

## 89. What is Chunking?

Chunking processes records in batches.

```php
User::chunk(100, function ($users) {
});
```

It prevents memory exhaustion when handling large datasets.

---

## 90. Difference Between chunk() and cursor()?

`chunk()` loads batches into memory.

`cursor()` loads one record at a time using generators.

Cursor is generally more memory efficient.

---

## 91. What is Repository Pattern?

Repository Pattern abstracts data access logic from business logic.

It improves testability and allows data sources to change without affecting controllers.

---

## 92. What is Service Layer Pattern?

The Service Layer contains business logic and acts as a bridge between controllers and repositories.

Controllers remain thin and focused on request handling.

---

## 93. Why keep controllers thin?

Thin controllers improve readability, testing, and maintainability.

Business logic should be placed in services or dedicated classes.

---

## 94. What is Action Pattern?

The Action Pattern encapsulates a single business operation into a dedicated class.

Example:

```php
CreateOrderAction
SendInvoiceAction
```

It improves code organization and reusability.

---

## 95. What is DTO (Data Transfer Object)?

DTOs are objects used to transfer structured data between layers.

They improve type safety and reduce dependency on raw request arrays.

---

## 96. What is Laravel Macro?

Macros allow developers to add custom methods to Laravel classes dynamically.

Example:

```php
Response::macro('success', function ($data) {
});
```

They help standardize common functionality.

---

## 97. What is Custom Cast?

Custom casts transform attribute values automatically.

Example:

```php
protected $casts = [
    'settings' => 'array'
];
```

They simplify data conversion logic.

---

## 98. What is Attribute Casting?

Casting converts database values into specific PHP types.

Examples:

```php
boolean
array
datetime
json
```

This improves consistency when working with model attributes.

---

## 99. What is Laravel Package Development?

Package development involves creating reusable Laravel modules that can be shared across projects.

Packages often include service providers, routes, migrations, and configurations.

---

## 100. How would you explain your Laravel architecture in an interview?

A maintainable Laravel architecture typically follows Controllers → Services → Repositories → Models. Business logic stays in services, data access stays in repositories, and controllers remain thin.

This separation of concerns improves scalability, testing, readability, and long-term maintenance.

# PHP OOP, DESIGN PATTERNS, TESTING & SECURITY (101-150)

---

## 101. What is Object-Oriented Programming (OOP)?

OOP is a programming paradigm that organizes code into objects containing properties and methods. It improves code reusability, maintainability, and scalability.

The four main principles of OOP are Encapsulation, Inheritance, Polymorphism, and Abstraction.

---

## 102. What is Encapsulation?

Encapsulation means hiding internal implementation details and exposing only what is necessary through public methods.

It helps protect data integrity and prevents unintended modifications from external code.

### Example

```php
class User
{
    private string $name;

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}
```

---

## 103. What is Inheritance?

Inheritance allows one class to acquire properties and methods from another class.

It promotes code reuse and establishes parent-child relationships between classes.

---

## 104. What is Polymorphism?

Polymorphism allows the same interface or method name to behave differently depending on the object implementation.

This makes systems more flexible and extensible.

---

## 105. What is Abstraction?

Abstraction hides implementation details and exposes only essential functionality.

It helps reduce complexity and focuses on what an object does rather than how it does it.

---

## 106. What are Access Modifiers?

Access modifiers control visibility of properties and methods.

```php
public
protected
private
```

Using proper access modifiers helps enforce encapsulation.

---

## 107. Difference Between public, protected, and private?

`public` members can be accessed anywhere.

`protected` members are accessible within the class and its child classes, while `private` members are accessible only inside the same class.

---

## 108. What is Method Overriding?

Method overriding occurs when a child class provides its own implementation of a parent class method.

It allows customized behavior while maintaining a common interface.

---

## 109. What is Method Overloading in PHP?

PHP does not support traditional method overloading like Java.

Similar behavior can be achieved using magic methods such as `__call()`.

---

## 110. What is Late Static Binding?

Late Static Binding allows static method calls to reference the called class instead of the class where the method was originally defined.

It is achieved using the `static` keyword.

---

## 111. Difference Between self and static?

`self` refers to the current class where the method is defined.

`static` refers to the class from which the method is called, supporting inheritance behavior.

---

## 112. What is a Final Class?

A final class cannot be extended by other classes.

```php
final class PaymentService
{
}
```

This prevents modification of critical functionality.

---

## 113. What is a Final Method?

A final method cannot be overridden in child classes.

It ensures important behavior remains unchanged across inheritance hierarchies.

---

## 114. What is an Anonymous Class?

Anonymous classes allow creation of one-time-use classes without explicitly naming them.

They are useful for small implementations and testing scenarios.

---

## 115. What is an Iterable?

An iterable is any data structure that can be traversed using a loop.

Examples include arrays, collections, and generators.

---

## 116. What is a Generator?

Generators produce values one at a time instead of loading everything into memory.

They are highly efficient for processing large datasets.

### Example

```php
function numbers()
{
    yield 1;
    yield 2;
    yield 3;
}
```

---

## 117. What is the yield Keyword?

The `yield` keyword pauses function execution and returns a value without terminating the function.

This enables lazy loading of data.

---

## 118. What is a Closure?

A closure is an anonymous function that can access variables from its parent scope.

Closures are heavily used throughout Laravel collections and routes.

---

## 119. What is a Callback Function?

A callback is a function passed as an argument to another function.

It allows dynamic execution of logic.

---

## 120. What are PHP Attributes?

Attributes provide metadata for classes, methods, and properties.

They are introduced in PHP 8 and replace many annotation-based approaches.

---

# SOLID PRINCIPLES

---

## 121. What is Single Responsibility Principle (SRP)?

A class should have only one reason to change.

Keeping responsibilities separate makes code easier to maintain and test.

---

## 122. What is Open/Closed Principle (OCP)?

Software entities should be open for extension but closed for modification.

New functionality should be added without altering existing working code.

---

## 123. What is Liskov Substitution Principle (LSP)?

Child classes should be replaceable for parent classes without breaking application behavior.

Violating LSP often causes unexpected runtime issues.

---

## 124. What is Interface Segregation Principle (ISP)?

Clients should not be forced to depend on methods they do not use.

Smaller, focused interfaces are preferred over large generic interfaces.

---

## 125. What is Dependency Inversion Principle (DIP)?

High-level modules should depend on abstractions rather than concrete implementations.

Laravel's Service Container is built around this principle.

---

# DESIGN PATTERNS

---

## 126. What is a Design Pattern?

A design pattern is a proven solution to recurring software design problems.

They improve maintainability, scalability, and communication among developers.

---

## 127. What is Singleton Pattern?

Singleton ensures only one instance of a class exists.

Laravel's `singleton()` binding implements this pattern.

---

## 128. What is Factory Pattern?

Factory Pattern creates objects without exposing object creation logic.

Laravel model factories are based on this concept.

---

## 129. What is Repository Pattern?

Repository Pattern separates data access logic from business logic.

It simplifies testing and supports changing storage implementations later.

---

## 130. What is Strategy Pattern?

Strategy Pattern allows selecting different algorithms at runtime.

Payment gateways are a common example of this pattern.

---

## 131. What is Observer Pattern?

Observer Pattern notifies dependent objects when an event occurs.

Laravel Observers and Events are examples of this pattern.

---

## 132. What is Adapter Pattern?

Adapter Pattern allows incompatible interfaces to work together.

It is useful when integrating third-party services.

---

## 133. What is Builder Pattern?

Builder Pattern constructs complex objects step by step.

Laravel's Query Builder follows this pattern.

---

## 134. What is Dependency Injection Pattern?

Dependencies are supplied externally rather than created internally.

This reduces coupling and improves testability.

---

## 135. What is Command Pattern?

Command Pattern encapsulates actions into separate objects.

Laravel Jobs are a practical example of this pattern.

---

# TESTING

---

## 136. What is Unit Testing?

Unit testing verifies individual components in isolation.

It ensures business logic behaves correctly under different conditions.

---

## 137. What is Feature Testing?

Feature tests validate application behavior across multiple layers.

They commonly test routes, middleware, controllers, and database interactions.

---

## 138. What is PHPUnit?

PHPUnit is the most widely used testing framework for PHP applications.

Laravel includes PHPUnit by default.

---

## 139. How do you run tests?

```bash
php artisan test
```

or

```bash
vendor/bin/phpunit
```

Tests should be run frequently during development.

---

## 140. What is Mocking?

Mocking replaces real dependencies with simulated objects.

It allows isolated testing without relying on external systems.

---

## 141. Why use Mocking?

Mocking improves test speed and reliability.

It also prevents dependencies such as APIs or databases from affecting test outcomes.

---

## 142. What is Test Driven Development (TDD)?

TDD follows the cycle:

1. Write Test
2. Watch Test Fail
3. Write Code
4. Refactor

This approach encourages better software design.

---

## 143. Difference Between Unit Test and Feature Test?

Unit tests verify isolated logic.

Feature tests validate complete application workflows involving multiple components.

---

## 144. What is Database Refresh During Testing?

Laravel provides database refreshing to ensure a clean state before tests.

```php
use RefreshDatabase;
```

This prevents tests from affecting one another.

---

# SECURITY

---

## 145. What is CSRF?

Cross-Site Request Forgery occurs when malicious websites force users to perform unintended actions.

Laravel automatically protects forms using CSRF tokens.

---

## 146. What is XSS?

Cross-Site Scripting allows attackers to inject malicious JavaScript into web pages.

Laravel's Blade templates escape output by default to prevent XSS attacks.

---

## 147. What is SQL Injection?

SQL Injection occurs when attackers manipulate database queries through user input.

Laravel Query Builder and Eloquent use parameter binding to mitigate this risk.

---

## 148. What is Rate Limiting?

Rate limiting restricts request frequency to protect applications from abuse and denial-of-service attacks.

Laravel provides built-in throttle middleware.

---

## 149. What is Password Hashing?

Password hashing converts passwords into irreversible hashes before storage.

Laravel uses bcrypt and Argon2 to securely hash passwords.

---

## 150. How do you secure a Laravel Application?

A secure Laravel application uses authentication, authorization, validation, HTTPS, CSRF protection, rate limiting, password hashing, secure environment variables, and regular dependency updates.

Security should be considered throughout the entire development lifecycle rather than as an afterthought.

# ADVANCED BACKEND, REDIS, PERFORMANCE, DEVOPS & REAL PROJECT QUESTIONS (151-200)

---

## 151. What is Redis?

Redis is an in-memory key-value datastore commonly used for caching, session storage, queues, and real-time analytics.

Because data is stored primarily in memory, Redis operations are significantly faster than traditional database queries.

---

## 152. Why is Redis Faster Than MySQL?

Redis stores data in memory, while MySQL typically reads from disk storage.

This reduces I/O operations and allows Redis to serve requests with extremely low latency.

---

## 153. Common Redis Use Cases?

Redis is commonly used for:

* Caching
* Queue Management
* Session Storage
* Rate Limiting
* Real-Time Counters

These use cases reduce database load and improve application performance.

---

## 154. How Does Laravel Use Redis?

Laravel can use Redis for caching, queue processing, session management, and broadcasting.

Redis integrates seamlessly through Laravel's cache and queue drivers.

---

## 155. What is Cache?

Cache is temporary storage for frequently accessed data.

Caching reduces expensive database queries and improves response times for end users.

---

## 156. What is Cache Invalidation?

Cache invalidation ensures stale data is removed or updated when underlying data changes.

Poor cache invalidation strategies can cause users to see outdated information.

---

## 157. Difference Between Cache::remember() and Cache::put()?

`Cache::put()` directly stores data in cache.

`Cache::remember()` first checks the cache and only executes the callback when data is missing.

### Example

```php id="n4r2k1"
$users = Cache::remember(
    'users',
    3600,
    fn() => User::all()
);
```

---

## 158. What is Cache Stampede?

A cache stampede occurs when many requests simultaneously regenerate expired cache data.

Techniques such as cache warming and staggered expiration help reduce this issue.

---

## 159. What is Database Indexing?

Indexes improve query performance by creating optimized lookup structures.

They reduce the amount of data scanned during query execution.

---

## 160. When Should You Add an Index?

Indexes should be added to columns frequently used in:

* WHERE clauses
* JOIN conditions
* ORDER BY clauses
* Foreign keys

Proper indexing significantly improves database performance.

---

## 161. What Are the Drawbacks of Too Many Indexes?

Although indexes improve read performance, they increase storage usage and slow insert, update, and delete operations.

Only necessary indexes should be created.

---

## 162. How Do You Find Slow Queries?

Common approaches include:

* Laravel Telescope
* Debugbar
* MySQL EXPLAIN
* Query Logs

These tools help identify bottlenecks and optimize performance.

---

## 163. What is EXPLAIN in SQL?

EXPLAIN shows how MySQL executes a query.

It helps developers understand index usage, table scans, and query execution plans.

### Example

```sql id="5s7gqp"
EXPLAIN
SELECT * FROM users
WHERE email = 'test@example.com';
```

---

## 164. What is the N+1 Query Problem?

N+1 occurs when one query fetches parent records and additional queries fetch related records individually.

Eager loading is the preferred solution in Laravel.

---

## 165. How Do You Solve N+1 Queries?

Use eager loading:

```php id="v3m8ky"
User::with('posts')->get();
```

This loads relationships efficiently using fewer database queries.

---

## 166. What is Query Optimization?

Query optimization involves improving SQL performance through indexing, query restructuring, caching, and minimizing unnecessary database operations.

Efficient queries are critical for scalable applications.

---

## 167. What is Database Sharding?

Sharding distributes data across multiple database servers.

It improves scalability when a single database can no longer handle increasing load.

---

## 168. What is Database Replication?

Replication copies data from a primary database to one or more replicas.

Read-heavy workloads often benefit from replication architectures.

---

## 169. What is Read/Write Splitting?

Write operations go to the primary database, while read operations are handled by replicas.

This improves performance and scalability in large applications.

---

## 170. What is Connection Pooling?

Connection pooling reuses database connections instead of creating new ones repeatedly.

This reduces overhead and improves application performance.

---

# QUEUES & HORIZON

---

## 171. Why Use Queues?

Queues move time-consuming tasks into the background.

This allows API responses to remain fast while processing heavy operations asynchronously.

---

## 172. What Tasks Should Be Queued?

Common queue candidates include:

* Sending Emails
* SMS Notifications
* Report Generation
* Image Processing
* PDF Creation

Background processing improves user experience.

---

## 173. What is Queue Retry Logic?

Retry logic automatically re-executes failed jobs.

This helps recover from temporary issues such as network failures or third-party service downtime.

---

## 174. What is Queue Timeout?

Queue timeout defines the maximum execution time for a job.

Jobs exceeding the timeout are marked as failed.

---

## 175. What is Laravel Horizon?

Horizon provides a dashboard for monitoring Redis queues.

It tracks workers, throughput, failed jobs, and queue health.

---

# DOCKER

---

## 176. What is Docker?

Docker is a containerization platform that packages applications and dependencies into isolated environments.

Containers ensure consistent behavior across development, staging, and production.

---

## 177. Why Use Docker?

Docker solves environment inconsistency problems.

Developers can run identical application stacks regardless of operating system differences.

---

## 178. What is a Docker Image?

A Docker image is a blueprint used to create containers.

Images contain application code, runtime dependencies, and configuration.

---

## 179. What is a Docker Container?

A container is a running instance of a Docker image.

Multiple containers can be created from the same image.

---

## 180. What is Docker Compose?

Docker Compose manages multiple containers through a single configuration file.

Example services include Laravel, MySQL, Redis, and Nginx.

---

# CI/CD

---

## 181. What is CI/CD?

CI/CD stands for Continuous Integration and Continuous Deployment.

It automates testing, building, and deployment processes.

---

## 182. What is Continuous Integration?

Continuous Integration automatically validates code changes through tests and quality checks.

This helps detect issues early during development.

---

## 183. What is Continuous Deployment?

Continuous Deployment automatically releases verified changes into production environments.

It reduces manual deployment effort and improves delivery speed.

---

## 184. What CI/CD Tools Have You Used?

Common tools include:

* GitHub Actions
* GitLab CI/CD
* Jenkins
* Bitbucket Pipelines

The choice depends on project requirements and team workflows.

---

## 185. What Happens During a Typical Deployment?

Typical deployment steps include:

1. Pull Code
2. Install Dependencies
3. Run Tests
4. Run Migrations
5. Clear/Build Cache
6. Restart Services

Automation reduces deployment risks.

---

# AWS & CLOUD BASICS

---

## 186. What is AWS?

AWS (Amazon Web Services) is a cloud computing platform providing infrastructure, storage, databases, networking, and deployment services.

It is widely used for hosting Laravel applications.

---

## 187. What is EC2?

EC2 provides virtual servers in AWS.

Laravel applications are commonly deployed on EC2 instances.

---

## 188. What is S3?

Amazon S3 is an object storage service.

Laravel often uses S3 for storing images, documents, backups, and user uploads.

---

## 189. What is CloudFront?

CloudFront is AWS's Content Delivery Network (CDN).

It improves content delivery speed by serving assets from edge locations.

---

## 190. Why Use CDN?

A CDN reduces latency and server load.

Static assets are delivered from geographically closer locations to users.

---

# REAL PROJECT SCENARIOS

---

## 191. An API Suddenly Becomes Slow. What Would You Check?

I would investigate database queries, N+1 issues, server resource usage, Redis performance, cache misses, queue backlogs, and external API dependencies.

Performance monitoring tools help identify the root cause.

---

## 192. Users Report Duplicate Orders. How Would You Investigate?

I would review transaction handling, queue retries, API idempotency, database constraints, and application logs.

The goal is to identify where duplicate requests are being processed.

---

## 193. How Would You Handle High Traffic During a Sale Event?

I would implement caching, queue heavy tasks, optimize database queries, use load balancing, and scale infrastructure horizontally.

Proper preparation prevents application downtime.

---

## 194. How Do You Debug Production Issues?

I review application logs, server logs, monitoring dashboards, database performance metrics, and deployment history.

Changes introduced recently are often good starting points.

---

## 195. What Logging Tools Have You Used?

Common logging solutions include:

* Laravel Logs
* Monolog
* ELK Stack
* CloudWatch
* Sentry

Logs provide visibility into application behavior.

---

## 196. What is Idempotency?

Idempotency ensures that repeating the same request produces the same result.

Payment and order APIs often use idempotency keys to prevent duplicates.

---

## 197. What is a Race Condition?

Race conditions occur when multiple processes access shared resources simultaneously and produce inconsistent results.

Database locks and transactions help prevent race conditions.

---

## 198. What is Optimistic Locking?

Optimistic locking assumes conflicts are rare and validates data before updating.

Version numbers or timestamps are commonly used.

---

## 199. What is Pessimistic Locking?

Pessimistic locking prevents concurrent modifications by locking records during transactions.

### Example

```php id="nkt5w9"
DB::table('users')
    ->lockForUpdate()
    ->find(1);
```

This is useful for critical financial operations.

---

## 200. If Asked "Why Should We Hire You?", What Would You Answer?

I have hands-on experience building and maintaining Laravel APIs, database-driven applications, authentication systems, queue-based workflows, and performance optimizations. I focus on writing clean, maintainable code, solving production issues efficiently, and continuously improving my backend development skills.

I can contribute immediately while also growing with the team's technical challenges and long-term goals.

---

# FINAL 2+ YEARS LARAVEL BACKEND DEVELOPER REVISION CHECKLIST

### PHP

✅ OOP
✅ Traits
✅ Interfaces
✅ Abstract Classes
✅ Dependency Injection
✅ SOLID Principles
✅ Design Patterns

### Laravel

✅ Lifecycle
✅ Service Container
✅ Service Providers
✅ Middleware
✅ Routing
✅ Eloquent ORM
✅ Relationships
✅ Policies
✅ Events
✅ Queues
✅ Horizon
✅ Notifications

### Database

✅ Joins
✅ Transactions
✅ Indexing
✅ EXPLAIN
✅ Optimization
✅ Replication

### API

✅ REST APIs
✅ Authentication
✅ Sanctum
✅ Pagination
✅ Rate Limiting
✅ API Resources

### Performance

✅ Redis
✅ Caching
✅ Eager Loading
✅ Queue Processing

### DevOps

✅ Docker
✅ CI/CD
✅ AWS Basics
✅ Monitoring
✅ Logging

### Production

✅ Debugging
✅ Race Conditions
✅ Idempotency
✅ Scaling Strategies
✅ High-Traffic Handling

Mastering these 200 questions and being able to explain each answer with real project examples will cover the vast majority of Laravel/PHP backend interviews for developers with approximately 2–4 years of experience.


If you can confidently explain these topics with examples, you will be prepared for most Laravel Backend Developer interviews with 2+ years of experience.
