---
title: Django vs Supabase
date: 2022-01-11
description: 'Coming from a Django background, this post is my experience of using Supabase for the first time (spoiler I was impressed).'
# image: images/cctv.jpeg
---

Coming from a Django background, this post is my experience of using Supabase for the first time (spoiler I was impressed).

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

which makse it perfect for a JS webapp.  Supabase no doubt does lots of other things, but these are the elements that immediately stood out to me.