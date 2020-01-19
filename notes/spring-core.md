## spring-core

### 1. The IoC Container

#### 1.3 Bean Overview

##### 1.3.2 实例化Bean

* 通过构造方法

 ```xml
<bean id="exampleBean" class="examples.ExampleBean"/>
    
<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
 ```

* 通过静态工厂方法

 ```xml
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>    
 ```

```java
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```


* 通过实例工厂方法

```xml
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```java
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```

一个工厂类也可以包含多于一个的工厂方法，即同一个factory-bean可以用于多个bean的配置

#### 1.4 Dependencies 依赖

##### 1.4.1 Dependency Injection 依赖注入

依赖项注入(DI)是一个过程，在这个过程中，对象仅通过构造函数参数、工厂方法的参数或从工厂方法构造或返回后在对象实例上设置的属性来定义它们的依赖项(即它们与之一起工作的其他对象)。然后容器在创建bean时注入这些依赖项。这个过程基本上是bean本身的逆过程(因此称为控制反转)，通过直接构造类或服务定位器模式来控制依赖项的实例化或位置

通过DI代码更加简洁，并且实现了解耦，仅依赖于抽象类或接口，便于在单元测试中使用stub和mock

##### Constructor-based Dependency Injection

```java
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...
}
```

