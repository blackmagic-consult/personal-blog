---
title: Cache Busters
date: 2019-10-16 22:54:10
tags:
---

I am a developer, as I have written about many times, but sometimes I act as a System Admin, and this is a story about me doing that, and a Wordpress Installation from Hell.

Wordpress has a bad temperment on a nice day, but this particular website is something special. Ten different developers (probably more) have worked on this Website, six of them are MIA, and if anyone besides me is writing documentation, I don't know where it is.

Errors crop up all the time. The Server Stack is a spiders web of crossed wires and faulty connections, and making a change in one place, no matter how minimal it may seem at the time, can have a cascade of failures resulting in serious downtime. I learnt this the hard way in my early days, and I tend to move slowly now, but the trail of events in this particular story is so bizzare, and blind sided me so hard, I could not have predicted it.

## Chapter One - The Error Report

When users try to share a blog post on Social Media, the featured image is not showing up - instead we're getting a random image on the webpage - or at the very least, I could find no correlation between posts and what image was chosen to be featured; sometimes it was a picture in the post, sometimes the logo in the header - often, nothing.

This isn't mission critical, but it is annoying - and I've had it happen before - not on this website in particular, but on Wordpress sites. Usually, it's an issue with the Media Cache, so the first thing I do is purge it and force Wordpress to re-cache all its media.

This doesn't work, and on further inspection, our cache is *completely empty*. The website hadn't been Cacheing anything locally for *years* - which is a problem in itself, but not related to this one. So we need to look elsewhere.

## Chapter Two - Down the Rabbit Hole

So it's not a cache issue - at least, not in the way we're expecting.

The next step I took was a Javascript Health Check. There are no errors firing when I update the image, and it uploads the photo to file storage just fine. I'm completely stumped at this point. As far as I can tell, everything is working properly.

My next step was loading a mock post full of pictures and figuring out how Wordpress was choosing the image to feature, since it was ignoring whatever we selected. This is when I notice something else strange.

We can create new blog posts just fine. But editing a post and trying to save it - no can do.

We can save drafts fine, but not preview them - it just loads forever, and trying to publish an update sends off a very vague error message.

It takes me awhile, but I do eventually figure out why. When I click the "Update" or "Preview" button, it sends a [POST](https://en.wikipedia.org/wiki/POST_(HTTP)) Request, and for some unknown reason, this POST request is failing.

Normally, when a server stops responding to POST requests, it stops responding across the board - not only when it's one kind.

A strange sight to see, but it explains why I couldn't find any error messages. I was looking in the wrong log.

## Chapter Three - WAF the FUCK

Three weeks prior to the error being reported, we switched our DNS over to Cloud Flare.

Before, we had no WAF running, or a Web Application Firewall, and I set up Cloud Flare to run a standard one for us. It was only a matter of time before a cross-site script attack was run, and before we had already been struck by a dodgy SQL injection.

There in our firewall logs, hidden amongst hundreds of REAL attack logs that the Firewall had prevented, was the secret:

**Ruleset 100003: DoS - Query String Cache Busting - number1=number2**



## Chapter Four - What the Fuck is Cache Busting

On a standard installation of Wordpress, it will cache static files, since they don't frequently get updated - that way, users don't have to download the entire website every time.

But what happens when static media does get updated? We need a way to tell browsers to fetch the new content.

Enter Cache Busting.

You can bust your cache three ways: 

1. Using file name versioning (e.g. style.v2.css)
2. Using file path versioning (e.g. /v2/style.css)
3. Using a query string (e.g. style.css?ver=2)

Our Wordpress install uses query strings to bust the cache, but it's not best practice anymore, due to Cache Bust DDoS attacks.

Essentially, how the attack works is you run a query string bust over and over again, despite the fact that nothing has updated. This stresses the server, eventually crashing it if it doesn't have DDoS protection.

Like I mentioned earlier in this post, the websites Cacheing rules weren't configured correctly.

So, every time we updated the page, Wordpress would go: "Bust the cache, there is a new file, you have version 1.0, the new version is 1.0"

The Web Application Firewall saw this and went "Wow, thats a DDoS attack. I'm not letting that request reach the server."

So we couldn't update blog posts, or change the featured image.

The problem was solved by fixing the cache rules, so now not only is the problem solved, but the website also got a performance boost.

End scene.