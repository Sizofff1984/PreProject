# 🌱 Spring Framework - Расширенные вопросы для собеседований

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Spring](https://img.shields.io/badge/Spring-6.x-green.svg)](https://spring.io/)
[![Java](https://img.shields.io/badge/Java-8%2B-orange.svg)](https://www.oracle.com/java/)

Расширенный интерактивный список из 34 контрольных вопросов по Spring Framework с подробными ответами, примерами кода и практическими рекомендациями для подготовки к собеседованиям.

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
**Principal** - представляет текущего аутентифицированного пользователя.

```java
@RestController
public class UserController {
    @GetMapping("/profile")
    public String getProfile(Principal principal) {
        return "Hello, " + principal.getName();
    }
    
    // Или через Authentication
    @GetMapping("/user-info")
    public String getUserInfo(Authentication authentication) {
        return "User: " + authentication.getName();
    }
}
```

#### Authorities
**Authorities** - коллекция прав/ролей пользователя.

```java
@Service
public class CustomUserDetailsService implements UserDetailsService {
    @Override
    public UserDetails loadUserByUsername(String username) {
        User user = userRepository.findByUsername(username);
        
        List<GrantedAuthority> authorities = user.getRoles().stream()
            .map(role -> new SimpleGrantedAuthority("ROLE_" + role.getName()))
            .collect(Collectors.toList());
        
        return new org.springframework.security.core.userdetails.User(
            user.getUsername(), 
            user.getPassword(), 
            authorities
        );
    }
}
```

#### Authentication
**Authentication** - объект, содержащий информацию об аутентификации.

```java
@Component
public class SecurityUtils {
    public String getCurrentUsername() {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        if (authentication != null && authentication.isAuthenticated()) {
            return authentication.getName();
        }
        return null;
    }
    
    public boolean hasRole(String role) {
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        return authentication.getAuthorities().stream()
            .anyMatch(authority -> authority.getAuthority().equals("ROLE_" + role));
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 23. Чем отличается InMemoryAuthentication от BasicAuthentication?

#### InMemoryAuthentication
**InMemoryAuthentication** - способ хранения пользователей в памяти приложения.

```java
@Configuration
@EnableWebSecurity
public class InMemorySecurityConfig {
    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails admin = User.builder()
            .username("admin")
            .password(passwordEncoder().encode("admin123"))
            .roles("ADMIN")
            .build();
            
        UserDetails user = User.builder()
            .username("user")
            .password(passwordEncoder().encode("user123"))
            .roles("USER")
            .build();
            
        return new InMemoryUserDetailsManager(admin, user);
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

#### BasicAuthentication
**BasicAuthentication** - способ аутентификации через HTTP Basic Auth.

```java
@Configuration
@EnableWebSecurity
public class BasicAuthConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults()) // Включаем Basic Auth
            .csrf(csrf -> csrf.disable());
            
        return http.build();
    }
}
```

**Ключевые различия:**
- **InMemoryAuthentication** - где хранятся пользователи (в памяти)
- **BasicAuthentication** - как происходит аутентификация (HTTP Basic)

**Можно комбинировать:**
```java
@Configuration
public class CombinedConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz.anyRequest().authenticated())
            .httpBasic(Customizer.withDefaults()) // Basic Auth
            .userDetailsService(inMemoryUserDetailsManager()); // InMemory users
            
        return http.build();
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

### 24. Как добавить секьюрность к контроллеру?

#### Способ 1: Аннотации безопасности

```java
@RestController
@RequestMapping("/api/users")
@PreAuthorize("hasRole('ADMIN')") // На весь контроллер
public class UserController {
    
    @GetMapping
    @PreAuthorize("hasRole('USER')") // На конкретный метод
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @PostMapping
    @PreAuthorize("hasRole('ADMIN') or #user.username == authentication.name")
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
    
    @DeleteMapping("/{id}")
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

#### Способ 2: Конфигурация SecurityFilterChain

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/users/**").hasRole("ADMIN")
                .requestMatchers("/api/profile/**").hasRole("USER")
                .requestMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            )
            .formLogin(form -> form
                .loginPage("/login")
                .permitAll()
            )
            .logout(logout -> logout
                .logoutSuccessUrl("/")
            );
            
        return http.build();
    }
}
```

#### Способ 3: Method Security

```java
@Configuration
@EnableMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig {
    // Конфигурация
}

@Service
public class UserService {
    @PreAuthorize("hasRole('ADMIN')")
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
    
    @PostAuthorize("returnObject.username == authentication.name or hasRole('ADMIN')")
    public User findById(Long id) {
        return userRepository.findById(id);
    }
}
```

#### Способ 4: Программная проверка

```java
@RestController
public class SecureController {
    
    @GetMapping("/secure-data")
    public ResponseEntity<?> getSecureData(Authentication authentication) {
        // Программная проверка прав
        if (!hasRequiredRole(authentication)) {
            return ResponseEntity.status(HttpStatus.FORBIDDEN).build();
        }
        
        return ResponseEntity.ok(secureService.getData());
    }
    
    private boolean hasRequiredRole(Authentication auth) {
        return auth.getAuthorities().stream()
            .anyMatch(authority -> authority.getAuthority().equals("ROLE_ADMIN"));
    }
}
```

[⬆️ Вернуться к списку вопросов](#-содержание)

---

## 🎯 Заключение

Этот расширенный список содержит 34 вопроса по Spring Framework, включая Spring Security, REST-сервисы, работу с базами данных и веб-технологии. Каждый вопрос содержит подробный ответ с примерами кода и практическими рекомендациями.

**Удачи на собеседовании! 🚀**
