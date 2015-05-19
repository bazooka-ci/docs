# Building a PHP project

This guide walks you through the build environment and configuration topics specific to PHP projects. Please make sure to read our [Getting Started](./home/getting_started) and [general build configuration](/home/build_configuration) guides first.

## Choose the PHP language

First of all, you need to specify in your build configuration that you project main language will be PHP

```yaml
language: php
```

## PHP versions

Bazooka built-in PHP support includes PHPUnit and Composer. It provides the following PHP version:

* 5.4
* 5.5
* 5.6

You can choose multiple versions on which your project will be tested:

```yaml
language: php
php:
  - 5.4
  - 5.5
  - 5.6
```

This will generate [permutations](/home/permutations) on which your project will be tested.

## Default phases

The defaults generated for a *php* project are equivalent to the following configuration:
```yaml
script: phpunit --configuration /phpunit_$DB.xml --coverage-text
```
