---
title: "GSoC Week 7"
date: 2023-07-23
tags: ["gsoc"]
series: ["gsoc"]
featured: true
---

## Passing the Midterm Evaluation 🎉

> ![midterm-evaluation-gsoc23](/images/gsoc/midterm-evaluation-gsoc23.png)

## Feedback from Mentor 📝

> ![midterm-evaluation-gsoc23-feedback](/images/gsoc/midterm-evaluation-feedback-gsoc23.png)

With this GSoC Phase 2 begins.

## Week 7 work 💼 

Over the past few days, I have been occupied with my final semester exams until Thursday, which consumed much of my time.
However, during the rest of the days I worked on serving assets using ActiveStorage PR [pull](https://github.com/CircuitVerse/CircuitVerse/pull/3860).
After discussing with my mentor, he advised me to implement a feature flag based approach for rolling out this new feature. This required making some necessary changes to the existing codebase.

Implementing feature flags in Rails proved to be a little challenging since they can only be applied at the method level, unlike class-level definitions.
Applying feature flags at the class level would have required reloading those specific classes on every incoming request.


Rails offers a configuration called `reload_classes_only_on_change` which when set to false reloads classes on every request.

```ruby
config.reload_classes_only_on_change = false

```
It comes with 2 File Checkers: [file_watchers](https://guides.rubyonrails.org/configuring.html#config-file-watcher)

```ruby
ActiveSupport::FileUpdateChecker

# depends on the listen gem
ActiveSupport::EventedFileUpdateChecker
```

This feature is primarily intended for development purposes, where frequent code testing and reloading are essential. Using it in a production environment would lead to 
increased RAM and CPU consumption, which is undesirable.
Consequently, I decided to update my approach and introduced a new migration to roll out the feature more effectively.

Now, with my final exams behind me and a month-long break ahead, I finally have the opportunity to dedicate more time to CircuitVerse. Starting next week, my focus will 
be solely on creating a container-based deployment workflow for CircuitVerse using GitHub Actions and mrsk.






