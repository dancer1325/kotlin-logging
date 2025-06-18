# [kotlin-logging](https://github.com/oshai/kotlin-logging) [![Slack channel](https://img.shields.io/badge/Chat-Slack-blue.svg)](https://kotlinlang.slack.com/messages/kotlin-logging/)

* == Kotlin's logging framework
  * lightweight
  * Multiplatform
  * written | [![Pure Kotlin](https://img.shields.io/badge/100%25-kotlin-blue.svg)](https://kotlinlang.org/)

## Overview

* kotlin-logging
  * == slf4j-api's wrapper
  * uses
    * Kotlin classes
  * allows
    * NO need to write the logger & class name
    * log messages with lazy-evaluated string -- via -- lambda expression `{}` -- TODO: ❓--
    * ALL previous slf4j implementation
      * still can be used
  * Reason to create it: 🧠EXISTING problems
    * [Idiomatic way of logging in Kotlin](http://stackoverflow.com/questions/34416869/idiomatic-way-of-logging-in-kotlin)
    * [Best practices for loggers](https://discuss.kotlinlang.org/t/best-practices-for-loggers/226/15)🧠

* [wiki](https://github.com/dancer1325/kotlin-logging/wiki)

### | IntelliJ
* | file level logger (TODO: ❓)
  - Text template: `private val logger = io.github.oshai.kotlinlogging.KotlinLogging.logger {}`
  - Applicable in `Kotlin: top-level`

## Features
### 👀NO need to check whether the log level is enabled👀
```Kotlin
logger.debug { "Some $expensive message!" }

// Reason: 🧠behind the scenes🧠
// if (logger.isDebugEnabled) logger.debug("Some $expensive message!")
```

### Define the logger / WITHOUT EXPLICITLY specifying the class name
```Kotlin
// 👀place ABOVE class declaration👀 -- Reason:🧠make field static🧠
private val logger = KotlinLogging.logger {}

// Reason: 🧠behind the scenes🧠
// val logger = LoggerFactory.getLogger("package.ClassName")
```

### Log exceptions / Kotlin-style
* Kotlin-style
  * == 
    * exception -- as -- first parameter
    * message -- as -- lambda

```Kotlin
logger.error(exception) { "a $fancy message about the $exception" }
```

### Fluent logging / Kotlin-style
```kotlin
logger.atWarn {
    message    = "foo $bar"
    cause      = exception
    payload    = buildMap(capacity = 3) {
        put("foo", 1)
        put("bar", "x")
        put("obj", Pair(2, 3))
    }
}
```

## Getting started
 
```Kotlin
import io.github.oshai.kotlinlogging.KotlinLogging

private val logger = KotlinLogging.logger {} 

class FooWithLogging {
    val message = "world"
    fun bar() {
        logger.debug { "hello $message" }
    }
}
```

## Download

* kotlin-logging
  * -- depends on -- 
    * slf4j-api
      * | JVM artifact
    * slf4j-api dependency
      * | v5+ -- TODO: is it fine ❓
    * logging implementation
      * | runtime
  * if you ONLY want to log statements | stdout -> add `org.slf4j:slf4j-simple:2.0.3`
    
* see 
  * [how-to-configure-slf4j](http://saltnlight5.blogspot.co.il/2013/08/how-to-configure-slf4j-with-different.html)
  * [a-guide-to-logging-in-java](https://www.marcobehler.com/guides/a-guide-to-logging-in-java)   


### Maven

```xml
<dependency>
  <groupId>io.github.oshai</groupId>
  <artifactId>kotlin-logging-jvm</artifactId>
  <version>7.0.3</version>
</dependency>
```

* _Example:_ [kotlin-logging-example-maven](https://github.com/microutils/kotlin-logging-example-maven)  

### Gradle
* ways
  * 
    ```Groovy
    implementation 'io.github.oshai:kotlin-logging-jvm:7.0.3'
    ```
  * download the JAR 
    * [here](https://github.com/oshai/kotlin-logging/releases/latest) OR
    * [here](https://repo1.maven.org/maven2/io/github/oshai/)

### Multiplatform

* experimental
* see
  * [wiki](https://github.com/oshai/kotlin-logging/wiki/Multiplatform-support)
  * [#21](https://github.com/oshai/kotlin-logging/issues/21)
  * [#45](https://github.com/oshai/kotlin-logging/issues/45)

## Version 5 vs. previous versions

* TODO: Version 5 is not backward compatible with previous versions (v.3, v.2, v.1). Group id (in maven) and packages names changed.
It is possible to use both version 5 and previous versions side-by-side so some of the code from the old version
and some new. It is also possible to have libs using old version and use the new version (and vice-versa).  
In that sense it's a completely new dependency.

Main changes are:
- Maven group id changed from `io.github.microutils` -> `io.github.oshai`.
- Root package change from `mu` -> `io.github.oshai.kotlinlogging`.
- Slf4j dependency is not provided anymore (users have to provide it). It means that >= 5.x can work with both slf4j 1 or 2.
- There are changes to multiplatform class hierarchy that might break compatibility.

More details in issue [#264](https://github.com/oshai/kotlin-logging/issues/264), 
and in the [change log](https://github.com/oshai/kotlin-logging/blob/master/ChangeLog.md)



## Who is using it

- https://www.jetbrains.com/youtrack/ (https://www.jetbrains.com/help/youtrack/standalone/Third-Party-Software-Used-by-YouTrack.html)
- https://github.com/DiUS/pact-jvm
- https://github.com/AsynkronIT/protoactor-kotlin
- https://github.com/square/misk
- https://github.com/openrndr/openrndr
- https://github.com/JetBrains/xodus

## FAQ

- Why not use plain slf4j? kotlin-logging has better native Kotlin support. It adds more functionality and enables less boilerplate code.
- Is all slf4j implementation supported (Markers, params, etc')? Yes.
- Is location logging possible? Yes, location awareness was added in kotlin-logging 1.4.
- When I do `logger.debug`, my IntelliJ IDEA run console doesn't show any output. Do you know how I could set the console logger to debug or trace levels? Is this an IDE setting, or can it be set in the call to KLogging()? Setting log level is done per implementation. kotlin-logging and slf4j are just facades for the underlying logging lib (log4j, logback etc') more details [here](http://stackoverflow.com/questions/43146977/how-to-configure-kotlin-logging-logger).
- Can I access the actual logger? In platforms available yes, via `DelegatingKLogger.underlyingLogger` property.

## Support

- Open an issue here: https://github.com/oshai/kotlin-logging/issues
- Ask a question in StackOverflow with [kotlin-logging tag](http://stackoverflow.com/tags/kotlin-logging/info).
- Chat on Slack channel: https://kotlinlang.slack.com/messages/kotlin-logging
- Chat on Telegram channel: https://t.me/klogging

## More links

- https://medium.com/@OhadShai/logging-in-kotlin-95a4e76388f2
- [kotlin-logging vs AnkoLogger for Android](https://medium.com/@OhadShai/logging-in-android-ankologger-vs-kotlin-logging-bb693671442a)
- [How kotlin-logging was published](https://medium.com/@OhadShai/no-forks-one-star-now-what-how-i-published-my-open-source-projects-8a5b5ae35d2c#.e3ygj6uf3)
- [Kotlin Logging Without the Fuss](https://realjenius.com/2017/08/31/logging-in-kotlin/)
- [DropWizard + Kotlin = Project Kronslott](https://medium.com/@davideriksson_91895/dropwizard-kotlin-project-kronslott-e2aa51b277b8)
- [Logging in Kotlin – the right approach](https://amarszalek.net/blog/2018/05/13/logging-in-kotlin-right-approach/)
- https://bednarek.wroclaw.pl/log4j-in-kotlin/
- https://jaxenter.com/kotlin-logging-168814.html



