# Spring annotation (RequiredArgsConstructors, Autowired)

## RequiredArgsConstructor Annotation

---

The `@RequiredArgsConstructor`
 annotation is a part of the Project Lombok library, which is a tool that can generate code for you at compile time. The `@RequiredArgsConstructor`
 annotation generates a constructor for a class that takes all of the final fields and fields annotated with `@NonNull`
 as arguments.

```java
@RequiredArgsConstructor
public class User {

  private final Long id;

  @NonNull
  private String username;

  @NonNull
  private String password;

  // Getters and setters

}
```

In this example, the `@RequiredArgsConstructor` annotation generates a constructor for the `User` class that takes the `id` field and the `username` and `password` fields as arguments. The `id` field is marked as final, and the `username` and `password` fields are annotated with `@NonNull`, which indicates that they cannot be `null`.

The `@RequiredArgsConstructor` annotation generates a constructor that takes all of the final fields and fields annotated with `@NonNull` as arguments, and it also generates a `NoArgsConstructor`, which is a constructor that takes no arguments.

You can use the `@RequiredArgsConstructor` annotation to generate a constructor that takes all of the final fields and fields annotated with `@NonNull` as arguments, which can be convenient when you have a large number of fields and you want to ensure that all required fields are initialized.

Overall, the `@RequiredArgsConstructor` annotation is a part of the Project Lombok library that generates a constructor for a class that takes all of the final fields and fields annotated with `@NonNull` as arguments. It can be convenient when you have a large number of fields and you want to ensure that all required fields are initialized.

## Autowired Annotation

---

The `@Autowired`
 annotation is a part of the Spring Framework, which is a popular framework for developing Java applications. The `@Autowired`
 annotation is used to inject dependencies into a class, such as service objects, repository objects, and other components.

```java
@Service
public class UserServiceImpl implements UserService {

  @Autowired
  private UserRepository userRepository;

  @Autowired
  private AuditService auditService;

  public User createUser(User user) {
    auditService.log("Creating user: " + user.getUsername());
    return userRepository.save(user);
  }

  // Other service methods

}
```

In this example, the `UserServiceImpl` class is annotated with `@Service`, which marks it as a service layer component. The `UserRepository` and `AuditService` objects are injected into the service class using the `@Autowired` annotation. The `createUser` method uses the `auditService` to log the action and the `userRepository` to save the user to the database.

You can also use the `@Inject` annotation instead of `@Autowired` to inject dependencies into a class. Both annotations have the same effect, but `@Inject` is a part of the Java Dependency Injection (JSR-330) specification, whereas `@Autowired` is a part of the Spring Framework.

Overall, the `@Autowired` annotation is used to inject dependencies into a class in the Spring Framework. It allows you to access objects that are managed by the Spring container, such as service objects, repository objects, and other components.

## Difference of RequiredArgsConstructor and Autowired

---

The `@RequiredArgsConstructor` and `@Autowired` annotations are used for different purposes in the Java programming language.

The `@RequiredArgsConstructor` annotation is a part of the Project Lombok library, which is a tool that can generate code for you at compile time. The `@RequiredArgsConstructor` annotation generates a constructor for a class that takes all of the final fields and fields annotated with `@NonNull` as arguments.

On the other hand, the `@Autowired` annotation is a part of the Spring Framework, which is a popular framework for developing Java applications. The `@Autowired` annotation is used to inject dependencies into a class, such as service objects, repository objects, and other components.

```java
@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

  private final UserRepository userRepository;

  @Autowired
  private AuditService auditService;

  public User createUser(User user) {
    auditService.log("Creating user: " + user.getUsername());
    return userRepository.save(user);
  }

  // Other service methods

}
```

In this example, the `UserServiceImpl` class is annotated with `@Service`, which marks it as a service layer component, and with `@RequiredArgsConstructor`, which generates a constructor that takes the `userRepository` field as an argument. The `AuditService` object is injected into the service class using the `@Autowired` annotation.

Overall, the `@RequiredArgsConstructor` annotation is used to generate a constructor that takes all of the final fields and fields annotated with `@NonNull` as arguments, while the `@Autowired` annotation is used to inject dependencies into a class. These annotations are used for different purposes and are not interchangeable.