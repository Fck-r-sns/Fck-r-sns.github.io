---
layout: post
year: 2020
title: "Cuckoo in Isolation"
subtitle: "An UE4 text game with dynamic narrative developed for a game jam"
---

`Cuckoo in Isolation` is a small text game made for a game jam. In the game, a bird with the name Cuckoo must survive in self-isolation. Each game day, a player makes decisions, what Cuckoo will do to maintain her well-being and mental health. E.g., she can start a blog, start a hobby, subscribe to Flexlix, get a pet, or buy a gaming console and participate in holy wars for the best gaming console on the internet. Each turn, something bad happens that can affect Cuckoo's well-being. If a player survives 30 days, it's a win. If well-being is 0, the game ends.

![](/assets/img/personal-projects/cuckoo.png) 

The game's main feature is the dynamic narrative. The player's decisions can change the game state. The game uses this state to determine which options are available to the player and what random events can happen. E.g., if Cuckoo has a blog and a pet, there might be an option to write a blog post about the pet. If Cuckoo has a subscription to a video streaming service, there might be a random event when her favorite series is closed because of bad ratings. 

Technologies used: C++, Unreal Engine 4.

Source code: [GitHub](https://github.com/binary-machinery/cuckoo_in_isolation)
