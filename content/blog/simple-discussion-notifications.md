---
title: Customising Notification Events in Rails
date: 2023-1-10T21:34:36+08:00
tags: ["rails", "notifications", "pre-gsoc", "circuitverse"]
series: ["customise notification in rails"]
featured: true
---



## 🎯 GOAL
Adding Notification Events to Simple Discussion 

Adding two new notification events:

1. Admin users(moderators) receive notification of all new threads.
2. All users that are subscribed to thread receive a comment notification. 


## Forum Thread Notifications

This sends notification to all moderators about a new thread, so that they can respond to the query asap.

For example in the following 

![admin-thread](https://user-images.githubusercontent.com/85568177/211388029-7d1fba45-59de-4842-b9f1-7fa5969c9284.jpg)

![meow-thread](https://user-images.githubusercontent.com/85568177/211388033-327b17fe-e64d-467a-9efc-17c81b554698.jpg)

![after-redirect](https://user-images.githubusercontent.com/85568177/211387995-424adb1f-40c9-4569-82cf-1a1ae13fecd1.jpg)
![b](https://user-images.githubusercontent.com/85568177/211388014-ee2b5d37-2966-4c1d-a65b-2fb39db745b1.jpg)
![convo](https://user-images.githubusercontent.com/85568177/211388017-c40d77b9-e323-4d77-9917-77b915ea831d.jpg)
![a](https://user-images.githubusercontent.com/85568177/211388022-e27bbab4-1773-4b80-bda2-bdccb145f51e.jpg)
![1](https://user-images.githubusercontent.com/85568177/211388026-5ac72320-c0ed-419d-977c-629a9c76aab8.jpg)



There were two methods to make this work -

1. To make a preferences table with users as foreign key and notification events as attributes.
2. To user rails ActiveRecord::Store and add a preferences hash/jsonb to users to table.

I found the second approach better and implemented the same. It was most used by rails developers to implement such features.

## 👤 Implementing the preferences in users model
Setting default notification settings to be true for each user.(which they can edit later)

```ruby
store :preferences, accessors: %i[star fork], coder: JSON
after_commit :set_preferences, on: :create

def set_preferences
    preferences[:star] = "true"
    preferences[:fork] = "true"
end
```
