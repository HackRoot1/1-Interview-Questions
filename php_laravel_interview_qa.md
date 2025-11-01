# Comprehensive Interview Questions: Core PHP & Laravel
## For 2.5 Years Experience Developers

This guide contains curated interview questions specifically designed for developers with 2.5 years of experience in PHP and Laravel development.

---

## Table of Contents

1. [Core PHP & OOP Concepts](#core-php--oop-concepts)
2. [Laravel Framework Fundamentals](#laravel-framework-fundamentals)
3. [API Development & Authentication](#api-development--authentication)
4. [Database Design & Optimization](#database-design--optimization)
5. [Performance Optimization](#performance-optimization)
6. [Testing & Code Quality](#testing--code-quality)
7. [Design Patterns & Architecture](#design-patterns--architecture)
8. [Real-World Scenario Questions](#real-world-scenario-questions)
9. [Advanced Topics](#advanced-topics)

---

## Core PHP & OOP Concepts

### Basic OOP Questions

**1. What are the four main principles of Object-Oriented Programming (OOP)?**

*Expected Answer:*
- **Encapsulation**: Bundling data (properties) and methods together, with controlled access through public, private, and protected modifiers
- **Abstraction**: Hiding implementation details and exposing only necessary functionality through interfaces or abstract classes
- **Inheritance**: Allowing classes to inherit properties and methods from parent classes using the `extends` keyword
- **Polymorphism**: Ability of objects to take multiple forms; implemented through method overriding and interfaces

**2. Explain the difference between inheritance and composition in PHP. When would you use each?**

*Expected Answer:*

**Inheritance (IS-A relationship):**
```php
class Animal {
    public function eat() {}
}
class Dog extends Animal {}
```
Use when: You have a clear hierarchical relationship (Dog IS-A Animal)

**Composition (HAS-A relationship):**
```php
class Engine {}
class Car {
    private Engine $engine;
}
```
Use when: You want to avoid tight coupling and have more flexible designs
- Composition is generally preferred as it's more flexible and avoids the fragile base class problem

**3. What is method overloading and how can it be simulated in PHP?**

*Expected Answer:*

PHP doesn't support traditional method overloading like Java, but it can be simulated using magic methods:

```php
class Calculator {
    public function __call($name, $arguments) {
        if ($name === 'add') {
            if (count($arguments) === 2) {
                return $arguments[0] + $arguments[1];
            } elseif (count($arguments) === 3) {
                return $arguments[0] + $arguments[1] + $arguments[2];
            }
        }
    }
}
```

Or better, use type hints and default parameters:
```php
public function add($a, $b = 0, $c = 0) {
    return $a + $b + $c;
}
```

### Intermediate OOP Questions

**4. Explain the SOLID principles and how they apply to PHP development.**

*Expected Answer:*

- **S - Single Responsibility Principle**: A class should have only one reason to change. Each class should handle one specific task.
- **O - Open/Closed Principle**: Classes should be open for extension but closed for modification. Use inheritance or composition rather than modifying existing code.
- **L - Liskov Substitution Principle**: Derived classes should be usable in place of their base classes without breaking functionality.
- **I - Interface Segregation Principle**: Clients shouldn't depend on interfaces they don't use. Create specific interfaces instead of one general interface.
- **D - Dependency Inversion Principle**: Depend on abstractions (interfaces), not on concrete implementations.

**5. What is Dependency Injection (DI) and why is it important?**

*Expected Answer:*

Dependency Injection is a design pattern where objects receive their dependencies from external sources rather than creating them internally.

```php
// Without DI (tight coupling)
class UserService {
    private $db;
    public function __construct() {
        $this->db = new Database();
    }
}

// With DI (loose coupling)
class UserService {
    private $db;
    public function __construct(Database $db) {
        $this->db = $db;
    }
}
```

Benefits:
- Easier to test (can inject mock objects)
- Loose coupling
- More maintainable code
- Better flexibility

**6. Explain interfaces and abstract classes. When would you use each?**

*Expected Answer:*

**Interfaces**: Define contracts without implementation
- Can't have properties
- All methods are abstract
- Used for "can-do" relationships
- A class can implement multiple interfaces

**Abstract Classes**: Partial implementation
- Can have properties
- Can have some implemented methods
- Used for "is-a" relationships (inheritance)
- A class can extend only one abstract class

```php
// Interface - what can it do?
interface PaymentInterface {
    public function process($amount);
}

// Abstract Class - what is it?
abstract class PaymentMethod {
    protected $amount;
    abstract public function validate();
}
```

**7. What are PHP namespaces and why are they important?**

*Expected Answer:*

Namespaces organize code into logical groups and prevent naming conflicts.

```php
namespace App\Services;

class UserService {
    // code
}

// Usage
$service = new App\Services\UserService();
// or with use
use App\Services\UserService;
$service = new UserService();
```

Benefits:
- Avoid naming conflicts
- Organize large projects
- Improve code readability
- Support autoloading (PSR-4)

**8. Explain traits in PHP and provide a use case.**

*Expected Answer:*

Traits are reusable sets of methods that can be used in multiple classes to solve the single inheritance limitation.

```php
trait Timestampable {
    public function getCreatedAt() {
        return $this->created_at;
    }
}

trait SoftDeletes {
    public function forceDelete() {
        // delete permanently
    }
}

class User {
    use Timestampable, SoftDeletes;
}
```

Use cases:
- Logging functionality
- Timestamp behavior
- Soft deletes
- Helper methods shared across unrelated classes

### Advanced PHP Questions

**9. What are design patterns and how do you implement Singleton and Factory patterns?**

*Expected Answer:*

**Singleton Pattern**: Ensures only one instance of a class exists

```php
class Database {
    private static $instance;
    private function __construct() {}
    
    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
}
```

**Factory Pattern**: Creates objects without specifying exact classes

```php
class PaymentFactory {
    public static function create($type) {
        switch($type) {
            case 'stripe':
                return new StripePayment();
            case 'paypal':
                return new PayPalPayment();
            default:
                throw new Exception("Unknown payment type");
        }
    }
}
```

**10. Explain static properties and methods. When would you use them?**

*Expected Answer:*

Static members belong to the class, not instances:

```php
class Counter {
    private static $count = 0;
    
    public static function increment() {
        self::$count++;
    }
    
    public static function getCount() {
        return self::$count;
    }
}

Counter::increment();
echo Counter::getCount(); // 1
```

Use cases:
- Singleton instances
- Global counters/configurations
- Utility/helper methods
- Factory methods

**11. What is the difference between `==` and `===` in PHP? Provide examples.**

*Expected Answer:*

- `==`: Loose comparison (type juggling allowed)
- `===`: Strict comparison (type must match)

```php
0 == false;        // true (loose)
0 === false;       // false (strict)

"5" == 5;          // true (loose)
"5" === 5;         // false (strict)

null == false;     // true (loose)
null === false;    // false (strict)
```

Always use `===` in production code to avoid bugs.

**12. Explain variable scope and the difference between global, static, and variable variables in PHP.**

*Expected Answer:*

```php
// Global scope
$globalVar = "global";

function example() {
    global $globalVar;  // Access global variable
    
    static $counter = 0;  // Persists between calls
    $counter++;
    
    echo $counter;  // 1, 2, 3...
}

// Variable variables
$name = "username";
$username = "John";
echo $$name;  // John

// Superglobals (always available)
$_GET, $_POST, $_SERVER, $_SESSION, $_COOKIE
```

---

## Laravel Framework Fundamentals

### Basic Laravel Questions

**13. What is Laravel and what are its key features?**

*Expected Answer:*

Laravel is a PHP web framework for building web applications following the MVC architectural pattern.

Key features:
- **Eloquent ORM**: Simple and elegant database layer
- **Blade Templating**: Powerful template engine with directives
- **Artisan CLI**: Command-line tool for automation
- **Migrations**: Version control for databases
- **Authentication**: Built-in user authentication
- **Authorization**: Role-based access control
- **Queues**: Asynchronous job processing
- **Service Container**: Dependency injection container

**14. Explain Laravel's Service Container and Dependency Injection.**

*Expected Answer:*

The Service Container is a powerful tool for managing class dependencies and performing dependency injection.

```php
// Binding
app()->bind('PaymentGateway', function() {
    return new StripeGateway();
});

// Resolving
$gateway = app('PaymentGateway');
// or
$gateway = app()->make('PaymentGateway');
```

In controllers:
```php
public function process(PaymentGateway $gateway) {
    // $gateway is automatically injected
}
```

Benefits:
- Automatic dependency resolution
- Easy to test
- Flexible and maintainable
- Easy to swap implementations

**15. What are Laravel service providers and what is their purpose?**

*Expected Answer:*

Service Providers are the central place of all Laravel application bootstrapping.

Two main methods:
- **register()**: Bind services into the container (don't resolve other providers)
- **boot()**: Use the registered services (can resolve other providers)

```php
class PaymentServiceProvider extends ServiceProvider {
    public function register() {
        $this->app->bind(
            'PaymentInterface',
            'StripePayment'
        );
    }
    
    public function boot() {
        // Use registered services
    }
}
```

Lifecycle:
1. All providers' `register()` methods are called
2. All providers' `boot()` methods are called
3. Application is ready

**16. Explain Laravel's routing system and route groups.**

*Expected Answer:*

Routes define the endpoints and HTTP methods for your application.

```php
// Basic routes
Route::get('/users', [UserController::class, 'index']);
Route::post('/users', [UserController::class, 'store']);
Route::put('/users/{id}', [UserController::class, 'update']);
Route::delete('/users/{id}', [UserController::class, 'destroy']);

// Route groups
Route::middleware('auth')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'show']);
    Route::post('/profile', [ProfileController::class, 'update']);
});

// Named routes
Route::get('/users/{id}', [UserController::class, 'show'])->name('users.show');
// URL generation
route('users.show', ['id' => 1]);

// Resource routes (CRUD)
Route::resource('posts', PostController::class);
```

**17. What is Eloquent ORM and how does it differ from Query Builder?**

*Expected Answer:*

**Eloquent ORM**: Object-Relational Mapping that treats tables as model objects

```php
class User extends Model {}

$users = User::where('active', 1)->get();
$user = User::find(1);
$user->name = 'John';
$user->save();
```

**Query Builder**: Low-level database query builder

```php
$users = DB::table('users')
    ->where('active', 1)
    ->get();
```

Key differences:
- Eloquent is object-oriented, Query Builder is procedural
- Eloquent includes relationships, Query Builder doesn't directly
- Use Eloquent for complex relationships and models
- Use Query Builder for simple queries or raw SQL

**18. Explain Laravel migrations. How do you create, run, and rollback migrations?**

*Expected Answer:*

Migrations are version control for databases.

Create:
```bash
php artisan make:migration create_users_table
php artisan make:migration add_phone_to_users_table
```

Run:
```bash
php artisan migrate
```

Rollback:
```bash
php artisan migrate:rollback  # Rollback last batch
php artisan migrate:reset     # Rollback all
php artisan migrate:refresh   # Reset and re-run
```

Example migration:
```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamps();
});
```

### Intermediate Laravel Questions

**19. Explain Laravel relationships (One-to-One, One-to-Many, Many-to-Many).**

*Expected Answer:*

**One-to-One**:
```php
class User extends Model {
    public function profile() {
        return $this->hasOne(Profile::class);
    }
}

$user->profile; // Access related model
```

**One-to-Many**:
```php
class Author extends Model {
    public function posts() {
        return $this->hasMany(Post::class);
    }
}

$author->posts; // Collection of posts
```

**Many-to-Many**:
```php
class User extends Model {
    public function roles() {
        return $this->belongsToMany(Role::class);
    }
}

$user->roles; // Collection of roles
```

Best practices:
- Use eager loading to avoid N+1 queries: `User::with('profile')->get()`
- Define inverse relationships: `belongsTo()`, `hasMany()`, `belongsToMany()`

**20. What is middleware in Laravel? How do you create and use custom middleware?**

*Expected Answer:*

Middleware provides a mechanism for filtering HTTP requests.

Create:
```bash
php artisan make:middleware CheckUserRole
```

Implement:
```php
public function handle($request, Closure $next) {
    if ($request->user() && $request->user()->role !== 'admin') {
        abort(403, 'Unauthorized');
    }
    return $next($request);
}
```

Register:
```php
// app/Http/Kernel.php
protected $routeMiddleware = [
    'admin' => \App\Http\Middleware\CheckUserRole::class,
];
```

Use:
```php
Route::post('/admin', [AdminController::class, 'store'])->middleware('admin');
```

**21. Explain Eloquent accessors, mutators, and casting.**

*Expected Answer:*

**Mutators** (set): Transform data before saving

```php
protected function setNameAttribute($value) {
    $this->attributes['name'] = strtoupper($value);
}

$user->name = 'john'; // Stored as 'JOHN'
```

**Accessors** (get): Transform data when retrieving

```php
protected function getNameAttribute($value) {
    return ucfirst($value);
}

echo $user->name; // Returns capitalized
```

**Casting**: Automatically cast to specific types

```php
protected $casts = [
    'is_active' => 'boolean',
    'created_at' => 'datetime',
    'metadata' => 'json',
];
```

**22. What is Eloquent eager loading and how does it solve the N+1 problem?**

*Expected Answer:*

**Problem**:
```php
$authors = Author::all(); // 1 query
foreach ($authors as $author) {
    echo $author->posts; // N queries
} // Total: 1 + N queries
```

**Solution - Eager Loading**:
```php
$authors = Author::with('posts')->get(); // 2 queries total

// Multiple relationships
Author::with('posts', 'comments')->get();

// Nested loading
Author::with('posts.comments')->get();
```

Alternative syntaxes:
```php
$authors = Author::load('posts'); // Lazy loading
```

### Advanced Laravel Questions

**23. Explain Laravel's service container binding and resolution.**

*Expected Answer:*

Bindings define how to create instances:

```php
// Simple binding
$this->app->bind('PaymentGateway', function() {
    return new StripeGateway();
});

// Singleton (same instance always)
$this->app->singleton('Config', function() {
    return new Configuration();
});

// Instance binding
$this->app->instance('PaymentGateway', new StripeGateway());

// Contextual binding
$this->app->when(UserController::class)
    ->needs(PaymentGateway::class)
    ->give(StripeGateway::class);
```

Resolution methods:
```php
$instance = app('PaymentGateway');
$instance = resolve('PaymentGateway');
// Constructor injection
public function __construct(PaymentGateway $gateway) {}
```

**24. What are Laravel facades and how do they work?**

*Expected Answer:*

Facades provide a static-like interface to services in the container.

```php
// Using facade
Cache::put('key', 'value', 60);
Log::info('User login');

// Behind the scenes
// app('cache')->put('key', 'value', 60);
// app('log')->info('User login');
```

How they work:
```php
class CacheFacade extends Facade {
    protected static function getFacadeAccessor() {
        return 'cache'; // Service container key
    }
}
```

Common facades: `Cache`, `Log`, `Mail`, `Auth`, `DB`, `Route`, `Session`

**25. Explain Laravel Eloquent scopes (global and local).**

*Expected Answer:*

**Local Scopes**: Reuse query logic

```php
public function scopeActive($query) {
    return $query->where('is_active', 1);
}

// Usage
$activeUsers = User::active()->get();
$activeUsers = User::active()->where('role', 'admin')->get();

// With parameters
public function scopeByRole($query, $role) {
    return $query->where('role', $role);
}
$admins = User::byRole('admin')->get();
```

**Global Scopes**: Automatically apply conditions

```php
protected static function boot() {
    parent::boot();
    
    static::addGlobalScope('active', function ($query) {
        $query->where('is_active', 1);
    });
}

// Override global scope
User::withoutGlobalScopes()->get();
```

**26. What is Laravel's event system and how do you use it?**

*Expected Answer:*

Events allow decoupled communication between parts of your application.

Create event:
```bash
php artisan make:event UserRegistered
```

Dispatch event:
```php
event(new UserRegistered($user));
// or
UserRegistered::dispatch($user);
```

Listen to event:
```php
public function listen($events) {
    $events->listen(UserRegistered::class, SendWelcomeEmail::class);
}
```

Event listener:
```php
public function handle(UserRegistered $event) {
    Mail::to($event->user->email)->send(new WelcomeEmail());
}
```

---

## API Development & Authentication

**27. Explain JWT (JSON Web Tokens) authentication. How do you implement it in Laravel?**

*Expected Answer:*

JWT is a stateless authentication mechanism. The server signs a token that the client uses for subsequent requests.

Structure: `header.payload.signature`

Implementation:

Install package:
```bash
composer require tymon/jwt-auth
```

Configure:
```bash
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
php artisan jwt:secret
```

Create token in controller:
```php
public function login(Request $request) {
    $credentials = $request->validate([
        'email' => 'required|email',
        'password' => 'required',
    ]);
    
    if (!$token = auth()->attempt($credentials)) {
        return response()->json(['error' => 'Unauthorized'], 401);
    }
    
    return response()->json(['token' => $token]);
}
```

Use token:
```php
public function getUser() {
    return response()->json(auth()->user());
}
```

Middleware:
```php
Route::middleware('auth:api')->group(function () {
    Route::get('/user', [UserController::class, 'show']);
});
```

**28. What are the differences between OAuth 2.0 and JWT?**

*Expected Answer:*

| Aspect | OAuth 2.0 | JWT |
|--------|----------|-----|
| Purpose | Authorization framework | Token format |
| Token Type | Access token + refresh token | Single token |
| Storage | Server maintains state | Stateless |
| Revocation | Immediate | Not immediate (until expiry) |
| Use Case | Third-party integrations | API authentication |
| Complexity | More complex | Simpler |

OAuth 2.0 is better for: Third-party applications, delegated access
JWT is better for: API authentication, mobile apps, microservices

**29. Explain API versioning strategies in Laravel.**

*Expected Answer:*

**URL Versioning**:
```php
Route::prefix('api/v1')->group(function () {
    Route::get('/users', [UserControllerV1::class, 'index']);
});

Route::prefix('api/v2')->group(function () {
    Route::get('/users', [UserControllerV2::class, 'index']);
});
```

**Header Versioning**:
```php
Route::middleware('api.version')->group(function () {
    Route::get('/users', [UserController::class, 'index']);
});

// Middleware checks Accept-Version header
```

**Query Parameter**:
```php
// /api/users?version=2
```

Best practice: Use URL versioning for clear separation

**30. How do you handle API request validation and error responses?**

*Expected Answer:*

Validation:
```php
public function store(Request $request) {
    $validated = $request->validate([
        'name' => 'required|string|max:255',
        'email' => 'required|email|unique:users',
        'password' => 'required|min:8',
    ]);
    
    $user = User::create($validated);
    return response()->json($user, 201);
}
```

Error handling:
```php
// app/Exceptions/Handler.php
public function render($request, Throwable $exception) {
    if ($request->expectsJson()) {
        if ($exception instanceof ValidationException) {
            return response()->json([
                'message' => 'Validation failed',
                'errors' => $exception->errors(),
            ], 422);
        }
        
        return response()->json([
            'message' => $exception->getMessage(),
        ], 500);
    }
}
```

Custom response format:
```php
return response()->json([
    'success' => true,
    'data' => $user,
    'message' => 'User created successfully',
], 201);
```

**31. How do you implement role-based access control (RBAC) in Laravel?**

*Expected Answer:*

Using Laravel's authorization gates and policies:

Create policy:
```bash
php artisan make:policy PostPolicy --model=Post
```

Define authorization logic:
```php
public function edit(User $user, Post $post) {
    return $user->id === $post->user_id;
}

public function delete(User $user, Post $post) {
    return $user->role === 'admin' || $user->id === $post->user_id;
}
```

Use in controller:
```php
$this->authorize('edit', $post);
$post->update($request->validated());
```

Use in views:
```php
@can('edit', $post)
    <a href="{{ route('posts.edit', $post) }}">Edit</a>
@endcan
```

Alternative with gates and roles:
```php
// Register gates
Gate::define('publish-post', function (User $user) {
    return $user->role === 'editor';
});

// Use
if (Gate::allows('publish-post')) {
    // Allow
}
```

---

## Database Design & Optimization

**32. Explain database normalization and the different normal forms (1NF, 2NF, 3NF).**

*Expected Answer:*

**First Normal Form (1NF)**:
- Eliminate repeating groups
- Each column contains atomic (indivisible) values

Before:
```
StudentID | Name  | Courses
1         | John  | Math, Physics, Chemistry
```

After:
```
StudentID | Name  | Course
1         | John  | Math
1         | John  | Physics
1         | John  | Chemistry
```

**Second Normal Form (2NF)**:
- Must be in 1NF
- Remove partial dependencies
- Non-key attributes must depend on the entire primary key

**Third Normal Form (3NF)**:
- Must be in 2NF
- Remove transitive dependencies
- Non-key attributes should not depend on other non-key attributes

Example - proper 3NF design:
```
Users Table: user_id, name, email
Posts Table: post_id, user_id, title, content
```

**33. How do you design a relational database schema? Explain primary keys, foreign keys, and indexes.**

*Expected Answer:*

**Primary Keys**: Uniquely identify records
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) UNIQUE NOT NULL
);
```

**Foreign Keys**: Establish relationships between tables
```sql
CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**Indexes**: Speed up data retrieval
```sql
CREATE INDEX idx_email ON users(email);
CREATE UNIQUE INDEX idx_username ON users(username);
CREATE INDEX idx_user_created ON posts(user_id, created_at);
```

Design best practices:
- Use surrogate keys (auto-increment integers) for primary keys
- Use FOREIGN KEY constraints for referential integrity
- Index columns used in WHERE, JOIN, and ORDER BY clauses
- Normalize to reduce data redundancy

**34. What is the N+1 query problem and how do you solve it?**

*Expected Answer:*

**Problem**:
```php
$users = User::all(); // 1 query
foreach ($users as $user) {
    echo $user->profile->bio; // N queries
}
// Total: 1 + N queries
```

**Solutions**:

1. Eager loading:
```php
$users = User::with('profile')->get(); // 2 queries
```

2. Lazy eager loading:
```php
$users = User::all();
$users->load('profile'); // 1 additional query
```

3. Query builder join:
```php
$users = User::join('profiles', 'users.id', '=', 'profiles.user_id')
    ->select('users.*', 'profiles.bio')
    ->get();
```

4. Single query aggregation:
```php
$users = User::with('profile:user_id,bio')->get();
```

**35. How do you write efficient SQL queries? Explain query optimization techniques.**

*Expected Answer:*

Techniques:

1. **Select only needed columns**:
```php
// Bad
User::all();

// Good
User::select('id', 'name', 'email')->get();
```

2. **Use indexes**:
```sql
CREATE INDEX idx_user_status ON users(status);
```

3. **Avoid OR conditions** (use IN instead):
```sql
-- Bad
WHERE id = 1 OR id = 2 OR id = 3

-- Good
WHERE id IN (1, 2, 3)
```

4. **Use LIMIT**:
```php
User::limit(10)->get();
```

5. **Avoid functions on indexed columns**:
```sql
-- Bad
WHERE YEAR(created_at) = 2024

-- Good
WHERE created_at >= '2024-01-01' AND created_at < '2025-01-01'
```

6. **Use EXISTS instead of IN for large result sets**:
```sql
SELECT * FROM users WHERE EXISTS (SELECT 1 FROM posts WHERE posts.user_id = users.id)
```

7. **Denormalization for read-heavy operations**:
```php
// Store frequently accessed data in parent table
$user->post_count; // Instead of count($user->posts)
```

---

## Performance Optimization

**36. What caching strategies would you use in a Laravel application?**

*Expected Answer:*

**Types of caching**:

1. **Query Caching**:
```php
$users = Cache::remember('users', 3600, function () {
    return User::all();
});
```

2. **Route Caching**:
```bash
php artisan route:cache
php artisan config:cache
php artisan view:cache
```

3. **Fragment Caching** (view caching):
```php
@cache
    <expensive-component />
@endcache
```

4. **HTTP Caching**:
```php
Route::get('/posts', [PostController::class, 'index'])->middleware('cache:60');
```

5. **Model Caching**:
```php
class User extends Model {
    protected $cache_duration = 3600;
}
```

Cache drivers: `file`, `redis`, `memcached`, `database`

Cache invalidation:
```php
Cache::forget('users');
Cache::flush();
Cache::tags(['users'])->flush(); // Tag-based cache
```

**37. How do you optimize Laravel application performance?**

*Expected Answer:*

1. **Database optimization**:
   - Use eager loading
   - Create indexes
   - Optimize queries
   - Use pagination

2. **Caching**:
   - Cache queries
   - Cache routes
   - Cache views
   - Use Redis for session storage

3. **Code optimization**:
   - Remove unused code
   - Use lazy loading appropriately
   - Optimize loops
   - Use facades efficiently

4. **Server-side optimization**:
   - Enable OpCode caching (OPcache)
   - Use HTTP/2
   - Enable gzip compression
   - CDN for static assets

5. **Monitoring**:
   ```php
   php artisan tinker
   // Use Laravel Debugbar
   // Use Laravel Telescope for production monitoring
   ```

6. **Async processing**:
```php
// Queue heavy tasks
Queue::dispatch(new ProcessImport($file));
```

**38. Explain Laravel queues and job processing.**

*Expected Answer:*

Create job:
```bash
php artisan make:job SendEmail
```

Define job:
```php
public function handle() {
    Mail::to($this->user->email)->send(new WelcomeMail());
}
```

Dispatch job:
```php
SendEmail::dispatch($user); // Queued
SendEmail::dispatchSync($user); // Synchronous
```

Configure in `.env`:
```
QUEUE_CONNECTION=redis
```

Run worker:
```bash
php artisan queue:work
```

Job options:
```php
SendEmail::dispatch($user)
    ->delay(now()->addMinutes(10))
    ->onQueue('emails')
    ->onConnection('redis');
```

---

## Testing & Code Quality

**39. How do you write unit tests in Laravel using PHPUnit?**

*Expected Answer:*

Create test:
```bash
php artisan make:test UserServiceTest --unit
```

Write test:
```php
use PHPUnit\Framework\TestCase;

class UserServiceTest extends TestCase {
    public function test_user_creation() {
        $user = User::create([
            'name' => 'John',
            'email' => 'john@example.com',
        ]);
        
        $this->assertDatabaseHas('users', [
            'email' => 'john@example.com',
        ]);
    }
}
```

Common assertions:
```php
$this->assertTrue($condition);
$this->assertEquals($expected, $actual);
$this->assertNull($value);
$this->assertArrayHasKey('key', $array);
$this->assertInstanceOf(User::class, $object);
```

Run tests:
```bash
php artisan test
php artisan test --filter=test_user_creation
```

**40. What is the difference between unit tests, feature tests, and integration tests?**

*Expected Answer:*

**Unit Tests**: Test individual functions/methods in isolation
```php
public function test_password_validation() {
    $this->assertTrue(User::isValidPassword('password123'));
    $this->assertFalse(User::isValidPassword('weak'));
}
```

**Feature Tests**: Test features from user perspective (with database)
```php
public function test_user_can_register() {
    $response = $this->post('/register', [
        'name' => 'John',
        'email' => 'john@example.com',
        'password' => 'password',
    ]);
    
    $response->assertRedirect('/dashboard');
    $this->assertDatabaseHas('users', ['email' => 'john@example.com']);
}
```

**Integration Tests**: Test how multiple components work together

---

## Design Patterns & Architecture

**41. Explain the Repository pattern and when you would use it.**

*Expected Answer:*

Repository pattern abstracts data access logic:

```php
interface UserRepositoryInterface {
    public function getAll();
    public function getById($id);
    public function create(array $data);
    public function update($id, array $data);
}

class UserRepository implements UserRepositoryInterface {
    public function __construct(private User $model) {}
    
    public function getAll() {
        return $this->model->all();
    }
}
```

Use in service:
```php
class UserService {
    public function __construct(private UserRepositoryInterface $repository) {}
    
    public function getActiveUsers() {
        return $this->repository->getAll()->where('active', 1);
    }
}
```

Benefits:
- Decouples business logic from data access
- Easy to test (mock repository)
- Easy to change data source
- Promotes consistency

**42. What is the Service layer pattern? How do you implement it?**

*Expected Answer:*

Service layer contains business logic, controllers delegate to services:

```php
class UserService {
    public function __construct(
        private UserRepository $repository,
        private MailService $mail
    ) {}
    
    public function registerUser(array $data) {
        // Business logic
        $user = $this->repository->create($data);
        
        $this->mail->sendWelcomeEmail($user);
        
        return $user;
    }
}
```

Use in controller:
```php
class UserController {
    public function __construct(private UserService $service) {}
    
    public function store(Request $request) {
        $user = $this->service->registerUser($request->validated());
        return response()->json($user, 201);
    }
}
```

**43. Explain the Model-View-Controller (MVC) pattern.**

*Expected Answer:*

MVC separates concerns into three layers:

**Model**: Represents data and business logic
```php
class Post extends Model {
    public function user() {
        return $this->belongsTo(User::class);
    }
}
```

**View**: Presents data to user
```html
<h1>{{ $post->title }}</h1>
<p>{{ $post->content }}</p>
```

**Controller**: Handles requests and coordinates between model and view
```php
class PostController {
    public function show($id) {
        $post = Post::find($id);
        return view('posts.show', ['post' => $post]);
    }
}
```

Flow:
1. User interacts with view
2. Controller receives request
3. Model processes business logic
4. View displays result

---

## Real-World Scenario Questions

**44. You have a Laravel application experiencing slow database queries. Walk me through your debugging process.**

*Expected Answer:*

1. **Enable query logging**:
```php
DB::enableQueryLog();
// Run query
dd(DB::getQueryLog());
```

2. **Use Laravel Debugbar**:
```bash
composer require barryvdh/laravel-debugbar --dev
```

3. **Check for N+1 problems**:
```php
// Look for multiple similar queries
User::with('posts')->get(); // Fix
```

4. **Analyze query execution plan**:
```sql
EXPLAIN SELECT * FROM posts WHERE user_id = 1;
```

5. **Check indexes**:
```sql
SHOW INDEX FROM posts;
```

6. **Add missing indexes**:
```php
Schema::table('posts', function (Blueprint $table) {
    $table->index('user_id');
});
```

7. **Optimize query structure**:
   - Select only needed columns
   - Use joins efficiently
   - Add WHERE conditions early

8. **Use caching**:
```php
$posts = Cache::remember('user_posts_1', 3600, function () {
    return Post::where('user_id', 1)->get();
});
```

**45. How would you handle file uploads securely in Laravel?**

*Expected Answer:*

```php
public function upload(Request $request) {
    // Validation
    $request->validate([
        'file' => 'required|file|mimes:pdf,jpg|max:2048',
    ]);
    
    // Store file
    $path = $request->file('file')->store('uploads', 'private');
    
    // Record in database
    Document::create([
        'file_path' => $path,
        'original_name' => $request->file('file')->getClientOriginalName(),
        'user_id' => auth()->id(),
    ]);
    
    return response()->json(['message' => 'File uploaded']);
}
```

Security measures:
- Validate file type (mime types)
- Validate file size
- Store outside web root (`private` disk)
- Generate unique filename
- Check user permissions
- Use signed URLs for download

**46. Describe your approach to handling concurrent requests and race conditions.**

*Expected Answer:*

Race condition example: Two users updating inventory simultaneously

Solutions:

1. **Database locks**:
```php
DB::transaction(function () {
    $product = Product::lockForUpdate()->find($id);
    $product->quantity -= $quantity;
    $product->save();
});
```

2. **Pessimistic locking**:
```php
$product = Product::lockForUpdate()->find($id);
```

3. **Optimistic locking** (version column):
```php
$product = Product::find($id);
// Check version before updating
if ($product->version != $original_version) {
    throw new ConcurrencyException();
}
$product->increment('version');
$product->save();
```

4. **Queue jobs**:
```php
// Process one at a time
Queue::dispatch(new UpdateInventory($product, $quantity))
    ->onQueue('inventory');
```

**47. How would you migrate legacy code to Laravel? What's your approach?**

*Expected Answer:*

1. **Assessment phase**:
   - Understand current system
   - Identify critical functionality
   - Plan migration strategy

2. **Create new Laravel structure**:
   - Set up models
   - Create migrations
   - Define routes

3. **Data migration**:
```php
// Create seeders
php artisan make:seeder UserSeeder

// Migrate data from old system
// Old system → temporary → Laravel
```

4. **Gradual migration**:
   - Migrate one module at a time
   - Use adapter pattern for old code integration
   - Run both systems in parallel

5. **Testing**:
   - Write tests for critical functions
   - Verify data integrity
   - Performance testing

6. **Deployment**:
   - Blue-green deployment
   - Gradual traffic shifting
   - Rollback plan

---

## Advanced Topics

**48. Explain Laravel service container auto-binding and reflection.**

*Expected Answer:*

Laravel automatically resolves type-hinted dependencies using reflection:

```php
public function __construct(UserRepository $repository, MailService $mail) {
    // Laravel automatically creates instances
}
```

How it works:
```php
// Reflect on constructor
$reflection = new ReflectionClass(UserService::class);
$dependencies = $reflection->getConstructor()->getParameters();

// Resolve each dependency
foreach ($dependencies as $param) {
    $type = $param->getType();
    $instance = app()->make($type);
}
```

For complex types, use binding:
```php
$this->app->bind(PaymentInterface::class, StripePayment::class);
```

**49. How do you implement soft deletes in Laravel?**

*Expected Answer:*

Soft deletes mark records as deleted without removing them:

Add migration:
```php
Schema::table('posts', function (Blueprint $table) {
    $table->softDeletes();
});
```

Use trait:
```php
class Post extends Model {
    use SoftDeletes;
}
```

Query usage:
```php
// Include soft-deleted records
Post::withTrashed()->get();

// Only soft-deleted records
Post::onlyTrashed()->get();

// Exclude soft-deleted
Post::all(); // Default
```

Restore:
```php
$post->restore();
$post->forceDelete(); // Permanently delete
```

**50. What are Laravel events and listeners? How do they promote loose coupling?**

*Expected Answer:*

Events and listeners decouple code through observer pattern:

Define event:
```php
class UserRegistered {
    public function __construct(public User $user) {}
}
```

Define listener:
```php
class SendWelcomeEmail {
    public function handle(UserRegistered $event) {
        Mail::to($event->user->email)->send(new Welcome());
    }
}
```

Dispatch event:
```php
event(new UserRegistered($user));
```

Benefits:
- No direct coupling between components
- Easy to add new listeners
- Easy to test listeners independently
- Single responsibility principle

---

## Additional Practical Scenario Questions

**51. You have a multi-tenant application. How would you implement tenant isolation?**

*Expected Answer:*

Methods:

1. **Database per tenant**:
```php
// Switch database based on tenant
config(['database.connections.mysql.database' => $tenant->database_name]);
```

2. **Schema per tenant**:
```php
// Use schema prefix
DB::connection()->setTablePrefix($tenant->prefix . '_');
```

3. **Row-level isolation**:
```php
$query->where('tenant_id', auth()->user()->tenant_id);

// Use global scope
protected static function boot() {
    parent::boot();
    static::addGlobalScope(function ($query) {
        $query->where('tenant_id', auth()->user()->tenant_id);
    });
}
```

Best practice: Row-level isolation for flexibility

**52. How do you handle circular dependencies in your application?**

*Expected Answer:*

Circular dependency example:
```
ClassA depends on ClassB
ClassB depends on ClassA
```

Solutions:

1. **Dependency inversion**:
```php
// Use interfaces, not concrete classes
ClassA depends on Interface
ClassB depends on Interface
```

2. **Lazy loading**:
```php
public function getService() {
    return app('ServiceB'); // Resolve at runtime
}
```

3. **Restructure code**:
```php
// Extract common functionality into third class
ClassA depends on ClassC
ClassB depends on ClassC
```

4. **Event-driven**:
```php
// Instead of direct dependency
ClassA fires event
ClassB listens to event
```

**53. Explain polymorphism in Laravel and provide a real-world example.**

*Expected Answer:*

Polymorphic relationships: A model can belong to multiple types of parent models.

Example - User can follow Posts, Users, or Tags:

```php
class Follow extends Model {
    public function followable() {
        return $this->morphTo();
    }
}

class Post extends Model {
    public function followers() {
        return $this->morphMany(Follow::class, 'followable');
    }
}

class User extends Model {
    public function followers() {
        return $this->morphMany(Follow::class, 'followable');
    }
}
```

Usage:
```php
// A user follows a post
$post->followers()->create(['user_id' => $user->id]);

// A user follows another user
$anotherUser->followers()->create(['user_id' => $user->id]);

// Get what a user follows
$user->follows()->polymorphicWhereType(Post::class)->get();
```

---

## Technical Preparation Tips

**For the interview:**

1. **Practice writing code**: Be prepared to write working PHP/Laravel code
2. **Understand trade-offs**: Discuss pros and cons of different approaches
3. **Relate to experience**: Connect answers to your 2.5 years of experience
4. **Ask clarifying questions**: If a question is vague, ask for clarification
5. **Think aloud**: Explain your thought process
6. **Use examples**: Always provide practical examples
7. **Discuss testing**: Show you write testable code
8. **Performance mindset**: Always consider performance implications
9. **Security awareness**: Be conscious of security best practices
10. **Stay current**: Follow Laravel release notes and community

---

## Additional Resources for Preparation

- Laravel Documentation: https://laravel.com/docs
- PHP PSR Standards: https://www.php-fig.org/
- Design Patterns: https://refactoring.guru/design-patterns
- SQL Optimization: Study database query plans
- Real projects: Review your own code from 2.5 years of experience

---

## Mock Interview Questions to Practice

1. Explain a complex feature you built in Laravel
2. Describe the most challenging bug you fixed
3. How do you approach performance issues in production?
4. Tell me about your testing strategy
5. How do you handle deployment and migrations?
6. What's your deployment process for database schema changes?
7. How do you secure an API?
8. Describe your code review process
9. How do you handle technical debt?
10. What's your approach to error handling and logging?

---

## Summary

As a 2.5-year experienced developer:
- You should have solid understanding of both theory and practice
- Be able to discuss trade-offs and architectural decisions
- Provide real-world examples from your projects
- Understand performance and security considerations
- Demonstrate ability to mentor junior developers
- Show continuous learning and staying updated with framework changes

Good luck with your interviews!
