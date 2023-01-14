---
layout: post
title:  "Download Snake!"
date:   2017-10-04 17:58:32 -0500
categories: [Games]
tags: [programming, java, game]
cover: /public/assets/images/snake.png
---

Download
========

To download simply right click the link below and save it onto you computer. Make sure you have Java installed, and then click the downloaded file to play!

[Snake.jar (95.1kB)][1]

Background
==========

Everyone loves Snake! This game was a bit of a doozy for me, it took me a little bit to finally figure out how to make my snake work. Another hard part of getting the game to work was finding out a way to have images embedded into my JAR file. It turns out you can embed images within the JAR file, just not text files, unless they are in binary. This proved challenging at first, but was quickly solved.

The real hard part was figuring out how to get that dang snake working! I finally realized I had to use a clever manipulation of a list to get it to work. I treated the list as a 2D array by saying that every even number was a y-coordinate of a body segment, and every odd number was the x-coordinate. Using these tuples embedded in my list I was able to generate the structure of the snake body that moves so cleverly around the board. It was a fun challenge, one that actually plagued me for a couple years before I finally solved it. I hope you enjoy it as much as I did making it! If you want to find out more about how I created this or any of my other games, comment below and let me know and I'll write a follow up article.

[1]: {{site.url}}/public/downloads/snake/Snake.jar
