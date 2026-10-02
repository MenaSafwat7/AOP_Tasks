# Session 2 - Lab 2: Practice every advice annotation

The PDF leaves the business service open, so this example uses a tiny product lookup.
`getProduct("1")` returns `Product 1`; `getProduct("bad")` throws an exception.

## Read in this order

1. `ProductService`: the real method.
2. `CoreAdviceAspect`: four separate advice annotations and a reusable `@Pointcut`.
3. `AroundAdviceAspect`: replaces those four advice methods with one method.
4. `Cacheable` and `CachingAspect`: a custom annotation and a small in-memory cache.
5. `AppConfig` and `Main`: enable Spring AOP and run the examples.

All Java files are in `src/main/java/com/example/annotations`.

| Annotation | Meaning in this lab |
| --- | --- |
| @Aspect | This class holds advice |
| @Pointcut | Give the method-matching expression a reusable name |
| @Before | Log before the service runs |
| @AfterReturning | Log the successful result |
| @AfterThrowing | Log the exception |
| @After | Log after either success or failure |
| @Around | Wrap the call; use proceed() to run the real method |
| @Cacheable | Our custom marker asking the caching aspect to remember the result |

`execution(* com.example.annotations.ProductService.*(..))` matches any return type,
any method on ProductService, and any arguments.
`@annotation(com.example.annotations.Cacheable)` matches methods carrying our marker.

## Run

Open this folder as a Maven project and run `Main`, or run here:

```shell
mvn -q compile exec:java
```

Main runs three independent Spring contexts. A `@Profile` is simply a switch choosing
which aspect to register. `@Bean` registers the service and aspect objects, and
`@EnableAspectJAutoProxy` enables automatic proxy creation.

1. **core**: the four separate advice annotations run. Success prints
   BEFORE -> TARGET -> SUCCESS -> AFTER. Failure prints
   BEFORE -> TARGET -> ERROR -> AFTER -> CALLER caught.
2. **around**: only AroundAdviceAspect is enabled. It produces the same behavior
   using try/catch/finally. It replaces the four advice methods rather than doubling their logs.
3. **cache**: only CachingAspect is enabled. ID 1 first prints CACHE MISS and TARGET;
   the second lookup prints CACHE HIT without TARGET. ID 2 is a separate miss.
   Two failed lookups of `bad` both miss because failures are not cached.

Our `@Cacheable` is separate from Spring's built-in annotation. The map and string key
are intentionally small examples for this single-threaded demo with String IDs;
there is no expiry or persistent storage.

Always call the bean obtained from the context. Calling `new ProductService()` or
calling another method through `this` inside the service bypasses Spring's proxy.
