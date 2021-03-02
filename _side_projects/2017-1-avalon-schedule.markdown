---
layout: post
year: 2017
title: "Avalon Schedule"
---

I had an issue in my university: a schedule might have changed the day before a lecture, and students might have missed the change if they hadn't checked the university website in time. I developed a mobile app that loads the schedule, shows it in a format suitable for mobile devices, and sends push notifications if it has changed.

<iframe width="650" height="370" src="https://www.youtube.com/embed/N8YupgQnMzE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The app loads the schedule's webpage and parses HTML to get a table from it (there was no public API). Every 2 hours, the app's background job checks the schedule in the background, and if it changes, it notifies the user about it.

Technologies used: Java, RxJava, Android SDK.
 
Source code: [GitHub](https://github.com/binary-machinery/AvalonSchedule)