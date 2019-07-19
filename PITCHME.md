@title[JUnit 5 - Platform & Jupiter API]
# JUnit 5
## Platform & Jupiter API

<a href="https://www.micromata.de"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/micromata-logo-horizontal.svg" height="80" /></a>

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

- User Guide: 📜 https://junit.org/junit5/docs/current/user-guide
- Samples: https://github.com/junit-team/junit5-samples
- Workshop JUnit 5: c.stein@micromata.de
- Support 💚 JUnit: https://junit.org/sponsoring
- Java 12: https://jdk.java.net/12 ☕
- Accento.dev: https://accento.dev am 24. September 2019

@ulend
</small>

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

#### JUnit 4 - One Jar To Rule Them All

![JUnit 4 Usage](https://raw.githubusercontent.com/marcphilipp/presentations/master/junit5-intro/junit4-usage.svg?sanitize=true)

JUnit 4 ships as a single artifact.

+++

#### JUnit 4 - Rename a private field...

...what could possibly go wrong?

![JUnit 4 #976](https://raw.githubusercontent.com/marcphilipp/presentations/master/junit5-intro/serialization-bug.png)

+++

## Separation of Concerns

- Extensible mechanisms to discover and execute tests
- An API to write tests
- An API to write extensions

+++

## Design Goals

- *Flexibility:* Adding new features should be easy
- *Backward Compatibility:* Old tests should still run
- *Forward Compatibility:* Tools should be able to execute _new_ tests

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

- Defines and uses **`TestEngine`**☑ interface
- Foundation for launching test engines

@ulend

+++

#### Test Engine ☑ Interface

```java
package org.junit.platform.engine;

public interface TestEngine {

  TestDescriptor discover(EngineDiscoveryRequest discoveryRequest...);

  void execute(ExecutionRequest executionRequest);

}
```
@[3](`TestEngine` interface)
@[5](Discovery)
@[7](Execution)
@[1-9]

<small>📜 [JUnit Platform Launcher API](https://junit.org/junit5/docs/current/user-guide/#launcher-api)</small>

+++

#### Platform Launcher

```java
   LauncherDiscoveryRequestBuilder.request()
     .selectors(
        selectPackage("org.example.user"),
        selectClass("org.example.payment.PaymentTests"),
        selectClasspathResource("/some/asset.txt"),
        selectClasspathRoots(Set.of(Path.of("..."), Path...)),
        selectDirectory("/home/user/tests"),
        selectFile("./test.list"),
        selectMethod(OrderTests.class, "test4"),
        selectModule("com.example.tool"),
        selectUniqueId("unique-id-1"),
        selectUri("http://what-ever.abc")
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
@[24](Build Discovery Request and...)

@ulend

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

Specsy, Spek, KotlinTest, Cucumber, Drools, jqwik, Mainrunner...

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
  - Netbeans (≥ 10.0)
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

 
+++ 
 
### Checks
 
| JUnit 4 | Jupiter |
| ------- | ------- |
| [`Assert`](https://junit.org/junit4/javadoc/latest/org/junit/Assert.html) | [`Assertions`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Assertions.html) |
| A set of assertion methods useful for writing tests. | `Assertions` is a collection of utility methods that support asserting conditions in tests. |

+++

### Jupiter Assertions

- `assertThrows()` was `@Test(expected=...)`
- `assertTimeout()` was `@Test(timeout=...)`
- `assertEquals(expected, actual, message)`
- `assertAll()`
- `assertLinesMatch()`

+++

### Unconditional Break-outs

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`@Ignore`](https://junit.org/junit4/javadoc/latest/org/junit/Ignore.html) | [`@Disabled`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Disabled.html) |
| Disable a test or a group of tests. | `@Disabled` is used to signal that the annotated element is disabled. |

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
| [`@BeforeClass`](https://junit.org/junit4/javadoc/latest/org/junit/BeforeClass.html) | [`@BeforeAll`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeAll.html) |
| [`@Before`](https://junit.org/junit4/javadoc/latest/org/junit/Before.html) | [`@BeforeEach`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/BeforeEach.html) |
| [`@After`](https://junit.org/junit4/javadoc/latest/org/junit/After.html) | [`@AfterEach`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterEach.html) |
| [`@AfterClass`](https://junit.org/junit4/javadoc/latest/org/junit/AfterClass.html) | [`@AfterAll`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/AfterAll.html) |

+++

## 🔀 Changed Concepts

Changed, evolved, matured.

+++

### Tagging and Filtering

| JUnit 4 | Jupiter |
| ------- | ------- |
| [`@Category`](https://github.com/junit-team/junit4/wiki/Categories) | [`@Tag`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/Tag.html) |
| `Categories` runner and marker interfaces | Plain `String`s as markers, [Tag Expressions](https://junit.org/junit5/docs/current/user-guide/#running-tests-tag-expressions) at Platform level. |


<small>📜 [Tagging and Filtering](https://junit.org/junit5/docs/current/user-guide/#writing-tests-tagging-and-filtering)</small>

+++

### Test Instance Lifecycle

| JUnit 4 | Jupiter |
| ------- | ------- |
| New test instance _per-method_ | `@TestInstance` with `Lifecycle.PER_METHOD` or `Lifecycle.PER_CLASS` |

<small>📜 [Test Instance Lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle)</small>

+++

### Extensions

| JUnit 4 | Jupiter |
| ------- | ------- |
| Competing `Runner`, `@Rule`, and `@ClassRule` concepts | `Extension` marker interface |

<small>
- Lifecycle: `BeforeAllCallback`, `BeforeEachCallback`, `BeforeTestExecutionCallback`, `TestExecutionExceptionHandler`, `AfterTestExecutionCallback`, `AfterEachCallback`, `AfterAllCallback`
- Other: `ExecutionCondition`, `TestInstanceFactory`, `TestInstancePostProcessor`, `ParameterResolver`, `TestTemplateInvocationContextProvider`
</small>

<small>📜 [Extension Model](https://junit.org/junit5/docs/current/user-guide/#extensions)</small>

+++

## ✨ New Jupiter Concepts

+++

### Composed Annotations

```java
@Tag("fast")                     // file: FastSystemTest.java
@Tag("system")
@Test
@interface FastSystemTest {}

@FastSystemTest                  // file: FirstJupiterTests.java
void mySecondTest() {...} 
```
@[1-2](Multiple tags)
@[3](Mark as test)
@[6-7](Use your composed annotation)
@[1-7]

+++

### Messages

- [`TestReporter`](https://junit.org/junit5/docs/current/api/org/junit/jupiter/api/TestReporter.html)
- Print extra/contextual information
- Don't use _stdout_ or _stderr_

+++

### Gathering, Grouping, Nesting

- Non-static nested test class (i.e., an inner class) that can share setup and state with an instance of its enclosing class

<small>📜 [Nested Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-nested)</small>

+++

### Testlets

- Generate "tests" at runtime

```java
@TestFactory
Stream<DynamicTest> checkAllTextFiles() throws Exception {
  return Files.walk(Paths.get("demo/test/jump"), 1)
      .filter(path -> path.toString().endsWith(".txt"))
      .map(path -> dynamicTest(
              "Checking file '" + path.getFileName() + "'",
              path.toUri(), // test source uri
              () -> checkLines(path)));
}
```

<small>📜 [Dynamic Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-dynamic-tests)</small>

+++

### Repetitions (via Test Templates)

- `@RepeatedTest` specifying the total number of repetitions desired

```java
@RepeatedTest(10)
void repeatedTest() {}

@RepeatedTest(5)
void repeatedTestWithRepetitionInfo(RepetitionInfo info) {
	assertEquals(5, info.getTotalRepetitions());
}
```

<small>📜 [Repeated Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-repeated-tests)</small>

+++

### Parameterized Tests (via Test Templates)

| JUnit 4 | Jupiter |
| ------- | ------- |
| Via limiting `Parameterized` runner | `@ParameterizedTest` in dedicated artifact |

- `@ValueSource`, `@EnumSource`
- `@CsvSource`, `@CsvFileSource`
- `@MethodSource`
- `@ArgumentsSource(Provider.class)`

<small>📜 [Parameterized Tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests)</small>

+++

### Parallel Execution

- <small>`junit.jupiter.execution.parallel.enabled = true`</small>
- `@Execution(ExecutionMode.CONCURRENT)`
- Parallel! 🚀🚀🚀...

<small>📜 [Parallel Execution](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parallel-execution)</small>

---

# Outlook

- Motivation
- Architecture
- Live Coding
- Concept Comparison
- 👉 **Outlook**

+++

## Coming In Jupiter 5.6

- New reporting format [opentest4j: #9](https://github.com/ota4j-team/opentest4j/issues/9)
- ❌ Remove Script-based Conditions
- Bug Fixes

+++

## Future

- Parameterized Classes [#871](https://github.com/junit-team/junit5/issues/871)
- Declarative Test Suites [theme: suites](https://github.com/junit-team/junit5/labels/theme%3A%20suites)
- _Your ideas?_

+++

## Take Away

- There is no JUnit 5...
- Use JUnit 5! ✅

+++

# Thanks
#### Happy testing!

Support 💚 JUnit<br>https://junit.org/sponsoring

<small>
_twitter_ [@sormuras](https://twitter.com/sormuras)<br>
_blog_ [sormuras.github.io](https://sormuras.github.io)<br>
_slides_ [junit5-there-is-no-junit5](https://github.com/sormuras/junit5-there-is-no-junit5)<br>
</small> 
