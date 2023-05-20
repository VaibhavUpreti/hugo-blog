---
title: Implementing Notifications in Simple Discussion Gem 
date: 2023-1-10T21:34:36+08:00
tags: ["notifications", "circuitverse"]
series: ["circuitverse"]
featured: false
---



## 🎯 GOAL
Adding Notification Events to Simple Discussion 

Adding two new notification events:

1. Notifying Admins on a new thread post 
2. Notifications to users that have subscribed to the thread



## New Thread notification:
This sends notification to all moderators about a new thread, so that they can respond to the query asap.

For example in the following 


-  for all admin users, moderators so that they can resolve the query asap.
- eg: ref, both the admin user receive notification when `user1` posted a new thread.

![admin-thread](https://user-images.githubusercontent.com/85568177/211388029-7d1fba45-59de-4842-b9f1-7fa5969c9284.jpg)

![meow-thread](https://user-images.githubusercontent.com/85568177/211388033-327b17fe-e64d-467a-9efc-17c81b554698.jpg)

## Forum Comment notification:

-  for all the users that have subscribed to the thread.
- eg: Users that have subscribed to a conversation receive notifications below...
conversation

![convo](https://user-images.githubusercontent.com/85568177/211388017-c40d77b9-e323-4d77-9917-77b915ea831d.jpg)

> Notifications

![b](https://user-images.githubusercontent.com/85568177/211388014-ee2b5d37-2966-4c1d-a65b-2fb39db745b1.jpg)

![a](https://user-images.githubusercontent.com/85568177/211388022-e27bbab4-1773-4b80-bda2-bdccb145f51e.jpg)

![1](https://user-images.githubusercontent.com/85568177/211388026-5ac72320-c0ed-419d-977c-629a9c76aab8.jpg)



### Redirects 
1. Comment notification redirects to the comment

![after-redirect](https://user-images.githubusercontent.com/85568177/211387995-424adb1f-40c9-4569-82cf-1a1ae13fecd1.jpg)

2. Thread notification redirects to the thread


### Future Scope
To add a central notification preference (new_thread_notification )for all users (not just admin users) to receive notification on a new thread... default false .... In customise event notification PR

Code can be found [here](https://github.com/CircuitVerse/CircuitVerse/pull/3483)
