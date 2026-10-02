# Session 1 - Lab 2: Spring advice interfaces

The service returns a fixed stock of 100. Reserving more than 100 throws an exception.
This is a small simulation; reservations do not update stored inventory.

## Read in this order

1. `InventoryService` and `InventoryServiceImpl`: the business methods.
2. The four advice classes in the table below.
3. `Main`: builds a proxy manually with `ProxyFactory`.
4. `AppConfig`: builds the same proxy as a Spring bean with `ProxyFactoryBean`.

All Java files are in `src/main/java/com/example/interfaces`.

| Class | Interface | Job |
| --- | --- | --- |
| LoggingBeforeAdvice | MethodBeforeAdvice | Print method name and arguments |
| LoggingAfterReturningAdvice | AfterReturningAdvice | Print checkStock's successful result |
| LoggingThrowsAdvice | ThrowsAdvice | Log reserveStock's exception |
| TimingInterceptor | MethodInterceptor | Time each call; finally runs on success and failure |

`invocation.proceed()` continues to the next advice and eventually the real service.
Timing is attached first so it wraps the other advice.
`ThrowsAdvice` is a marker interface: Spring discovers `afterThrowing` by its signature.

## Run

Open this folder as a Maven project and run `Main`, or run here:

```shell
mvn -q compile exec:java
```

Main runs three examples:

1. Manual `ProxyFactory` with all four advice objects.
2. The same behavior using a Spring application context. The `inventoryService`
   bean is the proxy and is available for injection into other beans.
3. Bonus: `NameMatchMethodPointcut` restricts BEFORE logging to `reserveStock`.
   Other advice still applies, so `checkStock` still prints its result and timing.

For `checkStock`: AROUND start -> BEFORE -> TARGET -> SUCCESS -> FINALLY.
For a failed reservation: AROUND start -> BEFORE -> TARGET -> ERROR -> FINALLY -> CALLER caught.
For a successful reservation, no SUCCESS line appears because that advice logs only `checkStock`.

Try changing the reservation to 100 (success) and 101 (failure).
