# 🌱 Дополнительные ответы на вопросы по Spring Framework

## 25. Связи таблиц many-to-many и one-to-one

### One-to-One (Один к одному)

```java
// Основная сущность
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String username;
    
    @OneToOne(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JoinColumn(name = "profile_id")
    private UserProfile profile;
    
    // конструкторы, геттеры, сеттеры
}

// Связанная сущность
@Entity
public class UserProfile {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String firstName;
    private String lastName;
    private String email;
    
    @OneToOne(mappedBy = "profile")
    private User user;
    
    // конструкторы, геттеры, сеттеры
}
```

### Many-to-Many (Многие ко многим)

```java
// Первая сущность
@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    
    @ManyToMany(cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses = new HashSet<>();
    
    // методы для управления связями
    public void addCourse(Course course) {
        courses.add(course);
        course.getStudents().add(this);
    }
    
    public void removeCourse(Course course) {
        courses.remove(course);
        course.getStudents().remove(this);
    }
}

// Вторая сущность
@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String title;
    
    @ManyToMany(mappedBy = "courses")
    private Set<Student> students = new HashSet<>();
    
    // конструкторы, геттеры, сеттеры
}
```

### Many-to-Many с дополнительными атрибутами

```java
// Промежуточная сущность
@Entity
public class StudentCourse {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "student_id")
    private Student student;
    
    @ManyToOne
    @JoinColumn(name = "course_id")
    private Course course;
    
    private LocalDateTime enrollmentDate;
    private Integer grade;
    
    // конструкторы, геттеры, сеттеры
}
```

---

## 26. Как работают каскады для таблиц и какие они бывают?

### Типы каскадов в JPA

#### CascadeType.PERSIST
```java
@Entity
public class Order {
    @OneToMany(cascade = CascadeType.PERSIST, mappedBy = "order")
    private List<OrderItem> items = new ArrayList<>();
    
    // При сохранении Order автоматически сохранятся все OrderItem
}
```

#### CascadeType.MERGE
```java
@Entity
public class User {
    @OneToOne(cascade = CascadeType.MERGE)
    private UserProfile profile;
    
    // При обновлении User автоматически обновится UserProfile
}
```

#### CascadeType.REMOVE
```java
@Entity
public class Department {
    @OneToMany(cascade = CascadeType.REMOVE, mappedBy = "department")
    private List<Employee> employees;
    
    // При удалении Department автоматически удалятся все Employee
}
```

#### CascadeType.REFRESH
```java
@Entity
public class Blog {
    @OneToMany(cascade = CascadeType.REFRESH, mappedBy = "blog")
    private List<Post> posts;
    
    // При обновлении Blog из БД автоматически обновятся все Post
}
```

#### CascadeType.DETACH
```java
@Entity
public class Customer {
    @OneToMany(cascade = CascadeType.DETACH, mappedBy = "customer")
    private List<Order> orders;
    
    // При отсоединении Customer от контекста отсоединятся все Order
}
```

#### CascadeType.ALL
```java
@Entity
public class Author {
    @OneToMany(cascade = CascadeType.ALL, mappedBy = "author")
    private List<Book> books;
    
    // Включает все типы каскадов
}
```

### Практические примеры

#### Безопасное использование каскадов
```java
@Entity
public class Order {
    @OneToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE}, 
               mappedBy = "order", 
               orphanRemoval = true)
    private List<OrderItem> items = new ArrayList<>();
    
    // orphanRemoval = true - удаляет "осиротевшие" записи
}
```

#### Осторожность с CascadeType.REMOVE
```java
@Entity
public class Category {
    // ПЛОХО - удаление категории удалит все продукты
    @OneToMany(cascade = CascadeType.REMOVE, mappedBy = "category")
    private List<Product> products;
    
    // ХОРОШО - только обновление и добавление
    @OneToMany(cascade = {CascadeType.PERSIST, CascadeType.MERGE}, 
               mappedBy = "category")
    private List<Product> products;
}
```

---

