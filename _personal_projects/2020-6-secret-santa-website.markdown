---
layout: post
year: 2020
title: "Secret Santa website"
subtitle: "A Vue.js / Flask website for family Secret Santa events"
---

Every New Year, we have a Secret Santa family event. One day we decided to create a personal website where people can sign in and join the event. When the event starts, its admin runs the sorting algorithm, and the app sends emails to all participants with a person to whom a participant needs to give a gift.

![](/assets/img/personal-projects/secret-santa.png)   

It is my first website project that I created from scratch. I decided to try Vue.js for the frontend and Flask for the backend. Almost immediately, I discovered how many different technologies I need to use to create a relatively simple website. Need to keep a user session? Here is Flask-Login. Need to share user state between Vue controllers? Here is Vuex. And so on.

The sorting algorithm uses backtracking search in a graph. I wrote an article about it.

- [How to solve the Secret Santa Problem using graph theory](https://binary-machinery.github.io/2021/02/03/secret-santa-graph.html)   

Technologies used (yes, I listed every single one): Python, Flask, Flask-CORS, Flask-Login, Passlib, SMTP, JavaScript, Vue.js, Vue Router, Vuex, Axios, HTML, AWS EC2, nginx.

The backend project was created together with [kath-leen](https://github.com/kath-leen).

Source code: [Frontend@GitHub](https://github.com/binary-machinery/secret_santa_frontend) [Backend@GitHub](https://github.com/binary-machinery/secret_santa_backend)
