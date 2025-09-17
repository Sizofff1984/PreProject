# 🌱 Spring Framework - Контрольные вопросы для собеседований

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Spring](https://img.shields.io/badge/Spring-6.x-green.svg)](https://spring.io/)
[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.oracle.com/java/)

Интерактивный список из 20 контрольных вопросов по Spring Framework с подробными ответами, примерами кода и практическими рекомендациями для подготовки к собеседованиям.

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

## 🎯 Заключение

Этот интерактивный список поможет вам эффективно подготовиться к собеседованию по Spring Framework. Каждый вопрос содержит подробный ответ с примерами кода и практическими рекомендациями.

**Удачи на собеседовании! 🚀**
