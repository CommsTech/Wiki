---
title: 
description: 
dateCreated: 
published: 
editor: markdown
tags: 
dateModified: 
---
# Home_Assistant_Chores

Original Article [Chore Tracking with Point System in Home Assistant - Smart Home Pursuits](https://smarthomepursuits.com/chore-tracking-with-point-system-in-home-assistant/)

**In this guide, I’m going to show you how to create your own chore or task tracker within Home Assistant, using only default Home Assistant features** – no third-party products or additional addons/integrations are needed. You can optionally assign a point value to each chore, allowing your kids to earn “points” for completing their tasks. I consider this setup to be similar to a sticker chart or reward system.

This setup has several moving parts, but I will document each step in detail so you can recreate it yourself. We will be creating Home Assistant Scripts, Helpers, Automations, and using Counters for this.

By the end of the guide, you will have created:

- Toggleable chores in a lovelace card
- Incrementing points after a task is complete
- Grid card with buttons to add, remove, or reset the counter
- Automations to reset daily chores & reset points counter weekly
- Automation to send notification once daily chores are complete
- Automation to send notification when point threshold is reached, earning your child their allowance

I also wanted to give credit where credit is due, so thanks [/u/brandroidian](https://www.reddit.com/user/brandroidian/) for the inspiration. I first stumbled upon this [Reddit thread](https://www.reddit.com/r/homeassistant/comments/lakixx/my_son_has_to_complete_chores_before_the_tv_smart/) a few months ago and loved the idea. He didn’t have a points system for it, so I wanted to expand on his idea a little.

---

## Step 1: Create “Input Booleans” for Chores

The first thing we need to do is create a few chores. To do this, we are using **_input booleans_**, which are basically going to be dummy entities that can be triggered from a script later on.

Navigate to **Configuration > Helpers**. Click **Add Helper.**

Choose the **Toggle** helper and give it name. I already have chore_1 – 7 created, so for this example I’m using _chore_8_.

I recommend naming them _chore_1_, _chore_2_, _chore_3_, etc, as this is what the Entity ID will be named. If you name the entity something like _Sweep_Kitchen_ at this step, your entity will end up being _input_boolean.sweep_kitchen_. This isn’t necessarily a problem, but when it comes to adding chores to the Lovelace card, it will be easier to search for and add chores 1-5 to the entities card, rather than trying to remember the exact entity name.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image.png)

You can choose an icon if you’d like, but this can be customized later on so I wouldn’t worry about it here. You can now see the Entity ID it created.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-1.png)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-2-1024x64.png)

Click the entity so you can name it something useful, like **“Sweep Kitchen”** and click Update.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-4.png)

Now, you have a easy to remember chore name but the entity ID is short and easy to find later on.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-5.png)

To start, I recommend creating 3-5 chores. You can always add more later.

---

## Step 2: Create an entities card

Optional: If you will be using this for your kids, I recommend creating a new view for each child.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-3.png)

Click **Add Card > Entities**.

Name it something like “Morning Chores”, uncheck Show Header Toggle, toggle Color Icons by State, and then add your chore_8 entity. Add the rest of your chore entities.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-6.png)

---

## Step 3: Create a Chore Counter for Points/Reward System

Navigate to **Configuration > Helpers**. Click **Add Helper**, and choose “**Counter**“.

Name it Chore Counter, set initial value to 0, and Step Size to 1 (This will add 1 point each time a chore is completed).

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-8.png)

## Step 4: Create Add, Remove, and Reset Counter Scripts

Next, we need to create a few scripts. This is so we can trigger adding points from button clicks and automations.

Navigate to **Configuration > Scripts > Add Script**.

### Add 1 Point Script

Name: Add 1 Point

Icon: hass-plus-thick

Under Sequence, choose **call service** and **counter.increment**.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-9.png)

---

### Remove 1 Point Script

Name: Remove 1 Point

Icon: hash:minus-thick

Under Sequence, choose **call service** and **counter.decrement**.

---

### Reset Points Script

Name: Reset Points

