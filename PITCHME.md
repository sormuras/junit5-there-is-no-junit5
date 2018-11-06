@title[JUnit 5 - Platform & Jupiter API]
# JUnit 5
## Platform & Jupiter API

<a href="https://www.micromata.de"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/micromata-logo.png" height="80" /></a><br>
M I C R O M A T A

<a href="https://junit.org/junit5"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-logo.png" height="80" /></a>
&nbsp;
<a href="https://maven.apache.org"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/maven-logo-black-on-white.png" height="80" /></a>
<br/>
<small>
😉 Christian Stein &nbsp; 🐦[@sormuras](https://twitter.com/sormuras) &nbsp; 📰[sormuras.github.io](https://sormuras.github.io)
</small>

+++

@title[Snapshots]
## Snapshots

<small>
@ul

- Support: https://steadyhq.com/en/junit
- Java 11: https://jdk.java.net/11
- Slack: `jvm-german` `#jug-bonn` https://bit.ly/jvmg-invite
- Unconference: https://entwickelbar.github.io am 24. November

@ulend
</small>

+++

@title[Agenda]
## Agenda

@ul

- Motivation
- Architecture
- Live Coding
- Concept Comparison
- Outlook

@ulend

---

# Motivation

- 👉 **Motivation**
- Architecture
- Live Coding
- Concept Comparison
- Outlook

+++

## There is **no** JUnit 5

@ul

- Who uses JUnit?
- Which Build Tool? Which IDE?
- Frameworks?

@ulend

Note:
Writing Tests vs. Running Tests vs. Make writing tests easier for X

+++

#### JUnit 4 - "Architecture"

![JUnit 4 Usage](https://raw.githubusercontent.com/marcphilipp/presentations/master/junit5-intro/junit4-usage.svg?sanitize=true)

JUnit 4 ships as a single artifact.

+++

#### JUnit 4 - Rename a privat field...

![JUnit 4 #976](https://raw.githubusercontent.com/marcphilipp/presentations/master/junit5-intro/serialization-bug.png)

...what could possibly go wrong?

---

# Architecture

- Motivation
- 👉 **Architecture**
- Live Coding
- Concept Comparison
- Outlook

+++

#### JUnit 5 = ...
# Platform
<br>

@ul

- Foundation for launching test engines
- Defines and uses **`TestEngine`**☑ interface

@ulend

+++

#### Platform Launcher

```java
   LauncherDiscoveryRequestBuilder.request()
     .selectors(
        selectPackage("org.example.user"),
        selectClass("org.example.payment.PaymentTests"),
        selectClass(ShippingTests.class),
        selectMethod("org.example.order.OrderTests#test1"),
        selectMethod("org.example.order.OrderTests#test2()"),
        selectMethod("org.example.order.OrderTests", "test3"),
        selectMethod(OrderTests.class, "test4"),
        selectMethod(OrderTests.class, testMethod),
        selectUniqueId("unique-id-1"),
        selectUniqueId("unique-id-2")
     )
     .filters(
        includeEngines("junit-jupiter", "spek"),
        excludeEngines("junit-vintage"),
        includeTags("fast"),
        excludeTags("slow"),
        includeClassNamePatterns(".*Test[s]?")
        includeClassNamePatterns("org\.example\.tests.*")
     )
     .configurationParameter("key1", "value1")
     .configurationParameters(configParameterMap)
     .build();
```
@[1](Launcher)
@[2-13](Selectors)
@[14-21](Filters)
@[22-23](Custom configuration parameters)
@[1-24]

@ulend

+++

#### Test Engine Interface

```java
package org.junit.platform.engine;

public interface TestEngine {

  TestDescriptor discover(EngineDiscoveryRequest discoveryRequest...);

  void execute(ExecutionRequest executionRequest);

}
```

+++

#### JUnit 5 = Platform + ...

![JUnit 5 Architecture](https://raw.githubusercontent.com/sormuras/sormuras.github.io/master/asset/img/junit5-architecture-a-platform.png)

+++

#### JUnit 5 = Platform + ...
# Vintage
<br>

@ul

- **`VintageTestEngine` is a `TestEngine`**☑
- Enables running JUnit 3 and 4 tests

@ulend

+++

![JUnit 5 Architecture](https://raw.githubusercontent.com/sormuras/sormuras.github.io/master/asset/img/junit5-architecture-b-vintage.png)

+++
 
#### JUnit 5 = Platform + Vintage + ...
# Jupiter
<br>

@ul

- **`JupiterTestEngine` is a `TestEngine`**☑
- New programming model for writing tests
- New extension model for writing extensions
- "*JUnit 5*"

@ulend

+++

![JUnit 5 Architecture](https://raw.githubusercontent.com/sormuras/sormuras.github.io/master/asset/img/junit5-architecture-c-jupiter.png)

+++

#### JUnit 5 = Platform + Vintage + Jupiter + ...
# Your Engine
<br>

@ul

- **`YourTestEngine implements TestEngine`**☑
- What is a test? **You define it!**
- How is a test evaluated? **You define it!**

@ulend

+++

![JUnit 5 Architecture](https://raw.githubusercontent.com/sormuras/sormuras.github.io/master/asset/img/junit5-architecture-d-overview.png)

+++

#### JUnit 5 = Platform + *many engines*
# 3<sup>rd</sup>-party Engines ☑

Specsy, Spek, KotlinTest, Cucumber, Drools, jqwik, ...

<small><https://github.com/junit-team/junit5/wiki/Third-party-Extensions></small>

---

# This is JUnit 5

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-1.png)

+++

## IDE Support

- Excellent support
  - IntelliJ IDEA (≥ 2016.2)
  - Eclipse (≥ 4.7.1a)
  - Visual Studio Code (Java Test Runner ≥ 0.4.0)
  - Netbeans (will come with 10.0)
- For other tools, there's `@RunWith(JUnitPlatform)`

+++

## Build Tool Support

- Native support in
  - **Ant** (≥ 1.10.3)
  - **Gradle** (≥ 4.6)
  - **Maven** Surefire (≥ 2.22.0)
- **`ConsoleLauncher`** to run tests from the command line or to support other build tools (e.g. Bazel)

--- 

# Live Coding

- Motivation
- Architecture
- 👉 **Live Coding**
- Concept Comparison
- Outlook

--- 

# Concept Comparison

- Motivation
- Architecture
- Live Coding
- 👉 **Concept Comparison**
- Outlook

+++

## JUnit 4 | Jupiter

| JUnit 4 | Jupiter |
| :-----: | :-----: |
| [`org.junit`](https://junit.org/junit4/javadoc/latest/org/junit/package-summary.html) | [`org.junit.jupiter.api`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/package-summary.html) |
| Provides JUnit core classes and annotations. | JUnit Jupiter API for writing tests. |

+++

## ♻ Common Concepts

Basic stuff is basic. Commonly.

+++

### Entry-points

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`@Test`](https://junit.org/junit4/javadoc/latest/org/junit/Test.html)  | [`@Test`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Test.html)  |
| The `Test` annotation tells JUnit that the `public void` method ... | `@Test` is used to signal that the annotated method is a test. |

<small>The `expected` and `timeout` annotation elements of `org.junit.Test` are handled by dedicated **Jupiter** assertions.</small>
 
+++ 
 
### Checks
 
| JUnit 4 | Jupiter |
| ------- | ------- |
| [`Assert`](https://junit.org/junit4/javadoc/latest/org/junit/Assert.html) | [`Assertions`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Assertions.html) |
| A set of assertion methods useful for writing tests. | `Assertions` is a collection of utility methods that support asserting conditions in tests. |
| `assertEquals(message, expected, actual)` | `assertEquals(expected, actual, message)` |

+++

### Unconditional Break-outs

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`@Ignore`](https://junit.org/junit4/javadoc/latest/org/junit/Ignore.html) | [`@Disabled`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Disabled.html) |
| Disable a test or a group of tests. | `@Disabled` is used to signal that the annotated test class or test method is currently disabled. |

+++

### Conditional Test Execution

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`Assume`](https://junit.org/junit4/javadoc/latest/org/junit/Assume.html) | [`Assumptions`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Assumptions.html) |
| Stating assumptions about the conditions in which a test is meaningful. | `Assumptions` support conditional test execution based on assumptions. |

<small>Annotation-based conditions for enabling or disabling tests in **Jupiter** [org.junit.jupiter.api.condition](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/condition/package-summary.html)</small>

+++

### Life-cycle Hooks

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`BeforeClass`](https://junit.org/junit4/javadoc/latest/org/junit/BeforeClass.html) | [`BeforeAll`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeAll.html) |
| [`Before`](https://junit.org/junit4/javadoc/latest/org/junit/Before.html) | [`BeforeEach`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeEach.html) |
| [`After`](https://junit.org/junit4/javadoc/latest/org/junit/After.html) | [`AfterEach`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterEach.html) |
| [`AfterClass`](https://junit.org/junit4/javadoc/latest/org/junit/AfterClass.html) | [`AfterAll`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterAll.html) |

+++

## 🔀 Changed Concepts

Changed, evolved, matured.

+++

### Tagging and Filtering

In contrast to the limited `org.junit.experimental.categories.Category` annotation and its associated runner in JUnit 4, JUnit Jupiter uses simple `String`s as markers. 
Test classes and methods can be tagged via the [`@Tag`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Tag.html) annotation.
Those tags can later be used to filter test discovery and execution.

For a detailed description consult the [Tagging and Filtering](https://junit.org/junit5/docs/current/user-guide/#writing-tests-tagging-and-filtering) chapter in the User-Guide.

+++

### Test Instance Lifecycle

In order to allow individual test methods to be executed in isolation and to avoid unexpected side effects due to mutable test instance state, JUnit creates a new instance of each test class before executing each test method.
This "per-method" test instance lifecycle is the default behavior in JUnit Jupiter and is analogous to all previous versions of JUnit, including JUnit 4.
If you would prefer that JUnit Jupiter execute all test methods on the same test instance, simply annotate your test class with `@TestInstance(Lifecycle.PER_CLASS)`.

- `org.junit.jupiter.api.TestInstance` _`@TestInstance` is a type-level annotation that is used to configure the lifecycle of test instances for the annotated test class or test interface._

For a detailed description consult the [Test Instance Lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle) chapter in the User-Guide.

+++

### Extension Points

In contrast to the competing `Runner`, `@Rule`, and `@ClassRule` concepts in JUnit 4, the JUnit Jupiter extension model consists of a single, coherent concept: the [`Extension`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/extension/package-summary.html) API.

For a detailed description consult the [Extension Model](https://junit.org/junit5/docs/current/user-guide/#extensions) chapter in the User-Guide.

+++

## ✨ New Concepts

- REVOLUTION!

+++

### Messages

In JUnit Jupiter you should use [`TestReporter`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/TestReporter.html) where you used to print information to _stdout_ or _stderr_ in JUnit 4.

+++

### Gathering, Grouping, Nesting

Nested tests give the test writer more capabilities to express the relationship among several group of tests.

- `org.junit.jupiter.api.Nested` _`@Nested` is used to signal that the annotated class is a nested, non-static test class (i.e., an inner class) that can share setup and state with an instance of its enclosing class._

For a detailed description consult the [Nested Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-nested) chapter in the User-Guide.

+++

### Testlets

A new kind of test is a dynamic test which is generated at runtime by a factory method that is annotated with `@TestFactory`.

- `org.junit.jupiter.api.TestFactory` _@TestFactory is used to signal that the annotated method is a test factory method._
- `org.junit.jupiter.api.DynamicNode` _DynamicNode serves as the abstract base class for a container or a test case generated at runtime._
- `org.junit.jupiter.api.DynamicContainer` _A DynamicContainer is a container generated at runtime._
- `org.junit.jupiter.api.DynamicTest` _A DynamicTest is a test case generated at runtime._

For a detailed description consult the [Dynamic Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-dynamic-tests) chapter in the User-Guide.

+++

### Test Templates

A `@TestTemplate` method is not a regular test case but rather a template for test cases.
As such, it is designed to be invoked multiple times depending on the number of invocation contexts returned by the registered providers.
Thus, it must be used in conjunction with a registered `TestTemplateInvocationContextProvider` extension.
Each invocation of a test template method behaves like the execution of a regular `@Test` method with full support for the same lifecycle callbacks and extensions.

Copied over from [Test Templates](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-templates)

+++

### Repetitions (via Test Templates)

JUnit Jupiter provides the ability to repeat a test a specified number of times simply by annotating a method with `@RepeatedTest` and specifying the total number of repetitions desired.

- `org.junit.jupiter.api.RepeatedTest` _`@RepeatedTest` is used to signal that the annotated method is a test template method that should be repeated a specified number of times with a configurable display name._
- `org.junit.jupiter.api.RepetitionInfo` _`RepetitionInfo` is used to inject information about the current repetition of a repeated test into `@RepeatedTest`, `@BeforeEach`, and `@AfterEach` methods._

For a detailed description consult the [Repeated Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-repeated-tests) chapter in the User-Guide.

+++

### Parameterized Tests (via Test Templates)

In JUnit 4 a limiting runner was needed from package `org.junit.runners.parameterized`.

In JUnit Jupiter parameterized tests are implemented as a test template extension.
The API resides in its own dedicated module: `org.junit.jupiter.params`
Be sure to include it in your test compile dependencies.

For a detailed description consult the [Parameterized Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests) chapter in the User-Guide.

+++

### Parallel Execution

JUnit Jupiter API for influencing parallel test execution.

- [org.junit.jupiter.api.parallel](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/parallel/package-summary.html)

For a detailed description consult the [Parallel Execution](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parallel-execution) chapter in the User-Guide.

---

# Outlook

- Motivation
- Architecture
- Live Coding
- Concept Comparison
- 👉 **Outlook**

+++

## Coming In Jupiter 5.4

- Bug Fixes
- Display Name Generator
- Test Kit

+++

## Future

- New reporting format [opentest4j: #9](https://github.com/ota4j-team/opentest4j/issues/9)
- Order Sequence of Tests [#13](https://github.com/junit-team/junit5/issues/13)
- Scenario Tests [#48](https://github.com/junit-team/junit5/issues/48)
- Declarative Test Suites [theme: suites](https://github.com/junit-team/junit5/labels/theme%3A%20suites)
- Parameterized Classes [#871](https://github.com/junit-team/junit5/issues/871)
- _Your ideas?_

+++

## Take Away

- There is no JUnit 5...
- Use JUnit 5! ✅

+++

# Thanks
#### Happy testing!

Support JUnit<br>https://steadyhq.com/en/junit

<small>
_twitter_ [@sormuras](https://twitter.com/sormuras)<br>
_blog_ [sormuras.github.io](https://sormuras.github.io)<br>
_slides_ [junit5-there-is-no-junit5](https://github.com/sormuras/junit5-there-is-no-junit5)<br>
_slack_ [slack jvm-german #jug-bonn](bit.ly/jvmg-invite)<br>
</small> 