## 27. REST-сервисы: преимущества и недостатки

### Преимущества REST

#### 1. Простота и читаемость
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")           // GET /api/users/1
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping                   // POST /api/users
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
    
    @PutMapping("/{id}")          // PUT /api/users/1
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    @DeleteMapping("/{id}")       // DELETE /api/users/1
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

#### 2. Stateless (без состояния)
```java
@RestController
public class AuthController {
    @PostMapping("/login")
    public ResponseEntity<TokenResponse> login(@RequestBody LoginRequest request) {
        String token = authService.authenticate(request);
        return ResponseEntity.ok(new TokenResponse(token));
    }
    
    // Каждый запрос содержит всю необходимую информацию
    @GetMapping("/profile")
    public User getProfile(@RequestHeader("Authorization") String token) {
        User user = tokenService.getUserFromToken(token);
        return user;
    }
}
```

#### 3. Кэшируемость
```java
@RestController
public class ProductController {
    @GetMapping("/products/{id}")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        Product product = productService.findById(id);
        
        return ResponseEntity.ok()
            .cacheControl(CacheControl.maxAge(1, TimeUnit.HOURS))
            .eTag(String.valueOf(product.getVersion()))
            .body(product);
    }
}
```

#### 4. Масштабируемость
```java
@Configuration
@EnableAsync
public class AsyncConfig {
    @Bean
    public TaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(50);
        executor.setQueueCapacity(100);
        return executor;
    }
}

@RestController
public class ReportController {
    @PostMapping("/reports/generate")
    @Async
    public CompletableFuture<String> generateReport(@RequestBody ReportRequest request) {
        // Асинхронная обработка
        return CompletableFuture.completedFuture("Report generated");
    }
}
```

### Недостатки REST

#### 1. Over-fetching и Under-fetching
```java
// Over-fetching - получаем больше данных чем нужно
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.findById(id); // Возвращает ВСЕ поля пользователя
}

// Under-fetching - нужно делать дополнительные запросы
@GetMapping("/users/{id}/orders")
public List<Order> getUserOrders(@PathVariable Long id) {
    return orderService.findByUserId(id); // Отдельный запрос для заказов
}
```

#### 2. Множественные запросы
```javascript
// Клиент должен делать несколько запросов
fetch('/api/users/1')           // Получить пользователя
  .then(user => 
    fetch(`/api/users/${user.id}/orders`)  // Получить его заказы
  )
  .then(orders => 
    orders.forEach(order => 
      fetch(`/api/orders/${order.id}/items`) // Получить товары каждого заказа
    )
  );
```

#### 3. Отсутствие реального времени
```java
// REST не поддерживает push-уведомления
// Нужно использовать polling
@RestController
public class NotificationController {
    @GetMapping("/notifications")
    public List<Notification> getNotifications() {
        return notificationService.getUnread();
    }
    
    // Клиент должен периодически опрашивать этот endpoint
}
```

---

## 28. Форматы данных в REST-сервисах

### JSON (JavaScript Object Notation)

```java
@RestController
public class UserController {
    @GetMapping(value = "/users/{id}", produces = MediaType.APPLICATION_JSON_VALUE)
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping(value = "/users", 
                 consumes = MediaType.APPLICATION_JSON_VALUE,
                 produces = MediaType.APPLICATION_JSON_VALUE)
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
}
```

**Пример JSON:**
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "profile": {
    "firstName": "John",
    "lastName": "Doe",
    "age": 30
  },
  "roles": ["USER", "ADMIN"]
}
```

### XML (eXtensible Markup Language)

```java
@RestController
public class ProductController {
    @GetMapping(value = "/products/{id}", produces = MediaType.APPLICATION_XML_VALUE)
    public Product getProduct(@PathVariable Long id) {
        return productService.findById(id);
    }
}

