---
date: 2022-08-23 14:06:00
language: en
slug: bundler-report-outdated
title: How to use `bundler_report` to identify outdated gems
---

# How to use `bundle_report` to identify outdated gems

The `next_rails` gem provides us with a tool called `bundle_report` that we
can use to identify outdated gems in our *bundle*.

To install it we can use the typical `gem install next_rails`.

After that we can simply use `bundle_report` to get a report that will look
like this:

```
$ bundle_report outdated

trollop 1.16.2:
  released almost 11 years ago
  (latest version, 2.9.10, released about 1 year ago)
jwt 1.5.6:
  released over 4 years ago
  (latest version, 2.2.2, released 6 months ago)
factory_bot 5.2.0:
  released 10 months ago
  (latest version, 6.1.0, released 7 months ago)
factory_bot_rails 5.2.0:
  released 10 months ago
  (latest version, 6.1.0, released 7 months ago)
rails 5.2.4.4:
  released 5 months ago
  (latest version, 6.1.1, released 30 days ago)
chunky_png 1.3.15:
  released about 2 months ago
  (latest version, 1.4.0, released about 1 month ago)
tzinfo 1.2.9:
  released about 2 months ago
  (latest version, 2.0.4, released about 2 months ago)

0 gems are sourced from git
17 of the 199 gems are out-of-date (9%)
```

This report will list all outdated gems, with the date and version of both
the latest release and the one that you are using.

At the bottom we'll also find the number of gems that are being sourced from
git and the ratio of gems that are outdated.
