# Circular Reference

## Causes of Circular Reference

---

A circular reference occurs when two or more objects depend on each other, resulting in a cycle of dependencies. In the context of an implemented class, a circular reference can occur if the implemented class and the interface it implements have a circular dependency.

## Example of Circular Reference

---

```java
public interface MyInterface {
  MyImplementedClass getImplementedClass();
}

public class MyImplementedClass implements MyInterface {
  private final MyInterface myInterface;

  public MyImplementedClass(MyInterface myInterface) {
    this.myInterface = myInterface;
  }

  @Override
  public MyImplementedClass getImplementedClass() {
    return this;
  }
}
```

In this example, the `MyInterface` interface has a method called `getImplementedClass` that returns an object of the `MyImplementedClass` class. The `MyImplementedClass` class implements the `MyInterface` interface and has a constructor injection for the `MyInterface` interface. The `getImplementedClass` method of the `MyImplementedClass` class returns `this`, which is an object of the `MyImplementedClass` class.

This creates a circular reference, because the `MyImplementedClass` class depends on the `MyInterface` interface, and the `MyInterface` interface depends on the `MyImplementedClass` class. This results in a cycle of dependencies, and it can cause problems when creating objects of the `MyImplementedClass` class.

## How to Fix Circular Reference

---

To fix a circular reference in an implemented class, you need to break the cycle of dependencies by injecting one of the dependencies using a different mechanism, such as setter injection or field injection. 

```java
public class MyImplementedClass implements MyInterface {
  private final MyInterface myInterface;

  @Autowired
  public MyImplementedClass(MyInterface myInterface) {
    this.myInterface = myInterface;
  }

  @Override
  public MyImplementedClass getImplementedClass() {
    return this;
  }
}
```

In this example, the `MyImplementedClass` class no longer depends on the `MyInterface` interface, because the `MyInterface` interface is injected using constructor injection. This breaks the circular reference, because the `MyImplementedClass` class no longer depends on the `MyInterface` interface, and the `MyInterface` interface does not depend on the `MyImplementedClass` class.

```java
public class MyImplementedClass implements MyInterface {

  @Override
  public MyImplementedClass getImplementedClass() {
    return this;
  }

	// you can use the methods in the MyInterface without object.
	private void someProcess(){
		// overrided methods.
	}
}
```

Overall, a circular reference in an implemented class occurs when the implemented class and the interface it implements have a circular dependency. To fix a circular reference, you need to break the cycle of dependencies by injecting one of the dependencies using a different mechanism, such as setter injection or field injection.