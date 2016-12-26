---
title: "The Caching Antipattern"
author: Robert Stiff
layout: post
tags: ["caching", "Software Development"]
date: 2016-12-25
---

> There are only two hard things in Computer Science: cache invalidation and naming things.
> 
> -- Phil Karlton

## Caching?

To make sure we're on the same page, when I say *caching*, I am talking about the practice of speeding up your own application by masking slow dependencies by remembering previous responses and using them instead of making another slow call to the dependency.

As mentioned breifly by Phil Carlton in his well known sound byte, caching is a tricky problem. This is compounded by a number of common mistakes which I have seen over the years which have resulted in unnecessary confusion delay.

## Common Mistakes

Here are some of the mistakes I have seen and what we should to differently.

<!--more-->

### Cache on startup

When you know for a fact that your dependencies are so slow they are unusable so you don't even try at run time. Pre-populating a cache as your application starts up rather than querying your dependent service is just admitting that your dependency is not fit for purpose. If it's a 3rd party service then maybe there is nothing you can do about that, but all too often this technique is used to avoid having to make your services actually fit for purpose.

The problem with caching like this is that it extends application startup times, making scaling and failure recovery or even non-viable.

And even if you are okay with having a service that is slow to start or restart (which you shouldn't be) it results in a cache which bares no relation to the nature of the data or your service's usage patterns. A simple cache expiry policy would be meaningless here, as this pattern explicitly exists to avoid ever hitting your dependencies again. 

### Caching too early

I don't mean too early in the request lifecycle, I mean too early in the development cycle. Many times I have seen developers write code, decide that it is too slow, and stick cache in front of it. 

From that point on, the fact that their service is slow is hidden. There is no reason to optimise or improve the solution as the cache will ensure that the second request is much quicker; so why worry, right?

### Integrated Cache

What's the S in SOLID? Single Responsibility. If your caching capability is integrated directly in to your service layer and you can't run without it you're definitely in breach of this principle. It's not my my place extoll the virtues of this principle here.

### Caching everything

Blindly applying caching to every external call to ensure responsiveness without a thought for he implications. Even worse, this approach can result in developers and operators not even knowing that caching is occurring and making assumptions about the reliability of the underlying services that are simply not true.

### Recaching

Caching everything, or even just caching a lot, can result in caches that cache caches which cache caches.

On one end of the spectrum, this might result all internal caches expiring shortly before the very front cache, resulting in a huge waste of time and resources to operate layers and layers of caching which are never used.

At the other then, it can result effectively summing the cache expiry time of all caches involved, so 10 layers of 30 minutes caches can result in a system serving data which is 5 hours old. How's *that* for counterintuitive?

### Unflushable Cache

Occasionally cache implementations may back on to data stores like Redis which has management tooling which can be used to flush the cache on demand. 

Other implementations, such as hand cranks in memory caches or even caches provided by mainstream frameworks will not expose any cache management tools. This leaves ops with the only option, to restart the service to flush the memory. (Or worse, know enough about the cache implementation to find it's location on file system and clear it out manually.)

I have seen releases that have taken hours longer than necessary while different members of the team tried to track down cache after cache, flush them with a restart or wait for expiry before moving on to the next layer. All the time with a system taken offline because the system cannot be said to be consist within itself.

### Local Copies

## Why they matter
The implications
Changes take X time to become visible
Development requires continual cache flushing
Cache flushing can be an hours long process
You can't guarantee the state of all assets.
Creates a need for workarounds like cache busters. Should you really need to learn how to work around a feature of your service?
## What we should do instead
### Serve on Stale
### Write through Cache
### Read through Cache
### Honour HTTP headers

Never cache
Edge cache
Lookup instead of cache. Make data first class.

## Finale

If you've come across any fundamental problems caused by caching and bad discipline there of, let me know and I can add them to the list.

