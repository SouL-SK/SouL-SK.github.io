# Spring Annotation

> ## Contents
> - @Entity
> - @Repository
> - @Service
> - @Controller
## Entity and Repository

The `@Entity` annotation is used to mark a Java class as an entity, which represents a persistent object in the database. It is used in combination with JPA annotations, such as `@Id`, `@Column`, and `@OneToMany`, to define the fields and relationships of the entity.

The `@Repository` annotation is used to mark a Java interface as a repository, which provides a way to access the data store and perform CRUD (Create, Read, Update, Delete) operations on the entity. It is typically used in combination with the `JpaRepository` interface, which provides a set of methods for querying and manipulating the entity.

```java
@Entity
public class User {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;
  @Column(name = "username", nullable = false)
  private String username;
  @Column(name = "password", nullable = false)
  private String password;
  // Getters and setters
}
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
  // Custom repository methods
}
```

In this example, the `User` entity class is annotated with `@Entity`, and the `UserRepository` interface is annotated with `@Repository`. The `UserRepository` interface extends the `JpaRepository` interface, which provides a set of methods for querying and manipulating the `User` entity.

Overall, the relationship between an entity and a repository in JPA is defined by the `@Entity` and `@Repository` annotations, which mark the entity class and repository interface, respectively. The repository interface typically extends the `JpaRepository` interface to provide a set of methods for querying and manipulating the entity.

## Service and Controller

The `@Service` annotation is used to mark a Java class as a service layer component, which performs business logic and interacts with the DAO (Data Access Object) layer to retrieve and store data. The `@Service` annotation can be used in combination with the `@Autowired` annotation to inject dependencies into the service class.

The `@Controller` annotation is used to mark a Java class as a controller, which is responsible for handling HTTP requests and returning the appropriate response. The `@Controller` annotation can be used in combination with the `@RequestMapping` annotation to map HTTP requests to specific methods in the controller class.

```java
@Service
public class UserServiceImpl implements UserService {
  @Autowired
  private UserRepository userRepository;
  // Service methods
}
@Controller
@RequestMapping("/users")
public class UserController {
  @Autowired
  private UserService userService;
  @GetMapping
  public String getUsers(Model model) {
    model.addAttribute("users", userService.getAllUsers());
    return "users";
  }
  // Other controller methods
}
```

In this example, the `UserServiceImpl` class is annotated with `@Service`, and the `UserController` class is annotated with `@Controller`. The `UserController` class uses the `@Autowired` annotation to inject an instance of the `UserService` into the controller, and it uses the `@RequestMapping` annotation to map HTTP `GET` requests to the `getUsers` method. The `getUsers` method uses the `userService` to retrieve all users and add them to the model, which is then rendered by the `users` view.

Overall, the relationship between a service and a controller in the Spring Framework is defined by the `@Service` and `@Controller` annotations, which mark the service class and controller class, respectively. The controller class can use the `@Autowired` annotation to inject an instance of the service class and the `@RequestMapping` annotation to map HTTP requests to specific methods in the controller.