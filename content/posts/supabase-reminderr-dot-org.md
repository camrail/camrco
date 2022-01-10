---
title: Reminderr.org made with, and to learn, Supabase
date: 2022-01-09
description: 'Using Supabase and React to build a portal to remind clients of important information between appointments.  Think the exercise routine that your physio gave you for your sore back, or the things that your psychologist has asked you to think about.'
image: images/cctv.jpeg
---

## Reminderr.org - made with Supabase.


Using Supabase and React to build a portal to remind clients of important information between appointments.  Think the exercise routine that your physio gave you for your sore back, or the things that your psychologist has asked you to think about.

## Tech Stack
- React
- Supabase

I've used the [Supabase React Quickstart](https://supabase.com/docs/guides/with-react) as the base React code which I will then morph into Reminderr.org.

I considered using Next.js but figured my React familiarity is low enough that any additional level of complexity wouldn't be a good idea.  Might regret that if this thing blows up (ha!) and I've skipped the production ready stuff.

## Why Reminderr?
### Business Case

When you go to see someone for help, be they a psychologist, mortgage broker or a physio, they'll often give you information in the session that they hope you'll remember, as well as give you homework.

The problem comes then comes with firstly the queue to do the things (those back exercises), THEN remember what the things actually are (bend over awkwardly 10 times).

### Tech case
While there's a semi plausible (but half baked) business case here, the real reason I want to build this app is so I can try out [Supabase](www.supabase.com) and revisit React.  I found out about Supabase while reading [Micro Sass Ideas](https://www.microsassidea.com) and coming from a purely Python/Django background it has piqued my interest.

This particular idea is appealing because:
- It's small
- Object model is simple
- The application is simple enough for me to focus on UI
- I like being B2B2C and the ability to offer white labelling type functionality...not sure why, just think portals are cool


## Head Scratchers

#### Error - there is no unique constraint matching given keys for referenced table "users"
I was trying to add a lookup from a client to their consultant so that we can tell which clients belonged to which user.  To do this, I was adding a foreign key from the `Profile` table to the `Users` table and was using `auth.users.instance_id` as the foreign key field.  For some reasons `instance_id` has no unique constraint (not sure why) which means that there could be ambiguity for the foreign key, so it throws the error.

Solution was to use the `id` field on `User` rather than `instance_id`.

## TBC - This build is in progress