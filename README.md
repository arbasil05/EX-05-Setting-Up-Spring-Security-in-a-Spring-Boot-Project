# EXP05 - Setting Up Spring Security in a Spring Boot Project

## AIM

To write a program for setting up Spring Security in a Spring Boot project to secure endpoints with Basic Authentication and role-based access control.

## ALGORITHM

1. Create a Spring Boot Project with the following dependencies:

   * Spring Web
   * Spring Security
   * Spring Boot DevTools (optional)

2. Add the Spring Security dependency in `pom.xml` if not using Spring Initializr.

3. Create a configuration class using `SecurityFilterChain` for newer Spring Boot versions.

4. Define an in-memory user with username, password, and roles using `UserDetailsService`.

5. Secure the REST endpoints using annotations or the security configuration class.

6. Run and test the application using a browser or Postman.

7. Verify that secured endpoints prompt for username and password.

## PROGRAM CODE

### pom.xml (Dependencies)

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
</dependencies>
```

### SecurityConfig.java (Spring Boot 3.x / Spring Security 6+)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic(httpBasic -> {});

        return http.build();
    }

    @Bean
    public InMemoryUserDetailsManager userDetailsService() {
        UserDetails user = User.withDefaultPasswordEncoder()
            .username("user")
            .password("password")
            .roles("USER")
            .build();

        return new InMemoryUserDetailsManager(user);
    }
}
```

### HelloController.java

```java
@RestController
public class HelloController {

    @GetMapping("/public")
    public String publicEndpoint() {
        return "This is a public endpoint.";
    }

    @GetMapping("/private")
    public String privateEndpoint() {
        return "This is a secured endpoint. You are authenticated!";
    }
}
```

## RESULT

The Spring Boot application was successfully configured with Spring Security to secure endpoints using Basic Authentication and role-based access control.