Icon: hash:backup-restore

Under Sequence, choose **call service** and **counter.reset**.

---

## Step 5: Create Lovelace Grid card to Add, Remove, or Reset Counter

Navigate to your child’s view again. Click **Add Card > Grid.** This will add a grid of helpful cards to allow you or your child to add, remove, or reset points. For box 4, we can set instructions and for box 5, you show the number of points your child has earned.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-10.png)

Set Columns to 3. Click the + button to add 5 cards to the grid.

**Boxes 1-3 are button cards.**  
B**ox 4 is a markdown card.**  
B**ox 5 is an entity.**

Here are the 5 grid cards I created.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-11.png)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-12.png)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-13.png)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-14.png)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-15.png)

Go ahead and test out your buttons. It should now add, remove, and reset your counter correctly and the points will be visible in box 5.

---

## Step 6: Create Automations

### Automation#1: Increment counter once chore is completed

It’s great that we can now add add points from Lovelace, but I want to avoid having my kids need to toggle the chore as complete, and then click a button. So let’s automate that.

**Configuration > Automations > Create Automation.**

Name it something like “If task complete, then add 1 point”

For triggers, choose **State** and the entity of your chore. Add “On” to the To field. If you have 5 chores, you will add 5 triggers.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-16.png)

For Actions, choose **call service** and use the script you just created called **script.add_one_point**.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-17.png)

Here is the full yaml if you’d prefer to just copy and paste this into the automation

```
alias: If Task Complete Then Add 1 Point
description: ''
trigger:
  - platform: state
    entity_id: input_boolean.chore_1
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_2
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_3
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_4
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_5
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_6
    to: 'on'
  - platform: state
    entity_id: input_boolean.chore_7
    to: 'on'
condition: []
action:
  - service: script.add_one_point
mode: single
max: 10
```

To test, go back to your child’s view and toggle a chore on and off. It should successfully add a point to your counter. Toggling off with do nothing – it won’t add or remove points.

### Automation #2: Reset chores at 1am

This one was a fun automation to create. You can either hardcode a time to reset the entity state to off in the automation, or, if you’d like to be able to change the time from lovelace, you can create a **date/time helper**.

I’ll show you how to create the helper in this example. For this automation, you can set the time from Lovelace to reset the state of the chores to Off at 1am. Here’s what a date/time helper looks like in Lovelace:

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-19.png)

**Configurations > Helpers > Add Helper > Date and/or time**

Name it **Chore Date Helper** and choose **time** only.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-18.png)

Then, add this as an entity to your existing “Morning Chores” entities card.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-21.png)

Now, create a new automation. Name it “**Reset Chores**“, give it a Trigger Type of **value of a date/time helper**, and choose the **input_datetime.chore_date_helper**

Conditions: Time Fixed Time, Fixed Time and toggle Monday-Sunday.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-22.png)

Actions: call service using service anem “input_boolean.turn_off”

Targets: Pick entity (Chore 1-5)

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-23.png)

And here is the full yaml for this automation:

```
alias: 'Reset Chores'
description: ''
trigger:
  - platform: time
    at: input_datetime.chore_points_date_helper
condition:
  - condition: time
    weekday:
      - mon
      - tue
      - wed
      - thu
      - fri
      - sat
      - sun
action:
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_1
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_2
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_3
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_4
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_5
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_6
  - service: input_boolean.turn_off
    data: {}
    entity_id: input_boolean.chore_7
mode: single
```

To test this out, go back to Lovelace, toggle on your chores, and then set the value of the date/time helper to 1 minute ahead of what time it is now. Once the automation triggers, it should turn off the chores. You will eventually want this time to something overnight so the chore tracker resets before they wake up the next day.

### Automation #3: Reset Points Weekly

This is similar to automation #2, except we are resetting points once per week instead of daily. We can still use the date/time helper which is great, we will just tell it to only run on Mondays at 1am.

Create a new automation and name it **Reset Chore Counter**.

Under Conditions, toggle Monday only.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-24.png)

