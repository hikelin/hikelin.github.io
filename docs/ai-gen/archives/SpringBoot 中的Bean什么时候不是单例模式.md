<think>
嗯，我现在在学习Spring Boot，特别是关于Bean的作用范围。我知道默认情况下，Spring中的Bean都是单例模式，也就是说每次注入的时候都会得到同一个实例。但是用户的问题问的是，在什么情况下Bean不是单例的。这个问题我需要仔细思考一下。

首先，我应该回忆一下Spring中的Bean作用范围有哪些。我记得常见的有Singleton（单例）、Prototype（原型）、Request、Session等。默认的确实是Singleton，所以当显式地将Bean定义为其他作用范围时，它就不是单例了。这可能是一个主要的情况。

接下来，我想起来在Spring Boot中，可以通过注解来配置作用范围，比如@Scope("prototype")或者使用@RequestScope和@SessionScope。如果用户在定义Bean的时候添加了这些注解，那么Bean就会是多例的，而不是单例。所以，这种情况下，Bean就不会是单例模式。

然后，我还记得可以通过XML配置文件来设置Bean的作用范围。在XML中，可以使用<bean>标签的scope属性来指定，比如<bean id="myBean" class="com.example.MyBean" scope="prototype"/>。这种情况下，Bean也不是单例。

另外，还有一种情况是，如果用户自定义了一个FactoryBean，那么它可能返回的是原型模式的Bean。这是因为FactoryBean通常用于创建特定的Bean实例，而这些实例的生命周期可能与Singleton不同。不过，这可能比较少见，除非有特别的需求才会用到。

接下来，我想到了作用域代理。如果在定义Bean的时候使用了Scoped-proxy注解，比如@Scope("request")和proxyMode=TARGET_CLASS，那么Spring会为这个Bean创建一个代理对象。这样，当每次注入这个Bean时，实际上获取的是不同作用域中的实例，而不是同一个单例。这也是一种情况。

还有，如果使用第三方框架或者库，它们可能自定义了Bean的作用范围，这样即使在Spring Boot中，这些Bean也可能是非单例模式。比如有些库会默认以原型模式创建Bean，所以需要注意这种情况。

然后，我想到了一些特殊的Bean，比如控制器或者处理请求的Bean，通常它们的生命周期与HTTP请求或会话相关，因此自动被配置为RequestScope或SessionScope，这也导致它们不是单例。

还有一种情况是，如果在定义Bean的时候使用了@Lookup注解，这会让Spring在每次注入时查找新的实例，而不是返回同一个单例。这可能是一种比较高级的用法，但确实存在这种情况。

最后，通过编程方式配置Bean的时候，可以设置作用范围。比如在@Configuration类中使用@Scope注解，或者在@Bean方法上直接指定scope属性，这样生成的Bean就不会是单例了。

综上所述，我总结出Spring Boot中的Bean不是单例的情况主要有以下几种：显式定义为Prototype、Request、Session等作用范围；通过XML配置文件设置不同的作用范围；使用FactoryBean自定义实例；应用Scoped-proxy创建代理对象；以及一些第三方库的默认行为。理解这些情况可以帮助我更好地控制Spring中的Bean生命周期，灵活应对不同需求。
</think>

在 Spring Boot 中，默认情况下 Bean 是单例模式，但以下情况会使其不是单例：

1. **显式定义为Prototype或其他范围**：
   - 使用 `@Scope("prototype")` 注解定义 Bean，每次请求都会创建新实例。

2. **基于Request或Session的作用域**：
   - 用 `@RequestScope` 或 `@SessionScope` 定义，生命周期绑定到 HTTP 请求或会话。

3. **通过XML配置文件设置作用域**：
   - 在 `<bean>` 标签中设置 `scope="prototype"` 等属性。

4. **使用FactoryBean创建原型Bean**：
   - 自定义 FactoryBean 返回原型模式的 Bean 实例。

5. **应用Scoped-proxy机制**：
   - 配置 `proxyMode` 为代理，以便在不同作用域中返回正确实例。

6. **第三方框架或库的自定义配置**：
   - 某些库可能默认创建非单例Bean，需关注其配置。

7. **特殊Bean如控制器**：
   - 控制器通常由框架管理，其生命周期可能与请求相关，不是单例。

8. **使用@Lookup注解动态查找**：
   - 每次注入时返回新实例，而非同一实例。

通过以上方法，可以在Spring Boot中控制Bean的作用范围，避免默认的单例模式。