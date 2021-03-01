---
layout: post
year: 2019
title: "Generative Music"
---

Generative (a.k.a. procedural) music is music created by a system, music created by algorithms. Inspired by some works of [Brian Eno](https://en.wikipedia.org/wiki/Brian_Eno), I decided to implement an app to produce generative music. I studied music theory at that time, so I tried to use some fundamental music ideas in the app. E.g., if chords are played in a key (e.g., C Major), they most probably will sound good.

<iframe width="650" height="346" src="https://www.youtube.com/embed/8z5i2JQDt9s" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="650" height="346" src="https://www.youtube.com/embed/SjmrtRajBBE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The main issue with the project is that generated melodies sound almost the same. Apparently, it's not that easy to teach a computer to produce music with just random numbers. E.g., a key doesn't only mean that there are a particular set of chords. It also means that there is a particular root note that tends to sound more often than the others. But how much more often?

We need to either use machine learning here or use some manual weight coefficients for root notes. The first one looks like the job for Google, Apple, Spotify, or another company with an immense amount of music data. The second one sounds like too many manual coefficients to make it sound good. So, the project is frozen at the moment.

Technologies used: C#, Unity.

Source code: [GitHub](https://github.com/binary-machinery/GenerativeMusic)
