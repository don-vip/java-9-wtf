---
title: WTF, Java 9?!
permalink: /
layout: home
---


Some things break on Java 9.
Here I collect [short, self contained, correct (or not) examples](http://www.sscce.org/) demonstrating what fails and (hopefully) how to fix it.
Note that there is no judgment involved!
This is not about whether the things _should_ break or not and whether that's good or bad - it's just about what _does_ break.

Topics or problems get their own little project and site to present themselves.
So far we have trouble with...

* [the compiler](compiler)
* [the Maven JAXB2 Plugin](maven-jaxb2-plugin)
* [Noto Sans](noto-sans)
* [XML transformations][xml-transformer]


## Observe!

The demos are organized as Maven modules (I use Maven 3.5.0).
You can run them individually (in their respective folders) or consecutively (in the root folder) with `mvn clean test`, maybe throwing in `-fae` (fail at end) to execute all even if some fail.

On Java 8, the build should be successful.
To run  it on Java 9:

* [define `JAVA_HOME` in a `.mavenrc` file](https://github.com/CodeFX-org/mvn-java-9/tree/master/mavenrc), pointing to your Java 9 install
* create a symlink for `javac9` and `java9` pointing to the respective binaries in your Java 9 install
* in the `.mvn` folders, rename the non-empty files `jvm9.config` to `jvm.config`, which adds Java-9-specific command line flags required for the project


## Contributions

Contributions are very welcome!
To make sure they have high quality, make sure your demo checks as many of these boxes as possible:

* it should be [as small as possible](http://www.sscce.org/)
* it should be a Maven module, where `mvn clean test` passes on Java 8 but fails on Java 9
* it should demonstrate the behavior without much editing:
    * in the POM use profiles if compiler or Surefire need arguments on different releases
    * if the entire Maven run needs arguments, have a look at how `.mvn/jvm.config` works
* its README should describe the problem, possibly show some code (and link to more), and also point out the solution if one is available; also, don't forget to link to it from above
* the README should also mention the Java 8 and 9 versions on which the described behavior was observed

If you have any questions, [open an issue](https://github.com/CodeFX-org/java-9-wtf/issues/new).
If want to contribute either open an issue or a PR, depending on how much you already have.

Once again, contributions are welcome! :)