// Конфигурация для поддержки XML
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer
            .favorParameter(true)
            .parameterName("format")
            .defaultContentType(MediaType.APPLICATION_JSON)
            .mediaType("json", MediaType.APPLICATION_JSON)
            .mediaType("xml", MediaType.APPLICATION_XML);
    }
}
```

**Пример XML:**
```xml
<product>
    <id>1</id>
    <name>Laptop</name>
    <price>999.99</price>
    <category>
        <id>1</id>
        <name>Electronics</name>
    </category>
</product>
```

### Другие форматы

#### YAML
```java
@RestController
public class ConfigController {
    @GetMapping(value = "/config", produces = "application/x-yaml")
    public Map<String, Object> getConfig() {
        Map<String, Object> config = new HashMap<>();
        config.put("database.url", "localhost:5432");
        config.put("cache.enabled", true);
        return config;
    }
}
```

#### CSV
```java
@RestController
public class ReportController {
    @GetMapping(value = "/users/export", produces = "text/csv")
    public ResponseEntity<String> exportUsers() {
        String csv = userService.exportToCsv();
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentDispositionFormData("attachment", "users.csv");
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(csv);
    }
}
```

---

## 29. Что такое @ResponseBody, @RequestBody, ResponseEntity?

### @RequestBody

```java
@RestController
public class UserController {
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // @RequestBody автоматически десериализует JSON в объект User
        return userService.create(user);
    }
    
    @PostMapping("/users/batch")
    public List<User> createUsers(@RequestBody List<User> users) {
        // Может работать с коллекциями
        return userService.createAll(users);
    }
    
    // Валидация с @RequestBody
    @PostMapping("/users/validated")
    public User createValidatedUser(@Valid @RequestBody User user) {
        return userService.create(user);
    }
}
```

### @ResponseBody

```java
@Controller // Обычный @Controller, не @RestController
public class DataController {
    @GetMapping("/users/{id}")
    @ResponseBody // Указываем, что возвращаем данные, а не представление
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @GetMapping("/users")
    @ResponseBody
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    // Без @ResponseBody вернется представление (view)
    @GetMapping("/users/page")
    public String getUsersPage(Model model) {
        model.addAttribute("users", userService.findAll());
        return "users"; // Вернется шаблон users.html
    }
}
```

### ResponseEntity

```java
@RestController
public class UserController {
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        try {
            User user = userService.findById(id);
            return ResponseEntity.ok(user);
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.create(user);
        
        return ResponseEntity
            .status(HttpStatus.CREATED)
            .header("Location", "/users/" + createdUser.getId())
            .body(createdUser);
    }
    
    @PutMapping("/users/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        if (!userService.exists(id)) {
            return ResponseEntity.notFound().build();
        }
        
        User updatedUser = userService.update(id, user);
        return ResponseEntity.ok(updatedUser);
    }
    
    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        if (!userService.exists(id)) {
            return ResponseEntity.notFound().build();
        }
        
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
    
    // Кастомные заголовки и статусы
    @GetMapping("/users/export")
    public ResponseEntity<byte[]> exportUsers() {
        byte[] data = userService.exportToCsv().getBytes();
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        headers.setContentDispositionFormData("attachment", "users.csv");
        headers.setCacheControl("no-cache, no-store, must-revalidate");
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(data);
    }
}
```

### Обработка ошибок с ResponseEntity

```java
@RestController
public class UserController {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException e) {
        ErrorResponse error = new ErrorResponse("USER_NOT_FOUND", e.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException e) {
        ErrorResponse error = new ErrorResponse("VALIDATION_ERROR", e.getMessage());
        return ResponseEntity.badRequest().body(error);
    }
}

public class ErrorResponse {
    private String code;
    private String message;
    private LocalDateTime timestamp = LocalDateTime.now();
    
    // конструкторы, геттеры, сеттеры
}
```

---

## 🎯 Продолжение следует...

Это первая часть дополнительных ответов. Остальные вопросы (30-34) будут в следующем файле для удобства навигации и загрузки.

**Следующие темы:**
- @RestController vs @Controller
- RestTemplate и его методы  
- AJAX/fetch
- HTTP протокол
- Bootstrap
