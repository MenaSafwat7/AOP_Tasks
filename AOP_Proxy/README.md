# Session 1 - Lab 1: JDK dynamic proxy

This lab uses plain Java and has no dependencies. Notifications are printed, not sent.

## Read in this order

1. `NotificationService`: the two methods the caller can use.
2. `NotificationServiceImpl`: the real work (printing an email or SMS).
3. `LoggingHandler`: prints the name and arguments, calls the real method,
   prints the return value, and measures the elapsed time.
4. `Main`: creates the proxy and calls both methods through it.

All Java files are in `src/main/java/com/example/proxy`.

## Run

Open this folder as a Maven project and run `Main`, or run here:

```shell
mvn -q compile exec:java
```

For each notification, expect this order:

```text
BEFORE: sendEmail args=[student@example.com, Hello from Java!]
Sending email to student@example.com: Hello from Java!
RETURN: null
TIME: ... ms
```

`null` is normal: these methods return `void`, so reflection has no value to return.
`finally` measures time even if a call fails. The handler unwraps reflection's
exception wrapper so callers receive the original service exception.

Try calling `target.sendEmail(...)` instead: it prints the email without the handler's logs.
