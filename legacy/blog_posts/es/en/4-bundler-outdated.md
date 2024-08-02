---
date: 2022-08-23 14:10:00
language: en
slug: bundler-outdated
title: How to use `bundler` to identify outdated gems
---

# How to use `bundler` to identify outdated gems

*Bundler*, the dependecy manager for *ruby* provides us with a tool to identify
the gems in our bundle that are not on their latest version.

We can simply use `bundle outdated` to get a similar report to this:

```s
$ bundle outdated

Gem                Current  Latest  Requested  Groups
actioncable        5.2.4.4  6.1.1
actionmailer       5.2.4.4  6.1.1
actionpack         5.2.4.4  6.1.1
actionview         5.2.4.4  6.1.1
activejob          5.2.4.4  6.1.1
activemodel        5.2.4.4  6.1.1
activerecord       5.2.4.4  6.1.1
activestorage      5.2.4.4  6.1.1
activesupport      5.2.4.4  6.1.1
chunky_png         1.3.15   1.4.0
factory_bot        5.2.0    6.1.0   ~> 5       default
factory_bot_rails  5.2.0    6.1.0   ~> 5       default
jwt                1.5.6    2.2.2
rails              5.2.4.4  6.1.1   ~> 5.2     default
railties           5.2.4.4  6.1.1
trollop            1.16.2   2.9.10
tzinfo             1.2.9    2.0.4
```

This report lists each gem with the current version, the latest version and
the restraint applied in the `Gemfile`, and the groups they are required if any.

Knowing the groups might be useful since production and development can be
treated differently in terms of updating, since the latter usually are less
risky to update.
