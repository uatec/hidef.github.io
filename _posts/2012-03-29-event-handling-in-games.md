---
title: "Event Handling In Games"
author: Robert Stiff
layout: post
tags: ["gamedev"]
date: 2012-03-29
---

Being predominantly an applications programmer, I have a different take on things that people in the games development area might. Event handling is something that one might do quite differently in many different situations. Here are the ideas I've had and the pros and cons.

<!--more-->

## Callbacks
Provided by the language. C#, nowadays, makes extensive use of **Funcs**, **Actions** and provides **Lambdas** to let you do it. Lots of other languages do this too. Even though this is a new concept in some of the higher level languages, only arriving in C# in 2003, C has had this for donkeys years. With C++ probably being the biggest gamedev language this option is widely available for most gamedevs for a long long time, however I have rarely seen it used.

## Messages
A more application oriented than game oriented way of handling the problem would be to send messages around the **game**, be they objects or strings or hash tables which contain the relevant information. This gives us the option to **trigger an event** and then not care about when or where or how it happens. The advantage in application programming is that you don't need to worry about handling the event, other code will. Great for chopping things up, but I have never seen this approach used in a game, **EVER**.

## Ad Hoc Handling
The most common use of event handling that I have seen in games, is just handling it as it comes. I understand that this is nice and simple and logical to have your update method identify that some work needs doing and doing it. But:

1. This buries your events deep in your update method 
2. It can make your update method a huge and unwieldy mass of code 
3. It forces you to process events **AS** they happen

In a simple game this is probably the easiest option as you don't have to think about it to go down this route. However as your game grows, you will be tied to doing **ALL** event work in any given update method, regardless of whether it requires immediate attention or not.

I would probably put myself in favour of a **Messaging approach**, especially if you are doing any networking. If you can make your game a system which passes messages around from component to component, then passing messages to a component on a remote system via networking is nothing new.

As such, the next game that I work on I intend to approach it as a messaging exercise to see if it as easy as I suspect to transition from a message based game engine to a remote-messaging based game engine (networking).

Does anybody have a different view on it? Does anybody do anything else?

---

Comments to: http://www.reddit.com/r/gamedev/comments/rj9mf/event_handling_in_games/ please.