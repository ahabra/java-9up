# Overview of Java >= 9
    Written by: Abdul Habra
    Last Update: 2020.02.13


## Laws Of Meeting
1. Law of Two Feet
2. Law of Big Mouth
3. Law of Equal Participation (a.k.a Uniform Distribution)

## SdkMan
Tool to manage parallel versions of SDKs (Java and others). Same idea as jEnv, but better :)

### Useful commands:

* sdk help
* sdk list java                  # List all available verions
* sdk current java               # Show what's currently in use
* sdk install java 8.0.202-zulu
* sdk install java 13.0.2-zulu
* sdk use java 13.0.2-zulu       # For current session of shell
* sdk default java 13.0.2-zulu   # Subsequent shells will use
* sdk uninstall java 13.0.2-zulu


## Java Versions

| Version | Version Scheme | Release | Oracle Support |
| ------- | -------------- | ------- | -------------- |
| 8       | 1.8.0_202      | 2014/3  | 12/2020        |
| 9       | 9.x.y          | 2017/9  | to next        |
| 10      | 10.x.y         | 2018/3  | to next        |
| 11      | 11.x.y         | 2018/9  | 9/2023 LTS     |
| 12      | 12.x.y         | 2019/3  | to next        |
| 13      | 13.x.y         | 2019/9  | to next        |
| 14      | 14.x.y         | 2020/3? | to next        |
| 15      | 15.x.y         | 2020/9? | to next        |


* LTS: Long-Term-Support starting with 11
* LTS every 3 years
* Non-LTS every 6 months
* Version scheme is NOT semantic versioning: $FEATURE.$INTERIM.$UPDATE.$PATCH


## Interesting Features
This is a selection based on the author's views 😃

### Improved Java Docs - Java 9
Include search box:
https://docs.oracle.com/javase/9/docs/api/index.html?overview-summary.html

### Jshell - Java 9
* REPL: Read Evaluate Print Loop
```java
/help
/exit
int a=42
System.out.println(a)
```
Use tab key for auto-colmplete

### JCMD Utility - Java 9
Sends diagnostic command requests to a running Java Virtual Machine (JVM).

* `jcmd` : List running VMs and their PIDs
* `jcmd PID help` : help for selected PID
* `jcmd PID GC.class_histogram` : List classes for given PID

### Run Java File - Java 11
Suppose we have this class:
```java
public class Hello{
    public static void main(String[] args){
        System.out.println("Hello 42");
    }
}
```
You can:
```
java Hello.java
```
No need for running `javac`

* You can have multiple classes in a single file


#### Java Shebang Files - Java 11
Create a file named for example `shebang`
```java
#!/path/to/bin/java --source 11

public class Foo {
    public static void main(String[] args) {
        System.out.println("Java Says: Hello " + System.getProperty("user.name"));
    }
}
```
Remember to `chmod +x shebang`

### Collections Factory Methods - Java 9
```java
List<Integer> list = List.of(1, 2);
Set<Integer> set = Set.of(1, 2); // elements must be unique
Map<Integer, String> map1 = Map.of(1, "a", 2, "b");
Map<Integer, String> map2 = Map.ofEntries(Map.entry(1, "a"), Map.entry(2, "b"));
Map.Entry<Integer, String> entry = Map.entry(1, "a");
```

### Interface Private Methods - Java 9

### Local Var Type Inference - Java 10
```java
var x = "a";
System.out.println(x.getClass());
```
Will print: `class java.lang.String`

* The `var` must be initialized to a non-null value, and must be local.

### Switch Expressions - Java 12 Preview, 14 Standard
This is a _preview_ feature. Use `--enable-preview` at command line to enable.

```java
int daysInMonth = switch(month) {
    case 2 -> 28;
    case 1, 3, 5, 7, 8, 10, 12 -> 31;
    case 4, 6, 9, 11 -> 30;
    default -> throw new IllegalArgumentException("Invalid value of month: " + month);
};
```

### Text Blocks - Java 13 Preview
Use `--enable-preview` at command line to enable.

* Multiline string inside a triple-double quotes:

```java
String html = """
<div>
  This is java's Multiline text block
</div>
""";
```

### Records - Java 14 Preview
Similar to Scala's Case Class.

```java
// java 14
record Book(String title, String author) {}
```
Is equivilant to

```java
// pre java 14
final class Book {
    public final String title;
    public final String author;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    // Plus equals, hashCode, toString
}
```

Finally, no need to Lombok or get/set code generators.

### Reactive Streams - Java 9
Concurrent, distributed, asynchronous, unbounded, reactive streams.

* `java.util.concurrent.Flow`: Has the following `static` inner `interface`s
    * `Subscriber`: Informs Publisher that it can accept N number of items
    * `Publisher`: If it has items, will push max of N items to Subscriber
    * `Processor`: Both Publisher and Subscriber
    * `Subscription`: Binds a single publisher with a single subscriber


* This process prevents overloading of subscriber. It is called __backpressure__
* Relationship from `Publisher` to `Subscriber` is 1-to-N
* During subscription, Publisher passes a `Subscription` object to Subscriber



### HTTP Client - Incubated in Java 9, standardized in Java 11
* Support http/1.1 and 2. Supports WebSockets
* java.net.http.Http
* Uses fluent API

```java
import java.net.http.*;
import java.net.http.HttpResponse.BodyHandlers;

HttpRequest request = HttpRequest.newBuilder()
      .uri(URI.create("http://example.com/")).build();

HttpClient client = HttpClient.newHttpClient();
client.sendAsync(request, BodyHandlers.ofString())
      .thenApply(HttpResponse::body)
      .thenAccept(System.out::println)
      .join();
```
Will print content of page.


### Module System / Jigsaw Project - Java 9
* Control JDK modules
* Somewhat similar to OSGi
* imho: Revenge of the EJB architects.


### Epsilon Garbage Collector - Java 11
* Allocates memory on heap, but no reclamaion.
* Experimental
* Useful for performance testing


### Depracations
* Applet API - Java 9
* Nashorn JS Engine - Java 11

### Removals
* Java EE and CORBA Modules - Java 11
    * java.xml.ws
    * java.xml.bind
    * java.activation
    * java.xml.ws.annotation
    * java.corba
    * java.transaction
    * java.se.ee
    * jdk.xml.ws
    * jdk.xml.bind
* Thread.destroy() and Thread.stop(Throwable) - Java 11
* Java Plugin and Java WebStart - Java 11
* JavaFX - Java 11



## References
1. https://www.oracle.com/technetwork/java/java-se-support-roadmap.html 
2. https://www.baeldung.com/java-time-based-releases
3. https://www.baeldung.com/new-java-9
4. https://www.baeldung.com/java-10-overview
5. https://www.baeldung.com/java-9-http-client
6. https://www.baeldung.com/java-single-file-source-code
7. https://dzone.com/articles/39-new-features-and-apis-in-jdk-12
8. https://dzone.com/articles/reactive-streams-in-java-9

