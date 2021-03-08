---
layout: post
year: 2020
title: "Beat Saber modding"
subtitle: "How I reverse-engineered tools for mod injection into Unity games and created my own mod"
---

[Beat Saber](https://store.steampowered.com/app/620980/Beat_Saber/) is a popular VR game powered by Unity. A significant part of its popularity is because of its modding community ([BSMG](https://bsmg.wiki/)). One day I decided to write my own mod, but before it, I wanted to learn how mods are made for Unity games.

I reverse-engineered tools for mod injection into Unity games (BSIPA, Unity Doorstop). They use various tech tricks, e.g., game and engine libraries modification, DLL input address table hooking, and Mono Runtime hijacking. Then I developed a simple mod that shows the amount of time spent in the current game session (both total and active).  

![](https://binary-machinery.github.io/assets/img/2020-05-28-mods-2/20200505214706_1.jpg)

As a result of the research, I wrote two articles about mod injection and development.

- [How mods are made for Unity games. Chapter 1: Inject mods into a game’s code](https://binary-machinery.github.io/2020/05/21/mods-1.html)
- [How mods are made for Unity games. Chapter 2: How to write a mod in C#](https://binary-machinery.github.io/2020/05/28/mods-2.html)

Technologies used: C#, Unity, C# decompiler, C, Harmony.

Source code: [GitHub](https://github.com/binary-machinery/BeatSaberTimeTracker)
