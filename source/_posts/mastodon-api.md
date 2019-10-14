---
title: Breaking my Brain with the Mastodon API
date: 2019-06-13 23:56:25
tags:
---

I have a desire, a simple one (in my opinion, at least.)

I want my nice-looking [polybar](https://polybar.github.io/) configuration let me know whenever I get a Mastodon notification.  It'd be convenient, save me from checking Mastodon more then I need too, and I know Mastodon has an open API that would (should) make it easy for me to spit out my unread notifications via curl, wrap that bugger in a shell script, and put it on my app bar.

Anyways, I start browsing the mastodon documentation, and after messing around with 0Auth for a little bit - my dreaded nemesis - I've hacked together the following command:

```
curl -H "Authorization: Bearer "Access Token" https://mastodon.social/api/v1/notifications
```

Which, you know, gave me what I was expecting - my notifications via json. But not just my unread ones - all of them. In fact, after thoroughly scouring the documentation, it looks like there is no way to make it **just** list my unseen notifications, I always get the whole list.

But I know for a fact there must be a seen vs. unseen tag, because my seen notifications will sync across my phone and desktop, i.e, I don't have to dismiss them on both, just one.

Further evidence supporting this tag theory is that when I log into mastodon on a new browser I'm not wiped out by all my notifications from the beginning of time.

Either way, this tag is not listed in the JSON i get out of my API request.

Frustratingly as well, there is no way to make it spit out a number - just the notification list. Not the end of the world but a waste of resources for what I'm doing.

I'm completely stumped! I have no idea what to do next.

I **could** make this code work, but the amount of black magic to make it happen would be torturous to me, and wasteful of instance-masters resources. 

Any advice?