Under Actions, **call service** and choose **counter.reset**. Choose _Pick entity_ and select _chore counter_.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-25.png)

Full yaml:

```
alias: 'Reset Chore Counter'
description: ''
trigger:
  - platform: time
    at: input_datetime.chore_points_date_helper
condition:
  - condition: time
    weekday:
      - mon
action:
  - service: counter.reset
    target:
      entity_id: counter.chore_counter
mode: single
```

### Automation #4: If points threshold is reached, send me a notification

This automation will let you know once your kid has reached your arbitrary “number” of points needed to earn their allowance (or whatever reward system you choose). Again, very similar to rewarding a child for filling up a sticker chart. If they have 5 chores a day to complete, they earned 5 points. 5 points x 7 days a week = 35 points.

Give it a name. Under Triggers, choose Numeric State and set to 34 (or one less than your arbitrary number). This is so you the automation triggers once it hits 35 points.

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-26.png)

Under Actions, send a notification:

![](https://smarthomepursuits.com/wp-content/uploads/2021/06/image-27.png)

Full yaml:

```
alias: Chore Points Threshold Reached
description: ''
trigger:
  - platform: numeric_state
    entity_id: counter.chore_counter
    above: '34'
condition: []
action:
  - device_id: b9ae47b5fe201965b76b088888888
    domain: mobile_app
    type: notify
    title: Daughter has completed her weekly chores!
    message: Reminder to take daughter out for ice cream!
mode: single
```

---

## Points Gauge

**Update 8/31/21:** I’ve recently revamped my setup a little bit. My daughter goes to her dads a few days a week every other week, which made tracking points on the weeks she was here a little more difficult. So, I used a gauge card instead of just displaying the overall points and and set different point thresholds for each gauge. Less points are needed when she isn’t here the full week and now she can see her progress.

![](https://smarthomepursuits.com/wp-content/uploads/2021/08/image-247.png)

### Home Week Gauge

![](https://smarthomepursuits.com/wp-content/uploads/2021/08/image-245.png)

### Away Week Gauge

![](https://smarthomepursuits.com/wp-content/uploads/2021/08/image-246.png)

Also, instead of hardcoding the “reset” time in the automation, I use a Date/Time Helper. I edited my automation to use the value of a date/time helper and added a card to Lovelace so I can adjust the reset time from the Lovelace if I wanted.

![](https://smarthomepursuits.com/wp-content/uploads/2021/08/image-248.png)

### Restricting Access to Buttons

I added the [Lovelace Restriction Card](https://smarthomepursuits.com/how-to-create-a-lovelace-restriction-card-in-home-assistant/) to Home Assistant via HACS. This allows me to lock my “Add 1 Point” and “Remove 1 Point” Lovelace buttons so my kids can’t add points themselves.

### Viewing History of Completed Chores

If you’d like a quick and easy way to see how many chores were completed, the easiest way I could figure this out was using a history card and the mini-graph addon.

Let’s say your points script resets on Sunday night, but you forgot to see if the weekly chores were completed. With this card, you can view the history for the last 36 hours to see if number of completed chores were met.

![image](https://community-assets.home-assistant.io/original/3X/0/3/032ed367d75f8823f45265404e329f3f0710f420.png)

```
entities:
  - counter.chore_counter
icon: hass:broom
hour24: false
hours_to_show: 36
name: Chore Counter History
show:
  extrema: true
  graph: bar
  icon: true
  name: true
  name_adaptive_color: true
  icon_adaptive_color: true
type: custom:mini-graph-card
```

---

## Wrapping Up

This was an incredibly fun project to work on. Prior to this, I had never used helpers, scripts, or counters. It was a great learning experience, and I hope you guys find it useful as well!

As I created this, I realized this can definitely be expanded upon for things other than just your kids chores. For example, create tasks for mowing the lawn, taking medicine, or washing the car. I use the app TickTick for my own personal checklist, but having a few repetitive things directly in Home Assistant is pretty awesome.

Good luck! Let me know if you have any questions or run into any issues along the way.