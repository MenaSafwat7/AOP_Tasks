# Simple AOP labs

Each folder is an independent Maven project. Use Java 17 or newer.
The Spring versions match the PDFs; no Spring Boot, database, or web server is needed.

| Folder | Exercise | Start here |
| --- | --- | --- |
| AOP_Proxy | Session 1, Lab 1: Java dynamic proxy | AOP_Proxy/README.md |
| AOP_interfaces | Session 1, Lab 2: Spring advice interfaces | AOP_interfaces/README.md |
| AOP_Annotations | Session 2, Lab 2: advice annotations and custom caching | AOP_Annotations/README.md |

Study them in that order. The common idea is:

```text
Main -> proxy -> extra behavior -> real service
```

Open a folder's `pom.xml` as a Maven project in your IDE, let dependencies load,
then run its `Main.main()` method under `src/main/java`.
Alternatively, with Maven installed and available in your terminal, run from this directory:

```shell
mvn -q -f AOP_Proxy/pom.xml compile exec:java
mvn -q -f AOP_interfaces/pom.xml compile exec:java
mvn -q -f AOP_Annotations/pom.xml compile exec:java
```

Each Main demonstrates the required behavior. Intentional service exceptions are caught
so the remaining examples can run. Console messages show the order of execution.
