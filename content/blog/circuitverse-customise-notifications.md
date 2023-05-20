---
title: Customising Notification Events in Rails
date: 2022-12-13T21:34:36+08:00
tags: ["notifications", "circuitverse"]
series: ["customise notification in rails"]
featured: false
---
## 🎯 GOAL: 
Customise notification events

With reference to issue [#3395](https://github.com/CircuitVerse/CircuitVerse/issues/3395)
& PR [#3396](https://github.com/CircuitVerse/CircuitVerse/pull/3396) in [Circuitverse](https://circuitverse.org/)

![image](https://user-images.githubusercontent.com/85568177/208179472-400da83b-f59c-4f9a-bf9b-4244fe55f6c4.jpg)

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

These preferences can be checked in each notification event.

- `if: :fork_notifications?` - Calls `fork_notifications?` and cancels delivery method if `false` is returned

- You can use the if: and unless: options on your delivery methods to check the user's preferences and skip processing if they have disabled that type of notification.


```ruby
class ForkNotification < Noticed::Base
  deliver_by :database, association: :noticed_notifications, if: :fork_notifications?

  def fork_notifications?
    project = params[:project]
    recipient = project.author
    return true if recipient.preferences[:fork] == "true"

    false
  end

```



## 🤹 Handling Updates

All updates are handled by a single patch request which accpets three params

- type : type of notification
- action: “true or false”
- count: number of fields

`config/routes.rb`

```ruby
patch "/:id/notifications/edit", to: "users/noticed_notifications#edit", as: "edit_notifications"
```


In 
controllers/users/noticed_notifications_controller.rb


```ruby
def edit
  NotificationPreference.new(params).call
  redirect_back(fallback_location: root_path)
end
```

## 👨‍🔧 Refactoring the controller to a service object

The edit method in the controller runs the following service-

Based on the params recieved in the patch request, the specific notification event preferences are updated.

 ```ruby
class NotificationPreference < ApplicationController
  def initialize(params)
    super()
    @recipient = User.find(params[:id])
    @notific_type = params[:type]
    @active = params[:active]
    @count = params[:count]
  end

  def call
    if @count == "1"
      if @notific_type == "star"
        update_star
      else
        update_fork
      end
    else
      update_all
    end
  end

  private

    def update_fork
      if @active == "true"
        @recipient.update!(fork: "false")
      else
        @recipient.update!(fork: "true")
      end
    end

    def update_star
      if @active == "true"
        @recipient.update!(star: "false")
      else
        @recipient.update!(star: "true")
      end
    end

    def update_all
      if @active == "true"
        @recipient.update!(star: "false", fork: "false")
      else
        @recipient.update!(star: "true", fork: "true")
      end
    end
end
```

Example Route params- 

```ruby
http://localhost:3000/users/24/notifications/disable_new?type=star
http://localhost:3000/users/24/notifications/disable_new?type=star&&action=true&&count=1
```

## 📦 Dealing with Production Database

For Production we would have create a migration to set default preferences of the notification events to "true". 
- For new users, the preferences are set to true on creation.

Creating migrations using `rails-data-migrations` Gem

```bash
rails generate data_migration populate_preferences_to_users
```

Updating all the default preferences to be true

```ruby
class PopulatePreferencesToUsers < ActiveRecord::DataMigration
  def up
    User.all.update!(star: "true", fork: "true")
  end
end
```

After running migration - 
```ruby
rake data:migrate
```


![Screenshot 2022-12-17 at 1 19 40 AM](https://user-images.githubusercontent.com/85568177/208177880-e72178ca-6586-4f08-af69-5e4c7d87274b.png)


## 📝 Test Cases

















