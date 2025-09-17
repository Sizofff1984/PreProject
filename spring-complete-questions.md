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
18. [Принципы работы Spring](#18-принципы-работы-spring)
19. [Связывание бинов и их жизненный цикл](#19-связывание-бинов-и-жизненный-цикл)
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

### 3. Чем бин отличается от POJO-класса?

#### POJO (Plain Old Java Object)
```java
public class User {
    private String name;
    private int age;
    
    // геттеры и сеттеры
}
```

#### Бин в Spring
```java
@Component
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    @PostConstruct
    public void init() {
        // инициализация
    }
    
    @PreDestroy
    public void cleanup() {
        // очистка ресурсов
    }
}
```

**Дополнительные возможности бина:**
- **Аннотации** - @Component, @Service, @Repository
- **Жизненный цикл** - @PostConstruct, @PreDestroy
- **Внедрение зависимостей** - @Autowired, @Resource
- **AOP** - @Transactional, @Cacheable
- **Конфигурация** - @Value, @ConfigurationProperties

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 4. Что такое Inversion of Control и как Spring реализует этот принцип?

#### Традиционный подход (плохо):
```java
public class OrderService {
    private PaymentService paymentService;
    private EmailService emailService;
    
    public OrderService() {
        this.paymentService = new PaymentService(); // жесткая связь
        this.emailService = new EmailService();
    }
}
```

#### IoC подход (хорошо):
```java
@Service
public class OrderService {
    private final PaymentService paymentService;
    private final EmailService emailService;
    
    public OrderService(PaymentService paymentService, EmailService emailService) {
        this.paymentService = paymentService; // Spring внедряет
        this.emailService = emailService;
    }
}
```

**Преимущества IoC:**
- **Слабая связанность** - классы не знают о конкретных реализациях
- **Тестируемость** - легко подставить моки
- **Гибкость** - можно менять реализации без изменения кода
- **Переиспользование** - один бин может использоваться в разных местах

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 5. Для чего существует такое количество ApplicationContext?

#### Типы контекстов:

**ClassPathXmlApplicationContext**
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

**AnnotationConfigApplicationContext**
```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```

**WebApplicationContext** (для веб-приложений)
```xml
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring-config.xml</param-value>
    </init-param>
</servlet>
```

**GenericApplicationContext** (программная конфигурация)
```java
GenericApplicationContext context = new GenericApplicationContext();
context.registerBean(UserService.class);
context.refresh();
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 6. Как можно связать бины?

#### Constructor Injection (рекомендуемый)
```java
@Service
public class UserService {
    private final UserRepository userRepository;
    private final EmailService emailService;
    
    public UserService(UserRepository userRepository, EmailService emailService) {
        this.userRepository = userRepository;
        this.emailService = emailService;
    }
}
```

#### Setter Injection
```java
@Service
public class UserService {
    private UserRepository userRepository;
    private EmailService emailService;
    
    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

#### Field Injection (не рекомендуется)
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private EmailService emailService;
}
```

#### Method Injection
```java
@Service
public class UserService {
    private UserRepository userRepository;
    
    @Autowired
    public void configure(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 7. Что такое Dependency Injection?

**DI** - это реализация принципа IoC, где зависимости внедряются извне.

#### Типы внедрения:

**По типу (byType)**
```java
@Autowired
private UserService userService; // Spring найдет бин типа UserService
```

**По имени (byName)**
```java
@Autowired
@Qualifier("userServiceImpl")
private UserService userService; // Spring найдет бин с именем "userServiceImpl"
```

**По конструктору**
```java
public UserController(UserService userService) {
    this.userService = userService;
}
```

#### Преимущества DI:
- **Тестируемость** - легко подставить моки
- **Слабая связанность** - классы не создают зависимости
- **Гибкость** - можно менять реализации
- **Читаемость** - зависимости видны явно

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 8. Какие бины будут использоваться для настройки приложения?

#### @Configuration
```java
@Configuration
@ComponentScan("com.example")
@PropertySource("classpath:application.properties")
public class AppConfig {
    
    @Bean
    @Primary
    public DataSource dataSource() {
        return new HikariDataSource();
    }
    
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

#### @Bean
```java
@Bean(name = "userService")
@Scope("singleton")
@Lazy
public UserService userService() {
    return new UserServiceImpl();
}
```

#### @ComponentScan
```java
@Configuration
@ComponentScan(basePackages = {"com.example.service", "com.example.repository"})
public class ServiceConfig {
}
```

#### @PropertySource
```java
@Configuration
@PropertySource("classpath:database.properties")
@PropertySource("classpath:email.properties")
public class PropertyConfig {
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 9. Как получить данные из файла .property?

#### application.properties
```properties
database.url=jdbc:mysql://localhost:3306/mydb
database.username=root
database.password=password
app.name=My Application
app.version=1.0.0
```

#### Через @Value
```java
@Component
public class DatabaseConfig {
    @Value("${database.url}")
    private String databaseUrl;
    
    @Value("${database.username}")
    private String username;
    
    @Value("${database.password}")
    private String password;
    
    @Value("${app.name:Default App}") // значение по умолчанию
    private String appName;
}
```

#### Через Environment
```java
@Component
public class DatabaseConfig {
    @Autowired
    private Environment env;
    
    public String getDatabaseUrl() {
        return env.getProperty("database.url");
    }
    
    public String getRequiredProperty(String key) {
        return env.getRequiredProperty(key); // бросает исключение если свойство не найдено
    }
}
```

#### Через @ConfigurationProperties
```java
@ConfigurationProperties(prefix = "database")
@Component
public class DatabaseProperties {
    private String url;
    private String username;
    private String password;
    private int maxConnections = 10;
    
    // геттеры и сеттеры
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 10. Как запустить Spring-приложение на Tomcat?

#### Способ 1: WAR-артефакт
```xml
<!-- pom.xml -->
<packaging>war</packaging>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
    </dependency>
</dependencies>
```

```xml
<!-- web.xml -->
<web-app>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring-config.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

#### Способ 2: Spring Boot с встроенным Tomcat
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

#### Способ 3: Программная конфигурация
```java
public class TomcatStarter {
    public static void main(String[] args) throws Exception {
        Tomcat tomcat = new Tomcat();
        tomcat.setPort(8080);
        
        Context context = tomcat.addWebapp("/", new File("src/main/webapp").getAbsolutePath());
        
        // Настройка Spring
        AnnotationConfigWebApplicationContext appContext = new AnnotationConfigWebApplicationContext();
        appContext.register(AppConfig.class);
        
        context.addApplicationListener(new ContextLoaderListener(appContext));
        
        tomcat.start();
        tomcat.getServer().await();
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 11. Что такое Artifacts?

**Артефакты** - это файлы, создаваемые в процессе сборки проекта.

#### Типы артефактов:

**JAR (Java Archive)**
- Содержит скомпилированные классы
- Используется для библиотек
- Может содержать ресурсы

**WAR (Web Archive)**
- Содержит веб-приложение
- Включает JAR + веб-ресурсы
- Развертывается на сервере приложений

**EAR (Enterprise Archive)**
- Содержит несколько модулей
- Используется в Java EE
- Может содержать WAR и JAR

#### Создание артефактов в Maven:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.2.3</version>
            <configuration>
                <warName>myapp</warName>
            </configuration>
        </plugin>
    </plugins>
</build>
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 12. В чем отличие артефакта war от war exploded?

#### WAR (Web Archive)
```
myapp.war
├── WEB-INF/
│   ├── classes/
│   ├── lib/
│   └── web.xml
├── index.html
└── css/
```

**Особенности:**
- **Сжатый архив** - все файлы упакованы
- **Быстрое развертывание** - один файл
- **Требует пересборки** при изменениях
- **Подходит для продакшена**

#### WAR Exploded
```
myapp/
├── WEB-INF/
│   ├── classes/
│   ├── lib/
│   └── web.xml
├── index.html
└── css/
```

**Особенности:**
- **Распакованный архив** - файлы в файловой системе
- **Горячая перезагрузка** - изменения видны сразу
- **Медленное развертывание** - много файлов
- **Подходит для разработки**

#### Настройка в IntelliJ IDEA:
1. **WAR**: Build → Build Artifacts → Build
2. **WAR Exploded**: Run → Edit Configurations → Deployment → Add → Artifact → WAR Exploded

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 13. Какая разница между аннотациями @Component, @Repository и @Service?

#### Иерархия аннотаций:
```
@Component (базовый)
├── @Repository (DAO слой)
├── @Service (бизнес-логика)
└── @Controller (веб-слой)
```

#### @Component
```java
@Component
public class UserValidator {
    public boolean isValid(User user) {
        return user != null && user.getName() != null;
    }
}
```

**Особенности:**
- **Базовый стереотип** - Spring сканирует и создает бин
- **Универсальное использование** - для любых компонентов
- **Автоматическое обнаружение** - @ComponentScan найдет

#### @Repository
```java
@Repository
public class UserRepository {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public User findById(Long id) {
        return jdbcTemplate.queryForObject("SELECT * FROM users WHERE id = ?", 
            new Object[]{id}, new UserRowMapper());
    }
}
```

**Особенности:**
- **Семантика DAO** - доступ к данным
- **Обработка исключений** - автоматически конвертирует SQLException в DataAccessException
- **Транзакционность** - может участвовать в транзакциях
- **Тестирование** - легко мокать для unit-тестов

#### @Service
```java
@Service
@Transactional
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private EmailService emailService;
    
    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        emailService.sendWelcomeEmail(savedUser);
        return savedUser;
    }
}
```

**Особенности:**
- **Бизнес-логика** - основная логика приложения
- **Транзакционность** - обычно содержит @Transactional
- **Координация** - объединяет несколько репозиториев
- **Сервисный слой** - между контроллером и репозиторием

#### Практические рекомендации:

**Используйте @Repository для:**
- Доступа к базе данных
- Работы с внешними API
- Кэширования данных

**Используйте @Service для:**
- Бизнес-логики
- Валидации
- Координации между компонентами

**Используйте @Component для:**
- Утилитарных классов
- Валидаторов
- Конвертеров

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 14. Как выглядит структура MVC-приложения?

#### Классическая структура:
```
src/main/java/
├── com.example.controller/     # Веб-слой
│   ├── UserController.java
│   └── OrderController.java
├── com.example.service/        # Бизнес-логика
│   ├── UserService.java
│   └── OrderService.java
├── com.example.repository/     # Доступ к данным
│   ├── UserRepository.java
│   └── OrderRepository.java
├── com.example.model/          # Модели данных
│   ├── User.java
│   └── Order.java
└── com.example.config/         # Конфигурация
    ├── AppConfig.java
    └── DatabaseConfig.java
```

#### Поток данных:
```
HTTP Request
    ↓
Controller (@Controller)
    ↓
Service (@Service)
    ↓
Repository (@Repository)
    ↓
Database
    ↓
Response (JSON/HTML)
```

#### Детальный пример:

**Controller (веб-слой)**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
}
```

**Service (бизнес-логика)**
```java
@Service
@Transactional
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private EmailService emailService;
    
    public User findById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException("User not found"));
    }
    
    public User createUser(User user) {
        validateUser(user);
        User savedUser = userRepository.save(user);
        emailService.sendWelcomeEmail(savedUser);
        return savedUser;
    }
    
    private void validateUser(User user) {
        if (user.getName() == null || user.getName().trim().isEmpty()) {
            throw new ValidationException("User name is required");
        }
    }
}
```

**Repository (доступ к данным)**
```java
@Repository
public class UserRepository {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public Optional<User> findById(Long id) {
        String sql = "SELECT * FROM users WHERE id = ?";
        try {
            User user = jdbcTemplate.queryForObject(sql, new Object[]{id}, new UserRowMapper());
            return Optional.of(user);
        } catch (EmptyResultDataAccessException e) {
            return Optional.empty();
        }
    }
    
    public User save(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        KeyHolder keyHolder = new GeneratedKeyHolder();
        
        jdbcTemplate.update(connection -> {
            PreparedStatement ps = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
            ps.setString(1, user.getName());
            ps.setString(2, user.getEmail());
            return ps;
        }, keyHolder);
        
        user.setId(keyHolder.getKey().longValue());
        return user;
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 15. Чем контроллер отличается от сервлета?

#### Сервлет (низкоуровневый)
```java
public class UserServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException {
        
        // Парсинг параметров
        String id = request.getParameter("id");
        
        // Валидация
        if (id == null || id.trim().isEmpty()) {
            response.sendError(HttpServletResponse.SC_BAD_REQUEST, "ID is required");
            return;
        }
        
        // Бизнес-логика
        UserService userService = new UserService();
        User user = userService.findById(Long.parseLong(id));
        
        // Сериализация ответа
        response.setContentType("application/json");
        ObjectMapper mapper = new ObjectMapper();
        mapper.writeValue(response.getWriter(), user);
    }
}
```

**Проблемы сервлетов:**
- **Много boilerplate кода** - парсинг, валидация, сериализация
- **Жесткая связанность** - создание сервисов вручную
- **Сложность тестирования** - нужно мокать HttpServletRequest/Response
- **Повторяющийся код** - для каждого endpoint

#### Spring Controller (высокоуровневый)
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}
```

**Преимущества контроллеров:**
- **Минимум кода** - Spring обрабатывает HTTP детали
- **Автоматическая сериализация** - JSON/XML из коробки
- **Валидация** - @Valid, @NotNull аннотации
- **Внедрение зависимостей** - @Autowired
- **Легкое тестирование** - можно тестировать как обычный Java-класс

#### Сравнение:

| Аспект | Сервлет | Spring Controller |
|--------|---------|-------------------|
| **Код** | Много boilerplate | Минимум кода |
| **HTTP** | Ручная обработка | Автоматическая |
| **Валидация** | Ручная | Аннотации |
| **Сериализация** | Ручная | Автоматическая |
| **Тестирование** | Сложное | Простое |
| **Конфигурация** | web.xml | Аннотации |

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 16. Какая основная зависимость фреймворка Spring?

#### spring-context - ядро Spring
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.21</version>
</dependency>
```

**Что включает spring-context:**
- **IoC контейнер** - управление бинами
- **Dependency Injection** - внедрение зависимостей
- **ApplicationContext** - основной интерфейс
- **BeanFactory** - фабрика бинов
- **Аннотации** - @Component, @Autowired, @Value

#### Почему не указывается явно:

**Транзитивные зависимости:**
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.21</version>
</dependency>
```

**spring-webmvc включает:**
- spring-context (IoC)
- spring-web (веб-функции)
- spring-aop (аспекты)
- spring-beans (управление бинами)

#### Иерархия зависимостей:
```
spring-boot-starter-web
├── spring-boot-starter
│   ├── spring-boot
│   └── spring-boot-autoconfigure
├── spring-webmvc
│   ├── spring-context
│   ├── spring-web
│   └── spring-aop
└── spring-boot-starter-tomcat
```

#### Когда указывать явно:
```xml
<!-- Если используете только IoC без веб-функций -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
</dependency>

<!-- Если нужны только бины без контекста -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
</dependency>
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 17. Как вернуть страницу в контроллере? Как вернуть данные?

#### Возврат страниц (View)

**Thymeleaf шаблон:**
```java
@Controller
public class UserController {
    @GetMapping("/users")
    public String getUsers(Model model) {
        List<User> users = userService.getAllUsers();
        model.addAttribute("users", users);
        return "users/list"; // templates/users/list.html
    }
    
    @GetMapping("/users/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userService.findById(id);
        model.addAttribute("user", user);
        return "users/detail"; // templates/users/detail.html
    }
}
```

**JSP страница:**
```java
@Controller
public class UserController {
    @GetMapping("/users")
    public ModelAndView getUsers() {
        List<User> users = userService.getAllUsers();
        ModelAndView modelAndView = new ModelAndView("users/list");
        modelAndView.addObject("users", users);
        return modelAndView;
    }
}
```

#### Возврат данных (API)

**JSON ответ:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
}
```

**XML ответ:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping(produces = MediaType.APPLICATION_XML_VALUE)
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
}
```

**Кастомный ответ:**
```java
@RestController
public class UserController {
    @GetMapping("/users/export")
    public ResponseEntity<byte[]> exportUsers() {
        byte[] csvData = userService.exportUsersToCsv();
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        headers.setContentDispositionFormData("attachment", "users.csv");
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(csvData);
    }
}
```

#### Обработка ошибок:
```java
@RestController
public class UserController {
    @GetMapping("/users/{id}")
    public ResponseEntity<?> getUser(@PathVariable Long id) {
        try {
            User user = userService.findById(id);
            return ResponseEntity.ok(user);
        } catch (UserNotFoundException e) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND)
                .body(new ErrorResponse("User not found"));
        }
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 18. Принципы работы Spring

#### 1. Inversion of Control (IoC)
**Принцип:** Объекты не создают свои зависимости, а получают их извне.

**Реализация в Spring:**
```java
// Плохо - жесткая связанность
public class OrderService {
    private PaymentService paymentService = new PaymentService();
}

// Хорошо - IoC
@Service
public class OrderService {
    private final PaymentService paymentService;
    
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService; // Spring внедряет
    }
}
```

#### 2. Dependency Injection (DI)
**Принцип:** Внедрение зависимостей извне.

**Типы внедрения:**
```java
// Constructor Injection (рекомендуемый)
public OrderService(PaymentService paymentService) {
    this.paymentService = paymentService;
}

// Setter Injection
@Autowired
public void setPaymentService(PaymentService paymentService) {
    this.paymentService = paymentService;
}

// Field Injection (не рекомендуется)
@Autowired
private PaymentService paymentService;
```

#### 3. Aspect-Oriented Programming (AOP)
**Принцип:** Разделение сквозной функциональности (логирование, транзакции, безопасность).

**Пример:**
```java
@Service
@Transactional
public class UserService {
    @Cacheable("users")
    public User findById(Long id) {
        return userRepository.findById(id);
    }
    
    @Async
    public void sendEmail(User user) {
        // отправка email
    }
}
```

#### 4. Модульность
**Принцип:** Разделение на независимые модули.

**Модули Spring:**
- **spring-core** - базовые функции
- **spring-context** - IoC контейнер
- **spring-web** - веб-функции
- **spring-data** - работа с данными
- **spring-security** - безопасность

#### 5. Конфигурируемость
**Принцип:** Гибкая настройка без изменения кода.

**Способы конфигурации:**
```java
// XML конфигурация
<bean id="userService" class="com.example.UserService">
    <property name="userRepository" ref="userRepository"/>
</bean>

// Аннотации
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}

// Java конфигурация
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService(userRepository());
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 19. Связывание бинов и жизненный цикл

#### Жизненный цикл бина:

**1. Создание экземпляра**
```java
// Spring создает объект через конструктор или фабричный метод
UserService userService = new UserService();
```

**2. Внедрение зависимостей**
```java
// Spring внедряет зависимости
userService.setUserRepository(userRepository);
userService.setEmailService(emailService);
```

**3. Инициализация**
```java
@Component
public class UserService {
    @PostConstruct
    public void init() {
        // инициализация после внедрения зависимостей
        System.out.println("UserService initialized");
    }
}
```

**4. Готов к использованию**
```java
// Бин готов к использованию
User user = userService.findById(1L);
```

**5. Уничтожение**
```java
@Component
public class UserService {
    @PreDestroy
    public void cleanup() {
        // очистка ресурсов перед уничтожением
        System.out.println("UserService destroyed");
    }
}
```

#### Связывание бинов:

**По типу (byType):**
```java
@Autowired
private UserRepository userRepository; // Spring найдет бин типа UserRepository
```

**По имени (byName):**
```java
@Autowired
@Qualifier("userRepositoryImpl")
private UserRepository userRepository; // Spring найдет бин с именем "userRepositoryImpl"
```

**По конструктору:**
```java
public UserService(UserRepository userRepository, EmailService emailService) {
    this.userRepository = userRepository;
    this.emailService = emailService;
}
```

#### Управление жизненным циклом:

**@Scope:**
```java
@Component
@Scope("prototype") // новый экземпляр при каждом запросе
public class UserService {
    // ...
}

@Component
@Scope("singleton") // один экземпляр (по умолчанию)
public class UserService {
    // ...
}
```

**@Lazy:**
```java
@Component
@Lazy // создается только при первом использовании
public class UserService {
    // ...
}
```

**@DependsOn:**
```java
@Component
@DependsOn("userRepository") // создается после userRepository
public class UserService {
    // ...
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 20. Основные паттерны Spring

#### 1. Singleton Pattern
**Назначение:** Один экземпляр на весь контейнер.

```java
@Component
@Scope("singleton") // по умолчанию
public class UserService {
    // Spring гарантирует один экземпляр
}
```

#### 2. Factory Pattern
**Назначение:** Создание объектов через фабрику.

```java
@Configuration
public class AppConfig {
    @Bean
    public DataSource dataSource() {
        return new HikariDataSource();
    }
    
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

#### 3. Template Method Pattern
**Назначение:** Определение алгоритма с возможностью переопределения шагов.

```java
@Service
public class UserService {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public List<User> findAll() {
        return jdbcTemplate.query("SELECT * FROM users", new UserRowMapper());
    }
}
```

#### 4. Proxy Pattern
**Назначение:** Создание прокси для AOP.

```java
@Service
@Transactional
public class UserService {
    // Spring создает прокси для @Transactional
    public User save(User user) {
        return userRepository.save(user);
    }
}
```

#### 5. Observer Pattern
**Назначение:** Уведомление о событиях.

```java
@Component
public class UserService {
    @Autowired
    private ApplicationEventPublisher eventPublisher;
    
    public User createUser(User user) {
        User savedUser = userRepository.save(user);
        eventPublisher.publishEvent(new UserCreatedEvent(savedUser));
        return savedUser;
    }
}

@Component
public class UserEventListener {
    @EventListener
    public void handleUserCreated(UserCreatedEvent event) {
        // обработка события
    }
}
```

#### 6. Strategy Pattern
**Назначение:** Разные реализации одного интерфейса.

```java
public interface PaymentStrategy {
    void pay(BigDecimal amount);
}

@Component("creditCard")
public class CreditCardPayment implements PaymentStrategy {
    @Override
    public void pay(BigDecimal amount) {
        // логика оплаты картой
    }
}

@Component("paypal")
public class PayPalPayment implements PaymentStrategy {
    @Override
    public void pay(BigDecimal amount) {
        // логика оплаты через PayPal
    }
}

@Service
public class PaymentService {
    @Autowired
    private Map<String, PaymentStrategy> paymentStrategies;
    
    public void processPayment(String paymentType, BigDecimal amount) {
        PaymentStrategy strategy = paymentStrategies.get(paymentType);
        strategy.pay(amount);
    }
}
```

#### 7. Decorator Pattern
**Назначение:** Добавление функциональности без изменения класса.

```java
@Service
public class UserService {
    @Cacheable("users")
    @Transactional
    public User findById(Long id) {
        return userRepository.findById(id);
    }
}
```

#### 8. Adapter Pattern
**Назначение:** Адаптация интерфейсов.

```java
@Component
public class LegacyUserServiceAdapter implements UserService {
    @Autowired
    private LegacyUserService legacyService;
    
    @Override
    public User findById(Long id) {
        LegacyUser legacyUser = legacyService.getUser(id);
        return convertToUser(legacyUser);
    }
}
```

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

### 22. Объекты Principal, Authorities, Authentication

#### Principal
**Principal** - представляет текущего пользователя в системе.

```java
@Service
public class UserService {
    @Autowired
    private Authentication authentication;
    
    public String getCurrentUsername() {
        if (authentication != null && authentication.isAuthenticated()) {
            return authentication.getName(); // возвращает username
        }
        return "anonymous";
    }
    
    public User getCurrentUser() {
        if (authentication != null && authentication.getPrincipal() instanceof UserDetails) {
            UserDetails userDetails = (UserDetails) authentication.getPrincipal();
            return userRepository.findByUsername(userDetails.getUsername());
        }
        return null;
    }
}
```

#### Authorities (полномочия)
**Authorities** - набор прав доступа пользователя.

```java
@Service
public class AuthorityService {
    public Set<GrantedAuthority> getUserAuthorities(User user) {
        Set<GrantedAuthority> authorities = new HashSet<>();
        
        // Добавляем роль
        authorities.add(new SimpleGrantedAuthority("ROLE_USER"));
        
        // Добавляем права
        if (user.isAdmin()) {
            authorities.add(new SimpleGrantedAuthority("ROLE_ADMIN"));
            authorities.add(new SimpleGrantedAuthority("USER_WRITE"));
            authorities.add(new SimpleGrantedAuthority("USER_DELETE"));
        }
        
        if (user.canManageOrders()) {
            authorities.add(new SimpleGrantedAuthority("ORDER_MANAGE"));
        }
        
        return authorities;
    }
}
```

#### Authentication (аутентификация)
**Authentication** - объект, содержащий информацию о пользователе и его полномочиях.

```java
@Service
public class AuthService {
    public void processAuthentication(Authentication authentication) {
        // Получаем имя пользователя
        String username = authentication.getName();
        
        // Получаем полномочия
        Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();
        
        // Проверяем аутентификацию
        if (authentication.isAuthenticated()) {
            System.out.println("User " + username + " is authenticated");
            
            // Проверяем наличие роли
            boolean isAdmin = authorities.stream()
                .anyMatch(auth -> auth.getAuthority().equals("ROLE_ADMIN"));
            
            if (isAdmin) {
                System.out.println("User has admin privileges");
            }
        }
        
        // Получаем детали (если есть)
        Object principal = authentication.getPrincipal();
        if (principal instanceof UserDetails) {
            UserDetails userDetails = (UserDetails) principal;
            boolean isAccountNonExpired = userDetails.isAccountNonExpired();
            boolean isAccountNonLocked = userDetails.isAccountNonLocked();
            boolean isCredentialsNonExpired = userDetails.isCredentialsNonExpired();
            boolean isEnabled = userDetails.isEnabled();
        }
    }
}
```

#### Кастомная реализация UserDetails

```java
public class CustomUserDetails implements UserDetails {
    private User user;
    
    public CustomUserDetails(User user) {
        this.user = user;
    }
    
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        Set<GrantedAuthority> authorities = new HashSet<>();
        authorities.add(new SimpleGrantedAuthority("ROLE_" + user.getRole()));
        
        // Добавляем права на основе роли
        if (user.isAdmin()) {
            authorities.add(new SimpleGrantedAuthority("USER_WRITE"));
            authorities.add(new SimpleGrantedAuthority("USER_DELETE"));
        }
        
        return authorities;
    }
    
    @Override
    public String getPassword() {
        return user.getPassword();
    }
    
    @Override
    public String getUsername() {
        return user.getUsername();
    }
    
    @Override
    public boolean isAccountNonExpired() {
        return !user.isExpired();
    }
    
    @Override
    public boolean isAccountNonLocked() {
        return !user.isLocked();
    }
    
    @Override
    public boolean isCredentialsNonExpired() {
        return !user.isPasswordExpired();
    }
    
    @Override
    public boolean isEnabled() {
        return user.isActive();
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 23. Чем отличается InMemoryAuthentication от BasicAuthentication?

#### InMemoryAuthentication (в памяти)

```java
@Configuration
@EnableWebSecurity
public class InMemorySecurityConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        
        // Создаем пользователей в памяти
        manager.createUser(User.withUsername("admin")
            .password(passwordEncoder().encode("admin123"))
            .roles("ADMIN")
            .authorities("USER_READ", "USER_WRITE", "USER_DELETE")
            .build());
            
        manager.createUser(User.withUsername("user")
            .password(passwordEncoder().encode("user123"))
            .roles("USER")
            .authorities("USER_READ")
            .build());
            
        return manager;
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .requestMatchers("/user/**").hasRole("USER")
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults());
        
        return http.build();
    }
}
```

**Особенности InMemoryAuthentication:**
- **Пользователи в памяти** - данные теряются при перезапуске
- **Простая настройка** - для тестирования и разработки
- **Быстрая инициализация** - нет обращения к БД
- **Не масштабируется** - не подходит для продакшена

#### BasicAuthentication (базовая аутентификация)

```java
@Configuration
@EnableWebSecurity
public class BasicAuthSecurityConfig {
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults())
            .csrf().disable(); // Отключаем CSRF для API
        
        return http.build();
    }
}
```

#### Database Authentication (рекомендуемый)

```java
@Configuration
@EnableWebSecurity
public class DatabaseSecurityConfig {
    
    @Autowired
    private CustomUserDetailsService userDetailsService;
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/login", "/register").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .defaultSuccessUrl("/dashboard")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutSuccessUrl("/login?logout")
                .permitAll()
            );
        
        return http.build();
    }
}

@Service
public class CustomUserDetailsService implements UserDetailsService {
    
    @Autowired
    private UserRepository userRepository;
    
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username)
            .orElseThrow(() -> new UsernameNotFoundException("User not found: " + username));
        
        return new CustomUserDetails(user);
    }
}
```

#### Сравнение методов аутентификации:

| Аспект | InMemory | Basic Auth | Database Auth |
|--------|----------|------------|---------------|
| **Хранение** | В памяти | Любой источник | База данных |
| **Постоянство** | Нет | Да | Да |
| **Масштабируемость** | Низкая | Высокая | Высокая |
| **Безопасность** | Средняя | Высокая | Высокая |
| **Сложность** | Низкая | Средняя | Высокая |
| **Использование** | Тесты | API | Продакшен |

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 24. Как добавить секьюрность к контроллеру?

#### Способ 1: Аннотации на уровне метода

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @PreAuthorize("hasRole('ADMIN')")
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PreAuthorize("hasRole('USER') and #id == authentication.name")
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PreAuthorize("hasAuthority('USER_WRITE')")
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
    
    @PreAuthorize("hasRole('ADMIN') or (hasRole('USER') and @userService.isOwner(#id, authentication.name))")
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    @PreAuthorize("hasAuthority('USER_DELETE')")
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

#### Способ 2: Аннотации на уровне класса

```java
@RestController
@RequestMapping("/api/admin")
@PreAuthorize("hasRole('ADMIN')")
public class AdminController {
    
    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PreAuthorize("hasAuthority('USER_DELETE')")
    @DeleteMapping("/users/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

#### Способ 3: Конфигурация в SecurityConfig

```java
@Configuration
@EnableWebSecurity
@EnableMethodSecurity(prePostEnabled = true)
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/public/**").permitAll()
                .requestMatchers(HttpMethod.GET, "/api/users").hasRole("USER")
                .requestMatchers(HttpMethod.POST, "/api/users").hasAuthority("USER_WRITE")
                .requestMatchers(HttpMethod.PUT, "/api/users/**").hasRole("USER")
                .requestMatchers(HttpMethod.DELETE, "/api/users/**").hasRole("ADMIN")
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(withDefaults())
            .csrf().disable();
        
        return http.build();
    }
}
```

#### Способ 4: Кастомные проверки

```java
@RestController
@RequestMapping("/api/documents")
public class DocumentController {
    
    @PreAuthorize("@documentSecurityService.canAccess(#id, authentication.name)")
    @GetMapping("/{id}")
    public Document getDocument(@PathVariable Long id) {
        return documentService.findById(id);
    }
    
    @PreAuthorize("@documentSecurityService.canEdit(#id, authentication.name)")
    @PutMapping("/{id}")
    public Document updateDocument(@PathVariable Long id, @RequestBody Document document) {
        return documentService.update(id, document);
    }
}

@Service
public class DocumentSecurityService {
    
    @Autowired
    private DocumentRepository documentRepository;
    
    public boolean canAccess(Long documentId, String username) {
        Document document = documentRepository.findById(documentId);
        return document.getOwner().equals(username) || 
               document.getSharedUsers().contains(username);
    }
    
    public boolean canEdit(Long documentId, String username) {
        Document document = documentRepository.findById(documentId);
        return document.getOwner().equals(username);
    }
}
```

#### Способ 5: Проверка в коде контроллера

```java
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    
    @Autowired
    private SecurityService securityService;
    
    @GetMapping("/{id}")
    public Order getOrder(@PathVariable Long id, Authentication authentication) {
        Order order = orderService.findById(id);
        
        // Проверяем права доступа
        if (!securityService.canAccessOrder(order, authentication)) {
            throw new AccessDeniedException("You don't have permission to access this order");
        }
        
        return order;
    }
    
    @PostMapping
    public Order createOrder(@RequestBody Order order, Authentication authentication) {
        // Проверяем, что пользователь может создавать заказы
        if (!securityService.canCreateOrder(authentication)) {
            throw new AccessDeniedException("You don't have permission to create orders");
        }
        
        return orderService.create(order, authentication.getName());
    }
}
```

#### Настройка для включения аннотаций

```java
@Configuration
@EnableMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig {
    // Дополнительная настройка если нужна
}
```

#### Практические рекомендации:

**Используйте аннотации для:**
- Простых проверок ролей и прав
- Читаемого и явного кода
- Централизованного управления безопасностью

**Используйте конфигурацию для:**
- Сложных URL-паттернов
- Глобальных правил безопасности
- Различных методов HTTP

**Используйте кастомные проверки для:**
- Бизнес-логики безопасности
- Сложных условий доступа
- Интеграции с внешними системами

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

### 26. Как работают каскады для таблиц и какие они бывают?

#### Типы каскадов в JPA

**CascadeType.ALL** - все операции каскадируются
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();
    
    // При удалении пользователя удаляются все его заказы
    // При сохранении пользователя сохраняются все заказы
}
```

**CascadeType.PERSIST** - только сохранение
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.PERSIST)
    private List<Address> addresses = new ArrayList<>();
    
    // При сохранении пользователя сохраняются адреса
    // При удалении пользователя адреса НЕ удаляются
}
```

**CascadeType.MERGE** - только обновление
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.MERGE)
    private List<Profile> profiles = new ArrayList<>();
    
    // При обновлении пользователя обновляются профили
    // При удалении пользователя профили НЕ удаляются
}
```

**CascadeType.REMOVE** - только удаление
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.REMOVE)
    private List<Session> sessions = new ArrayList<>();
    
    // При удалении пользователя удаляются сессии
    // При сохранении пользователя сессии НЕ сохраняются
}
```

**CascadeType.REFRESH** - только обновление из БД
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.REFRESH)
    private List<Log> logs = new ArrayList<>();
    
    // При обновлении пользователя из БД обновляются логи
}
```

**CascadeType.DETACH** - только отключение от контекста
```java
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.DETACH)
    private List<Cache> caches = new ArrayList<>();
    
    // При отключении пользователя от контекста отключаются кэши
}
```

#### Комбинирование каскадов

```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // Для заказов - полный каскад
    @OneToMany(mappedBy = "user", cascade = {CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE})
    private List<Order> orders = new ArrayList<>();
    
    // Для адресов - только сохранение и обновление
    @OneToMany(mappedBy = "user", cascade = {CascadeType.PERSIST, CascadeType.MERGE})
    private List<Address> addresses = new ArrayList<>();
    
    // Для логов - только обновление
    @OneToMany(mappedBy = "user", cascade = CascadeType.MERGE)
    private List<Log> logs = new ArrayList<>();
}
```

#### Практические примеры

**Сохранение с каскадом:**
```java
@Service
@Transactional
public class UserService {
    
    public User createUserWithOrders(User user, List<Order> orders) {
        // Устанавливаем связь
        user.setOrders(orders);
        orders.forEach(order -> order.setUser(user));
        
        // Сохраняем только пользователя - заказы сохранятся автоматически
        return userRepository.save(user);
    }
}
```

**Удаление с каскадом:**
```java
@Service
@Transactional
public class UserService {
    
    public void deleteUser(Long userId) {
        User user = userRepository.findById(userId)
            .orElseThrow(() -> new UserNotFoundException("User not found"));
        
        // Удаляем только пользователя - заказы удалятся автоматически
        userRepository.delete(user);
    }
}
```

#### Осторожность с каскадами

**Проблема N+1 запросов:**
```java
// Плохо - может вызвать N+1 проблему
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    private List<Order> orders = new ArrayList<>();
}

// Хорошо - используем LAZY загрузку
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, fetch = FetchType.LAZY)
    private List<Order> orders = new ArrayList<>();
}
```

**Проблема с производительностью:**
```java
// Осторожно с большими коллекциями
@Entity
public class User {
    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
    private List<Log> logs = new ArrayList<>(); // Может быть миллионы записей!
    
    // Лучше использовать отдельный сервис для логов
}
```

#### Рекомендации по использованию каскадов:

**Используйте CascadeType.ALL для:**
- Тесно связанных сущностей (User-Profile)
- Небольших коллекций
- Сущностей, которые не имеют смысла без родителя

**Используйте CascadeType.PERSIST для:**
- Создания связанных сущностей
- Когда дочерние сущности должны существовать только с родителем

**Используйте CascadeType.MERGE для:**
- Обновления связанных сущностей
- Когда нужно синхронизировать изменения

**НЕ используйте каскады для:**
- Больших коллекций
- Сущностей с собственной бизнес-логикой
- Связей, которые могут быть разорваны

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

### 28. Форматы данных в REST-сервисах

#### JSON (JavaScript Object Notation)

**Самый популярный формат для REST API:**

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PostMapping(consumes = MediaType.APPLICATION_JSON_VALUE)
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
}
```

**Пример JSON запроса/ответа:**
```json
// POST /api/users
{
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
}

// Ответ
{
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30,
    "createdAt": "2023-12-01T10:00:00Z"
}
```

#### XML (eXtensible Markup Language)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = MediaType.APPLICATION_XML_VALUE)
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @PostMapping(consumes = MediaType.APPLICATION_XML_VALUE)
    public User createUser(@RequestBody User user) {
        return userService.create(user);
    }
}
```

**Пример XML запроса/ответа:**
```xml
<!-- POST /api/users -->
<user>
    <name>John Doe</name>
    <email>john@example.com</email>
    <age>30</age>
</user>

<!-- Ответ -->
<user>
    <id>1</id>
    <name>John Doe</name>
    <email>john@example.com</email>
    <age>30</age>
    <createdAt>2023-12-01T10:00:00Z</createdAt>
</user>
```

#### Поддержка нескольких форматов

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @GetMapping(value = "/{id}", produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
}
```

#### Кастомные форматы

**CSV формат:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(value = "/export", produces = "text/csv")
    public ResponseEntity<String> exportUsers() {
        List<User> users = userService.findAll();
        String csv = convertToCsv(users);
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.valueOf("text/csv"));
        headers.setContentDispositionFormData("attachment", "users.csv");
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(csv);
    }
    
    private String convertToCsv(List<User> users) {
        StringBuilder csv = new StringBuilder();
        csv.append("ID,Name,Email,Age\n");
        
        for (User user : users) {
            csv.append(user.getId()).append(",")
               .append(user.getName()).append(",")
               .append(user.getEmail()).append(",")
               .append(user.getAge()).append("\n");
        }
        
        return csv.toString();
    }
}
```

**YAML формат:**
```java
@RestController
@RequestMapping("/api/config")
public class ConfigController {
    
    @GetMapping(produces = "application/x-yaml")
    public ResponseEntity<String> getConfig() {
        String yaml = """
            database:
              host: localhost
              port: 5432
              name: myapp
            server:
              port: 8080
            """;
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.valueOf("application/x-yaml"));
        
        return ResponseEntity.ok()
            .headers(headers)
            .body(yaml);
    }
}
```

#### Настройка сериализации

**Jackson конфигурация:**
```java
@Configuration
public class JacksonConfig {
    
    @Bean
    @Primary
    public ObjectMapper objectMapper() {
        ObjectMapper mapper = new ObjectMapper();
        
        // Настройки для JSON
        mapper.setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);
        mapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
        mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        
        // Форматирование дат
        mapper.registerModule(new JavaTimeModule());
        mapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);
        
        return mapper;
    }
}
```

**JAXB конфигурация для XML:**
```java
@Configuration
public class JaxbConfig {
    
    @Bean
    public Jaxb2Marshaller jaxb2Marshaller() {
        Jaxb2Marshaller marshaller = new Jaxb2Marshaller();
        marshaller.setPackagesToScan("com.example.model");
        return marshaller;
    }
}
```

#### Сравнение форматов:

| Формат | Размер | Читаемость | Производительность | Поддержка |
|--------|--------|------------|-------------------|-----------|
| **JSON** | Компактный | Высокая | Высокая | Универсальная |
| **XML** | Большой | Средняя | Средняя | Широкая |
| **CSV** | Очень компактный | Низкая | Очень высокая | Ограниченная |
| **YAML** | Средний | Очень высокая | Низкая | Ограниченная |

#### Рекомендации по выбору формата:

**Используйте JSON для:**
- REST API
- Веб-приложений
- Мобильных приложений
- Микросервисов

**Используйте XML для:**
- Enterprise систем
- SOAP сервисов
- Систем с требованиями к валидации
- Интеграции с legacy системами

**Используйте CSV для:**
- Экспорта данных
- Аналитики
- Массовой обработки
- Простых структур данных

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

### 31. RestTemplate и его методы

#### Что такое RestTemplate

**RestTemplate** - это клиент для работы с REST API в Spring. Позволяет легко отправлять HTTP запросы и получать ответы.

#### Настройка RestTemplate

```java
@Configuration
public class RestTemplateConfig {
    
    @Bean
    public RestTemplate restTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        
        // Добавляем перехватчики
        restTemplate.setInterceptors(List.of(new LoggingInterceptor()));
        
        // Настраиваем таймауты
        HttpComponentsClientHttpRequestFactory factory = new HttpComponentsClientHttpRequestFactory();
        factory.setConnectTimeout(5000);
        factory.setReadTimeout(10000);
        restTemplate.setRequestFactory(factory);
        
        return restTemplate;
    }
}
```

#### Основные методы RestTemplate

**GET запросы:**
```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    // GET с возвратом объекта
    public User getUser(Long id) {
        String url = "http://localhost:8080/api/users/" + id;
        return restTemplate.getForObject(url, User.class);
    }
    
    // GET с возвратом ResponseEntity
    public ResponseEntity<User> getUserWithHeaders(Long id) {
        String url = "http://localhost:8080/api/users/" + id;
        return restTemplate.getForEntity(url, User.class);
    }
    
    // GET с параметрами
    public List<User> getUsers(String name, Integer age) {
        String url = "http://localhost:8080/api/users?name={name}&age={age}";
        Map<String, Object> params = Map.of("name", name, "age", age);
        
        User[] users = restTemplate.getForObject(url, User[].class, params);
        return Arrays.asList(users);
    }
    
    // GET с URI переменными
    public User getUserByUri(Long id) {
        String url = "http://localhost:8080/api/users/{id}";
        Map<String, Object> uriVariables = Map.of("id", id);
        
        return restTemplate.getForObject(url, User.class, uriVariables);
    }
}
```

**POST запросы:**
```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    // POST с объектом в теле
    public User createUser(User user) {
        String url = "http://localhost:8080/api/users";
        return restTemplate.postForObject(url, user, User.class);
    }
    
    // POST с возвратом ResponseEntity
    public ResponseEntity<User> createUserWithResponse(User user) {
        String url = "http://localhost:8080/api/users";
        return restTemplate.postForEntity(url, user, User.class);
    }
    
    // POST с получением Location заголовка
    public String createUserAndGetLocation(User user) {
        String url = "http://localhost:8080/api/users";
        URI location = restTemplate.postForLocation(url, user);
        return location.toString();
    }
}
```

**PUT запросы:**
```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    // PUT для обновления
    public void updateUser(Long id, User user) {
        String url = "http://localhost:8080/api/users/" + id;
        restTemplate.put(url, user);
    }
    
    // PUT с параметрами
    public void updateUserWithParams(Long id, String name, String email) {
        String url = "http://localhost:8080/api/users/{id}?name={name}&email={email}";
        Map<String, Object> params = Map.of("id", id, "name", name, "email", email);
        
        restTemplate.put(url, null, params);
    }
}
```

**DELETE запросы:**
```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    // DELETE для удаления
    public void deleteUser(Long id) {
        String url = "http://localhost:8080/api/users/" + id;
        restTemplate.delete(url);
    }
    
    // DELETE с параметрами
    public void deleteUserWithParams(Long id, String reason) {
        String url = "http://localhost:8080/api/users/{id}?reason={reason}";
        Map<String, Object> params = Map.of("id", id, "reason", reason);
        
        restTemplate.delete(url, params);
    }
}
```

#### Работа с заголовками

```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    public User getUserWithAuth(Long id, String token) {
        String url = "http://localhost:8080/api/users/" + id;
        
        HttpHeaders headers = new HttpHeaders();
        headers.set("Authorization", "Bearer " + token);
        headers.set("Accept", "application/json");
        
        HttpEntity<String> entity = new HttpEntity<>(headers);
        
        ResponseEntity<User> response = restTemplate.exchange(
            url, HttpMethod.GET, entity, User.class);
        
        return response.getBody();
    }
    
    public User createUserWithHeaders(User user, String token) {
        String url = "http://localhost:8080/api/users";
        
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.set("Authorization", "Bearer " + token);
        
        HttpEntity<User> entity = new HttpEntity<>(user, headers);
        
        return restTemplate.postForObject(url, entity, User.class);
    }
}
```

#### Обработка ошибок

```java
@Service
public class UserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    public User getUserSafely(Long id) {
        try {
            String url = "http://localhost:8080/api/users/" + id;
            return restTemplate.getForObject(url, User.class);
        } catch (HttpClientErrorException e) {
            if (e.getStatusCode() == HttpStatus.NOT_FOUND) {
                throw new UserNotFoundException("User not found with id: " + id);
            }
            throw new RuntimeException("Error fetching user", e);
        } catch (HttpServerErrorException e) {
            throw new RuntimeException("Server error", e);
        } catch (RestClientException e) {
            throw new RuntimeException("Network error", e);
        }
    }
}
```

#### Асинхронные запросы

```java
@Service
public class AsyncUserService {
    
    @Autowired
    private RestTemplate restTemplate;
    
    @Async
    public CompletableFuture<User> getUserAsync(Long id) {
        String url = "http://localhost:8080/api/users/" + id;
        User user = restTemplate.getForObject(url, User.class);
        return CompletableFuture.completedFuture(user);
    }
    
    @Async
    public CompletableFuture<List<User>> getUsersAsync() {
        String url = "http://localhost:8080/api/users";
        User[] users = restTemplate.getForObject(url, User[].class);
        return CompletableFuture.completedFuture(Arrays.asList(users));
    }
}
```

#### Перехватчики (Interceptors)

```java
@Component
public class LoggingInterceptor implements ClientHttpRequestInterceptor {
    
    private static final Logger logger = LoggerFactory.getLogger(LoggingInterceptor.class);
    
    @Override
    public ClientHttpResponse intercept(
            HttpRequest request, 
            byte[] body, 
            ClientHttpRequestExecution execution) throws IOException {
        
        logger.info("Request: {} {}", request.getMethod(), request.getURI());
        logger.info("Headers: {}", request.getHeaders());
        
        ClientHttpResponse response = execution.execute(request, body);
        
        logger.info("Response: {} {}", response.getStatusCode(), response.getStatusText());
        logger.info("Response Headers: {}", response.getHeaders());
        
        return response;
    }
}
```

#### Конфигурация с перехватчиками

```java
@Configuration
public class RestTemplateConfig {
    
    @Bean
    public RestTemplate restTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        
        // Добавляем перехватчики
        List<ClientHttpRequestInterceptor> interceptors = List.of(
            new LoggingInterceptor(),
            new AuthInterceptor(),
            new RetryInterceptor()
        );
        restTemplate.setInterceptors(interceptors);
        
        return restTemplate;
    }
}
```

#### Альтернативы RestTemplate

**WebClient (рекомендуемый для новых проектов):**
```java
@Service
public class UserService {
    
    @Autowired
    private WebClient webClient;
    
    public Mono<User> getUser(Long id) {
        return webClient
            .get()
            .uri("/api/users/{id}", id)
            .retrieve()
            .bodyToMono(User.class);
    }
    
    public Flux<User> getUsers() {
        return webClient
            .get()
            .uri("/api/users")
            .retrieve()
            .bodyToFlux(User.class);
    }
}
```

#### Преимущества и недостатки RestTemplate:

**Преимущества:**
- Простота использования
- Синхронный API
- Хорошая интеграция со Spring
- Поддержка различных форматов данных

**Недостатки:**
- Синхронный (блокирующий)
- Устаревший (deprecated в Spring 5)
- Нет поддержки реактивного программирования

**Рекомендации:**
- Для новых проектов используйте WebClient
- RestTemplate подходит для простых синхронных задач
- WebClient лучше для высоконагруженных приложений

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
