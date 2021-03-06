# SLang

[![Build Status](https://travis-ci.org/SonarSource/slang.svg?branch=master)](https://travis-ci.org/SonarSource/slang)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=org.sonarsource.slang%3Aslang&metric=alert_status)](https://sonarcloud.io/dashboard?id=org.sonarsource.slang%3Aslang) [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=org.sonarsource.slang%3Aslang&metric=coverage)](https://sonarcloud.io/component_measures/domain/Coverage?id=org.sonarsource.slang%3Aslang)

This is a developer documentation. If you want to analyze source code in SonarQube read one of the following documentations:

* Ruby language: [analysis of Ruby documentation](https://docs.sonarqube.org/latest/analysis/languages/ruby/)
* Scala language: [analysis of Scala documentation](https://docs.sonarqube.org/latest/analysis/languages/scala/)
* Go language: [analysis of Go documentation](https://docs.sonarqube.org/latest/analysis/languages/go/)

SLang (SonarSource Language) is a framework to quickly develop code analyzers for SonarQube. SLang defines language agnostic AST. Using this AST
we can develop simple syntax based rules. Then we use parser for real language to create this AST. Currently Ruby and Scala 
analyzers use this approach.

## Ruby

We use [whitequark parser](https://github.com/whitequark/parser) to parse Ruby language by embedding it using JRuby runtime.

* AST documentation for the parser can be found [here](https://github.com/whitequark/parser/blob/master/doc/AST_FORMAT.md)
* We use simple [Ruby script](sonar-ruby-plugin/src/main/resources/whitequark_parser_init.rb) to call the parser and invoke our [visitor](sonar-ruby-plugin/src/main/java/org/sonarsource/ruby/converter/RubyVisitor.java) written in Java 

## Scala

We use [Scalameta](https://scalameta.org/) to parse Scala language.

### Scala coverage

For Scala files, we will import both [Scoverage](http://scoverage.org/) and JaCoCo coverage reports. Note that this will result in strange behavior since:

* Only line coverage will be used from the Scoverage report.
* JaCoCo can be imprecise when computing conditions coverage on Scala code, generating FP (typically on pattern matching).

This situation only applies to two Scala files, this current situation is acceptable.

## Go

We use the native Go parser to parse Go language.

## Have question or feedback?

To provide feedback (request a feature, report a bug etc.) use the [SonarQube Community Forum](https://community.sonarsource.com/). Please do not forget to specify the language, plugin version and SonarQube version.

## Building

### Setup

If you are on Windows, read the [sonar-go-to-slang/README.md](sonar-go-to-slang/README.md) instructions.

*SonarSource internal usage: Configure your `gradle.properties` - read the [private/README.md](private/README.md) instructions.*

### Build
Build and run Unit Tests:

    ./gradlew build

## Integration Tests

By default, Integration Tests (ITs) are skipped during build.
If you want to run them, you need first to retrieve the related projects which are used as input:

    git submodule update --init its/sources

Then build and run the Integration Tests using the `its` property:

    ./gradlew build -Pits --info --no-daemon -Dsonar.runtimeVersion=7.9

You can also build and run only Ruling Tests using the `ruling` property:

    ./gradlew build -Pruling --info --no-daemon -Dsonar.runtimeVersion=7.9

If you want to run ruling tests for specific language, you can use `ruling-{lang}` property (`ruling-scala`, `ruling-ruby`, `ruling-go`). For example:

    ./gradlew build -Pruling-scala --info --no-daemon -Dsonar.runtimeVersion=7.9

## License headers

Note: The license check is not working correctly, see [SONARSLANG-367](https://jira.sonarsource.com/browse/SONARSLANG-367).

When adding a new source file, you will need to add license headers. Instead of copy-pasting blocks, the following command line can be used:

    ./gradlew licenseFormat
