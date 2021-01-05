---
layout: post
year: 2017
title: "Avalon Schedule"
---

When I was in a university getting my second master's degree, there was a problem: a schedule might have changed the day before a lesson and students might have missed the change if they hadn't checked the university website in time. I decided to implement a mobile app to work with the schedule in a format better suitable for mobile devices and which checks the schedule and notifies a user if it has been changed.

The app is pretty straightforward: a user sets a link to the particular schedule page, the app loads it and parses HTML to get a table from it (there was no public API, only HTML). Once in a while (every 2 hours as far as I remember) the app checks the schedule in background and if it changes, it notifies the user about it.

It was published to Google Play Market and had 15 installs, so there were some people who found it useful.

<iframe width="560" height="315" src="https://www.youtube.com/embed/N8YupgQnMzE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Technologies used: Java, RxJava, Android
 
Source code: [GitHub](https://github.com/binary-machinery/libmodbus_cpp)