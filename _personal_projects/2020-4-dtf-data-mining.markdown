---
layout: post
year: 2020
title: "Daily TeleFrag data analysis"
subtitle: "How I downloaded all data from a website using its API and analyzed it"
---

[DTF](https://dtf.ru/) (a.k.a. Daily TeleFrag) is one of the most famous Russian websites about games, movies, and popular culture in general. It has API, and all users and posts there have incremental identifiers. It means I can write a script to fetch all public users and posts data one by one by requesting IDs starting with 1 and up to the last registered user or last written post. And I did it.

API has a limit for requests per second, but I managed to copy all users and posts data to my local database. I parsed raw JSON responses and wrote them to PostgreSQL tables as regular columns. After that, I could use SQL to process the data and make some conclusions about the website's audience and content.

E.g., this is a graph of new users per week.

![](/assets/img/personal-projects/dtf_new_users.PNG)

There are definitely some anomalies here, so this way, I detected some bot attacks on the website.

I wrote a series of articles about DTF data analysis (in Russian), which were quite popular on the website.

- [Analysis of users with open data](https://dtf.ru/flood/157826)
- [Analysis of posts and subsites with open data](https://dtf.ru/flood/167514)
- [How I found hidden subsites with open data](https://dtf.ru/u/354-evgeniy-prihodko/171552)

Technologies used: Python, PostgreSQL.

Source code: [GitHub](https://github.com/binary-machinery/ochoba_data_mining)
