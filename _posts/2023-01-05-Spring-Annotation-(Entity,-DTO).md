# Spring annotation (Entity, DTO)

An entity is a Java class that represents a database table or view. It is used to map the table or view to a Java object, and it is typically annotated with the `@Entity` annotation. Here is an example of an entity class:

```
@Entity
@Table(name = "users")
public class User {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private Long id;

  @Column(name = "username", unique = true)
  private String username;

  @Column(name = "password")
  private String password;

  // Other entity fields and methods

}

```

In this example, the `User` class is annotated with `@Entity`, which marks it as an entity class. The `@Table` annotation specifies the name of the database table, and the `@Id`, `@GeneratedValue`, and `@Column` annotations specify the primary key, the generation strategy, and the column names for the entity fields.

A DTO is a Java class that is used to transfer data between layers of an application. It typically does not have any annotations, and it does not have any relationship with the database. Here is an example of a DTO class:

```
public class UserDTO {

  private Long id;
  private String username;
  private String password;

  // Constructors, getters, and setters

}

```

In this example, the `UserDTO` class is a plain Java class that has fields for the user ID, username, and password. It does not have any annotations, and it does not have any relationship with the database.

A repository is a Java interface that is used to manage data in a database. It typically extends the `JpaRepository` interface, and it is used to perform CRUD (create, read, update, delete) operations on the entity. Here is an example of a repository interface:

```
public interface UserRepository extends JpaRepository<User, Long> {

  User findByUsername(String username);

}

```

In this example, the `UserRepository` interface extends the `JpaRepository` interface, and it specifies the `User` entity and the primary key type (`Long`) as its generic parameters. The `findByUsername` method is a custom query that returns a user based on the username.

To use entities, DTOs, and repositories in your application, you need to create the entity, DTO, and repository classes, and configure the Spring container to scan and manage them. You can then use the repository to perform CRUD operations on the entity, and you can use the DTO to transfer data