# 🌱 Spring Framework - Полный список вопросов для собеседований

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Spring](https://img.shields.io/badge/Spring-6.x-green.svg)](https://spring.io/)
[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.oracle.com/java/)
[![Spring Security](https://img.shields.io/badge/Spring%20Security-6.x-red.svg)](https://spring.io/projects/spring-security)
[![REST](https://img.shields.io/badge/REST-API-blue.svg)](https://restfulapi.net/)

Полный интерактивный список из **34 контрольных вопросов** по Spring Framework с подробными ответами, примерами кода и практическими рекомендациями для подготовки к собеседованиям.

## 📋 Содержание

### Основы Spring
1. [Что такое бин?](#1-что-такое-бин)
2. [Виды бинов?](#2-виды-бинов)
3. [Чем бин отличается от POJO-класса?](#3-чем-бин-отличается-от-pojo-класса)
4. [Что такое Inversion of Control и как Spring реализует этот принцип?](#4-что-такое-inversion-of-control-и-как-spring-реализует-этот-принцип)
5. [Для чего существует такое количество ApplicationContext?](#5-для-чего-существует-такое-количество-applicationcontext)

### Управление зависимостями
6. [Как можно связать бины?](#6-как-можно-связать-бины)
7. [Что такое Dependency Injection?](#7-что-такое-dependency-injection)
8. [Какие бины будут использоваться для настройки приложения?](#8-какие-бины-будут-использоваться-для-настройки-приложения)
9. [Как получить данные из файла .property?](#9-как-получить-данные-из-файла-property)

### Развертывание и артефакты
10. [Как запустить Spring-приложение из-под сервера Tomcat?](#10-как-запустить-spring-приложение-из-под-сервера-tomcat)
11. [Что такое Artifacts?](#11-что-такое-artifacts)
12. [В чем отличие артефакта war от war exploded?](#12-в-чем-отличие-артефакта-war-от-war-exploded)

### Архитектура и аннотации
13. [Какая разница между аннотациями @Component, @Repository и @Service в Spring?](#13-какая-разница-между-аннотациями-component-repository-и-service-в-spring)
14. [Как выглядит структура MVC-приложения?](#14-как-выглядит-структура-mvc-приложения)
15. [Чем контроллер отличается от сервлета?](#15-чем-контроллер-отличается-от-сервлета)
16. [Какая основная зависимость фреймворка Spring? Почему во многих сборках она не указывается явно?](#16-какая-основная-зависимость-фреймворка-spring-почему-во-многих-сборках-она-не-указывается-явно)
17. [Как вернуть страницу в контроллере? Как вернуть данные?](#17-как-вернуть-страницу-в-контроллере-как-вернуть-данные)

### Принципы и паттерны
18. [Уметь рассказать про принципы работы Spring](#18-уметь-рассказать-про-принципы-работы-spring)
19. [Связывание бинов и их жизненный цикл](#19-связывание-бинов-и-их-жизненный-цикл)
20. [Основные паттерны Spring](#20-основные-паттерны-spring)

### Spring Security
21. [Что такое авторизация и аутентификация?](#21-что-такое-авторизация-и-аутентификация)
22. [Объекты Principal, Authorities, Authentication](#22-объекты-principal-authorities-authentication)
23. [Чем отличается InMemoryAuthentication от BasicAuthentication?](#23-чем-отличается-inmemoryauthentication-от-basicauthentication)
24. [Как добавить секьюрность к контроллеру?](#24-как-добавить-секьюрность-к-контроллеру)

### Базы данных и JPA
25. [Связи таблиц many-to-many и one-to-one](#25-связи-таблиц-many-to-many-и-one-to-one)
26. [Как работают каскады для таблиц и какие они бывают?](#26-как-работают-каскады-для-таблиц-и-какие-они-бывают)

### REST-сервисы
27. [REST-сервисы: преимущества и недостатки](#27-rest-сервисы-преимущества-и-недостатки)
28. [Форматы данных в REST-сервисах](#28-форматы-данных-в-rest-сервисах)
29. [Что такое @ResponseBody, @RequestBody, ResponseEntity?](#29-что-такое-responsebody-requestbody-responseentity)
30. [Чем @RestController отличается от @Controller?](#30-чем-restcontroller-отличается-от-controller)
31. [RestTemplate и его методы](#31-resttemplate-и-его-методы)

### Веб-технологии
32. [Что такое AJAX/fetch?](#32-что-такое-ajaxfetch)
33. [HTTP протокол](#33-http-протокол)
34. [Bootstrap](#34-bootstrap)

---

## 📖 Ответы

### 1. Что такое бин?

**Бин (Bean)** - это объект, который полностью управляется Spring IoC контейнером. Это не просто Java-объект, а объект с особым статусом:

**Ключевые характеристики:**
- Spring знает о его существовании
- Spring управляет его жизненным циклом
- Spring может внедрять его в другие компоненты
- Spring может применять к нему аспекты (AOP)
- Spring может конфигурировать его свойства

**Пример создания бина:**
```java
@Component
public class UserService {
    // Spring автоматически создаст бин этого класса
}

// Или через конфигурацию
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 2. Виды бинов

#### Singleton (по умолчанию)
- **Один экземпляр** на весь контейнер
- **Потокобезопасность** - Spring гарантирует, что один экземпляр
- **Экономия памяти** - не создается новый объект при каждом запросе
- **Кэширование** - подходит для сервисов без состояния

#### Prototype
- **Новый экземпляр** при каждом запросе
- **Не управляется** Spring после создания
- **Больше памяти** - создается новый объект
- **Подходит для** объектов с состоянием

#### Request (веб-приложения)
- **Один экземпляр** на HTTP-запрос
- **Автоматически уничтожается** после завершения запроса
- **Потокобезопасность** в рамках одного запроса

#### Session (веб-приложения)
- **Один экземпляр** на HTTP-сессию
- **Живет** пока активна сессия
- **Подходит для** пользовательских данных

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 21. Что такое авторизация и аутентификация?

#### Аутентификация (Authentication)
**Аутентификация** - это процесс проверки подлинности пользователя (кто это?).

```java
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public AuthenticationManager authManager(HttpSecurity http) throws Exception {
        AuthenticationManagerBuilder authenticationManagerBuilder = 
            http.getSharedObject(AuthenticationManagerBuilder.class);
        
        authenticationManagerBuilder
            .userDetailsService(userDetailsService)
            .passwordEncoder(passwordEncoder());
        
        return authenticationManagerBuilder.build();
    }
}
```

#### Авторизация (Authorization)
**Авторизация** - это процесс проверки прав доступа (что может делать?).

```java
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasRole("USER")
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .permitAll()
            );
        return http.build();
    }
}
```

**Ключевые различия:**
- **Аутентификация** - проверка логина/пароля
- **Авторизация** - проверка прав доступа к ресурсам

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 25. Связи таблиц many-to-many и one-to-one

#### One-to-One (Один к одному)

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

#### Many-to-Many (Многие ко многим)

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

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 27. REST-сервисы: преимущества и недостатки

#### Преимущества REST

**1. Простота и читаемость**
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

**2. Stateless (без состояния)**
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

#### Недостатки REST

**1. Over-fetching и Under-fetching**
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

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 29. Что такое @ResponseBody, @RequestBody, ResponseEntity?

#### @RequestBody

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

#### @ResponseBody

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

#### ResponseEntity

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
    
    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        if (!userService.exists(id)) {
            return ResponseEntity.notFound().build();
        }
        
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 30. Чем @RestController отличается от @Controller?

#### @Controller (обычный контроллер)

```java
@Controller
@RequestMapping("/users")
public class UserController {
    @GetMapping
    public String getAllUsers(Model model) {
        List<User> users = userService.findAll();
        model.addAttribute("users", users);
        return "users/list"; // Возвращает имя view (шаблона)
    }
    
    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userService.findById(id);
        model.addAttribute("user", user);
        return "users/detail"; // Возвращает имя view
    }
    
    // Для возврата данных нужен @ResponseBody
    @GetMapping("/api/{id}")
    @ResponseBody
    public User getUserAsJson(@PathVariable Long id) {
        return userService.findById(id);
    }
}
```

#### @RestController (REST контроллер)

```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll(); // Автоматически возвращает JSON
    }
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id); // Автоматически возвращает JSON
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.create(user); // Автоматически возвращает JSON
    }
    
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user); // Автоматически возвращает JSON
    }
    
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id); // Возвращает HTTP 200 OK
    }
}
```

#### Ключевые различия:

| Аспект | @Controller | @RestController |
|--------|-------------|-----------------|
| **Возврат** | View (HTML страницы) | Данные (JSON/XML) |
| **@ResponseBody** | Нужен явно | Включен автоматически |
| **Использование** | MVC приложения | REST API |
| **Content-Type** | text/html | application/json |
| **Аннотация** | @Controller | @Controller + @ResponseBody |

#### Когда использовать:

**@Controller для:**
- Веб-приложений с HTML страницами
- MVC архитектуры
- Возврата шаблонов (Thymeleaf, JSP)

**@RestController для:**
- REST API
- Микросервисов
- Мобильных приложений
- SPA (Single Page Applications)

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 32. Что такое AJAX/fetch?

#### AJAX (Asynchronous JavaScript and XML)

**AJAX** - технология асинхронного обмена данными между браузером и сервером без перезагрузки страницы.

```javascript
// Классический AJAX с XMLHttpRequest
function loadUserData(userId) {
    const xhr = new XMLHttpRequest();
    
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
            const user = JSON.parse(xhr.responseText);
            document.getElementById('userName').textContent = user.name;
        }
    };
    
    xhr.open('GET', '/api/users/' + userId, true);
    xhr.send();
}

// AJAX с jQuery
$.ajax({
    url: '/api/users/' + userId,
    method: 'GET',
    success: function(user) {
        $('#userName').text(user.name);
    },
    error: function(xhr, status, error) {
        console.error('Error:', error);
    }
});
```

#### Fetch API (современный подход)

```javascript
// Современный fetch API
async function loadUserData(userId) {
    try {
        const response = await fetch('/api/users/' + userId);
        
        if (!response.ok) {
            throw new Error('Network response was not ok');
        }
        
        const user = await response.json();
        document.getElementById('userName').textContent = user.name;
    } catch (error) {
        console.error('Error:', error);
    }
}

// POST запрос с fetch
async function createUser(userData) {
    try {
        const response = await fetch('/api/users', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(userData)
        });
        
        if (!response.ok) {
            throw new Error('Failed to create user');
        }
        
        const newUser = await response.json();
        console.log('User created:', newUser);
    } catch (error) {
        console.error('Error:', error);
    }
}
```

#### Spring контроллер для AJAX

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        try {
            User user = userService.findById(id);
            return ResponseEntity.ok(user);
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.create(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        try {
            User updatedUser = userService.update(id, user);
            return ResponseEntity.ok(updatedUser);
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        try {
            userService.delete(id);
            return ResponseEntity.noContent().build();
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build();
        }
    }
}
```

#### Преимущества AJAX/fetch:

- **Без перезагрузки страницы** - улучшенный UX
- **Асинхронность** - не блокирует интерфейс
- **Частичное обновление** - обновляются только нужные части
- **Интерактивность** - динамические приложения

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 33. HTTP протокол

#### Основы HTTP

**HTTP (HyperText Transfer Protocol)** - протокол прикладного уровня для передачи данных в веб.

#### HTTP методы (глаголы)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    // GET - получение данных
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    // POST - создание новых ресурсов
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
    
    // PUT - полное обновление ресурса
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    // PATCH - частичное обновление ресурса
    @PatchMapping("/{id}")
    public User partialUpdateUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
        return userService.partialUpdate(id, updates);
    }
    
    // DELETE - удаление ресурса
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

#### HTTP статус коды

```java
@RestController
public class UserController {
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        try {
            User user = userService.findById(id);
            return ResponseEntity.ok(user); // 200 OK
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build(); // 404 Not Found
        }
    }
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        try {
            User createdUser = userService.create(user);
            return ResponseEntity.status(HttpStatus.CREATED).body(createdUser); // 201 Created
        } catch (ValidationException e) {
            return ResponseEntity.badRequest().build(); // 400 Bad Request
        }
    }
    
    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        try {
            userService.delete(id);
            return ResponseEntity.noContent().build(); // 204 No Content
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build(); // 404 Not Found
        }
    }
}
```

#### HTTP заголовки

```java
@RestController
public class UserController {
    @GetMapping("/users/export")
    public ResponseEntity<byte[]> exportUsers() {
        byte[] csvData = userService.exportToCsv();
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        headers.setContentDispositionFormData("attachment", "users.csv");
        headers.setCacheControl("no-cache, no-store, must-revalidate");
        headers.setExpires(0);
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(csvData);
    }
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id, 
                                      @RequestHeader("Accept") String accept) {
        User user = userService.findById(id);
        
        HttpHeaders headers = new HttpHeaders();
        headers.setETag(String.valueOf(user.getVersion()));
        headers.setLastModified(user.getLastModified());
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(user);
    }
}
```

#### HTTP vs HTTPS

**HTTP:**
- Порт 80
- Не зашифрован
- Быстрее
- Подходит для статического контента

**HTTPS:**
- Порт 443
- Зашифрован (SSL/TLS)
- Безопаснее
- Обязателен для персональных данных

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 34. Bootstrap

#### Что такое Bootstrap

**Bootstrap** - популярный CSS фреймворк для создания адаптивных веб-интерфейсов.

#### Интеграция с Spring Boot

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    
    <!-- Bootstrap Icons -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css" rel="stylesheet">
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container">
            <a class="navbar-brand" href="/">User Management</a>
            <div class="navbar-nav ms-auto">
                <a class="nav-link" href="/users">Users</a>
                <a class="nav-link" href="/admin">Admin</a>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <div class="container mt-4">
        <div class="row">
            <div class="col-md-12">
                <h1 class="mb-4">User List</h1>
                
                <!-- User Table -->
                <div class="card">
                    <div class="card-header">
                        <h5 class="card-title mb-0">All Users</h5>
                    </div>
                    <div class="card-body">
                        <table class="table table-striped table-hover">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Email</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr th:each="user : ${users}">
                                    <td th:text="${user.id}">1</td>
                                    <td th:text="${user.name}">John Doe</td>
                                    <td th:text="${user.email}">john@example.com</td>
                                    <td>
                                        <button class="btn btn-sm btn-primary me-2">
                                            <i class="bi bi-pencil"></i> Edit
                                        </button>
                                        <button class="btn btn-sm btn-danger">
                                            <i class="bi bi-trash"></i> Delete
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
                
                <!-- Add User Form -->
                <div class="card mt-4">
                    <div class="card-header">
                        <h5 class="card-title mb-0">Add New User</h5>
                    </div>
                    <div class="card-body">
                        <form th:action="@{/users}" method="post">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="mb-3">
                                        <label for="name" class="form-label">Name</label>
                                        <input type="text" class="form-control" id="name" name="name" required>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="mb-3">
                                        <label for="email" class="form-label">Email</label>
                                        <input type="email" class="form-control" id="email" name="email" required>
                                    </div>
                                </div>
                            </div>
                            <button type="submit" class="btn btn-success">
                                <i class="bi bi-plus"></i> Add User
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

#### Spring контроллер для Bootstrap страниц

```java
@Controller
@RequestMapping("/users")
public class UserController {
    
    @GetMapping
    public String getAllUsers(Model model) {
        List<User> users = userService.findAll();
        model.addAttribute("users", users);
        return "users/list"; // Возвращает Bootstrap шаблон
    }
    
    @PostMapping
    public String createUser(@ModelAttribute User user, RedirectAttributes redirectAttributes) {
        try {
            userService.create(user);
            redirectAttributes.addFlashAttribute("successMessage", "User created successfully!");
        } catch (Exception e) {
            redirectAttributes.addFlashAttribute("errorMessage", "Error creating user: " + e.getMessage());
        }
        return "redirect:/users";
    }
    
    @GetMapping("/{id}/edit")
    public String editUserForm(@PathVariable Long id, Model model) {
        User user = userService.findById(id);
        model.addAttribute("user", user);
        return "users/edit";
    }
    
    @PostMapping("/{id}")
    public String updateUser(@PathVariable Long id, @ModelAttribute User user, RedirectAttributes redirectAttributes) {
        try {
            userService.update(id, user);
            redirectAttributes.addFlashAttribute("successMessage", "User updated successfully!");
        } catch (Exception e) {
            redirectAttributes.addFlashAttribute("errorMessage", "Error updating user: " + e.getMessage());
        }
        return "redirect:/users";
    }
    
    @PostMapping("/{id}/delete")
    public String deleteUser(@PathVariable Long id, RedirectAttributes redirectAttributes) {
        try {
            userService.delete(id);
            redirectAttributes.addFlashAttribute("successMessage", "User deleted successfully!");
        } catch (Exception e) {
            redirectAttributes.addFlashAttribute("errorMessage", "Error deleting user: " + e.getMessage());
        }
        return "redirect:/users";
    }
}
```

#### AJAX с Bootstrap

```javascript
// AJAX удаление с Bootstrap модальным окном
function deleteUser(userId, userName) {
    const modal = new bootstrap.Modal(document.getElementById('deleteModal'));
    document.getElementById('userToDelete').textContent = userName;
    
    document.getElementById('confirmDelete').onclick = function() {
        fetch('/users/' + userId, {
            method: 'DELETE',
            headers: {
                'X-CSRF-TOKEN': document.querySelector('meta[name="_csrf"]').getAttribute('content')
            }
        })
        .then(response => {
            if (response.ok) {
                location.reload(); // Перезагружаем страницу
            } else {
                alert('Error deleting user');
            }
        })
        .catch(error => {
            console.error('Error:', error);
            alert('Error deleting user');
        });
        
        modal.hide();
    };
    
    modal.show();
}
```

#### Преимущества Bootstrap:

- **Адаптивность** - автоматическая адаптация под разные экраны
- **Готовые компоненты** - кнопки, формы, таблицы, модальные окна
- **Консистентность** - единый стиль по всему приложению
- **Быстрая разработка** - меньше CSS кода
- **Кроссбраузерность** - работает во всех современных браузерах

[⬆️ Вернуться к списку вопросов](#-содержание)

---

## 🎯 Заключение

Этот полный список содержит 34 вопроса по Spring Framework, включая Spring Security, REST-сервисы, работу с базами данных и веб-технологии. Каждый вопрос содержит подробный ответ с примерами кода и практическими рекомендациями.

**Удачи на собеседовании! 🚀**
