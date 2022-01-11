---
title: Django vs Supabase
date: 2022-01-11
description: 'Coming from a Django background, this post is my experience of using Supabase for the first time.'
# image: images/cctv.jpeg
---

Coming from a Django background, this post is my experience of using Supabase for the first time).

For anyone coming from a Django background, or a stack with a more traditional backend / frontend split (Rails, Node), this is going to give you a good impression of your experience.

## Is this a fair comparison?
No, not really.  
Supabase and Django are in the same game but in different competitions and aren't trying to do the same thing.  That said, from a developer perspective, you will find yourself considering which is best for your project and thus its helpful to know where they excel.

## So what is Supabase?
[Supabase](https:www.supabase.com) is currently running with the tagline of "the open source Firebase alternative" which is great if you know what Firebase is, but I didn't!  So my take is this: when you spin up Supabase you get the following, within seconds, for free:
- a database
- with an API sitting over the top
- with auth
- with row level security, meaning that you can lock down records to the user who owns them

which makse it easy to get started quickly for a JS webapp.  Supabase no doubt does lots of other things, but these are the elements that immediately stood out to me.

## Impressions Rollercoaster
### First Impressions: wow this is so easy
Spinning up a Supabase environment, runnning through the React User Management tutorial, tweaking the tutorial to enable Oauth for Google logins was all a breeze.  This made me think what the process would have been like with Django and made me realise how quickly Supabase could get to this point.  With Django, spinning up that environment and getting the base level settings would have taken significant time, although something like Django Cookie Cutter would cut a decent amount of the boilerplate.

### Second Impressions: wow I have a lot to learn
As I then progressed to trying to do some relatively simple things, I realised that I had little idea what was going on.  The very first thing I tried to do was to edit the React User Management code to be able to handle two user types which was tracked via their Profile.  The first user was the Service Provider who would be setting up the Portal, and the second user type was the Client, who would be receiving the information in the portal.  For this, I simply wanted to write either "Provider" or "Client" to the Profile.Role field, however to do this I needed to:
- Get the User's ID after their account was created, which with my bad JS async knowledge was a challenge
- Take that User's ID and create them a Profile 
- Write the Role of "Provider" to that Profile

In Django, I would have handled the Profile creation with a User post save signal, however with Supabase I wasn't really sure how to go about it.  It seemed like Triggers within Supabase were one way to handle this, but this led me down the question of where should I be handling which operations, which functions should I write as a trigger in the Supabase backend, which functions should I handle from the React webapp. I had forgotten that I already knew the common patterns from Django, from sheer time in motion but also from principles books like Two Scoops of Django.  Sometimes it takes trying to learn something new to realise what you know.