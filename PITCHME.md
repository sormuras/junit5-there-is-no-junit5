@title[JUnit 5 - There Is No JUnit 5]
# JUnit 5
## There Is No JUnit 5

<br/>
<br/>
<a href="https://www.micromata.de"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/micromata-logo.png" height="80" /></a>
<a href="https://junit.org/junit5"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-logo.png" height="80" /></a>
<a href="https://maven.apache.org"><img src="https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/maven-logo-black-on-white.png" height="80" /></a>
<br/>
<small>
😉 Christian Stein &nbsp; 🐦[@sormuras](https://twitter.com/sormuras) &nbsp; 📰[sormuras.github.io](https://sormuras.github.io)
</small>

+++

@title[Agenda]
#### There Is No JUnit 5
## Agenda
<br>

@ul

- What is JUnit 5?
- JUnit API Evolution
- Jupiter **Live Coding**
- Questions and Answers

@ulend

---

# What is JUnit 5?

+++

## There is **no** JUnit 5

Note:
Not a single artifact.

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-0.png)

Note:
Now, into the red box: the Platform

+++
 
#### JUnit 5 = ...
# Platform
<br>

@ul

- Foundation for launching test engines
- Defines and uses **`TestEngine`** ☑ interface

@ulend

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-0.png)

Note:
Next, let's dive in to green box: Jupiter

+++
 
#### JUnit 5 = Platform + ...
# Jupiter
<br>

@ul

- **`JupiterTestEngine` is a `TestEngine`**☑
- New programming model for writing tests
- New extension model for writing extensions
- *JUnit 5*

@ulend

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-0.png)

Note:
Don't forget your legacy, the yellow box: Vintage

+++

#### JUnit 5 = Platform + Jupiter + ...
# Vintage
<br>

@ul

- **`VintageTestEngine` is a `TestEngine`**☑
- Enables running JUnit 3 and 4 tests

@ulend

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-0.png)

Note:
Now the most interesting part in blue color: Your Engine

+++

#### JUnit 5 = Platform + Jupiter + Vintage + ...
# Your Engine
<br>

@ul

- **`YourTestEngine implements TestEngine`**☑
- What is a test? **You define it!**
- How is a test evaluated? **You define it!**

@ulend

+++

#### JUnit 5 = Platform + *many engines*
# 3<sup>rd</sup>-party Engines ☑

TestNG, Specsy, Spek, KotlinTest, Cucumber, Drools, jqwik, ...

<small><https://github.com/junit-team/junit5/wiki/Third-party-Extensions></small>

---

# This is JUnit 5

+++

![JUnit 5 Architecture](https://github.com/sormuras/testing-in-the-modular-world/raw/master/img/junit5-architecture-1.png)

--- 

# JUnit API Evolution

--- 

# Jupiter Live Coding

---

# Take-away

* There is no JUnit 5 - use JUnit 5! ✅
* Try and test Java modules. ☕
* How to test them best? To be seen...

+++

# Thanks!
#### Happy testing. 

*twitter* [@sormuras](https://twitter.com/sormuras)
*blog* [sormuras.github.io](https://sormuras.github.io)
*slide:* [junit5-there-is-no-junit5](https://github.com/sormuras/junit5-there-is-no-junit5)
