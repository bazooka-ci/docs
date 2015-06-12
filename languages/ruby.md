# Building a Ruby project

This guide walks you through the build environment and configuration topics specific to Ruby projects. Please make sure to read our [Getting Started](./home/getting_started) and [general build configuration](/home/build_configuration) guides first.

## Choose the Ruby language

First of all, you need to specify in your build configuration that you project main language will be Ruby

```yaml
language: ruby
```

## Ruby versions

Bazooka built-in Ruby support provides the following Ruby version:

* 2.0
* 2.1
* 2.2
* jruby1.7.20

You can choose multiple versions on which your project will be tested:

```yaml
language: ruby
ruby:
  - 2.2
  - jruby1.7.20
```

This will generate [permutations](/home/permutations) on which your project will be tested.

## Default phases

No default phases are generated for a *ruby* project. It's up to you to configure which build tool you want to use. For instance:

```yaml
before_install: gem install bundler

before_script: bundle install

script: bundle exec rake
```
