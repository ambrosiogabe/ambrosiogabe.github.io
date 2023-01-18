---
layout: post
title:  "Some Insights into Game Programming"
date:   2017-09-15 14:02:32 -0500
categories: [Game-Programming]
tags: [programming, python, java]
cover: /public/images/articleHeaders/downloadPong.png
---

What game to start with? What programming language do I use? Do I build my own game engine? These are all questions that we ask and more, I wish to address some of these if I can. First, let me start off by saying that I am a self-taught game programmer, as the title of my website implies, so all the things I will teach you are things that I learned myself and you can too if you need to. I say this as hope to all you out there who want to get started, but don't think they can. I'm here to tell you that you can do it. It may be difficult and there may be times of trouble, but you can accomplish it.

Which Language?
===============

So, one of the biggest questions I had starting off was, what programming language do I use to program games?! All the AAA games use C, but that looks like gibberish. Websites use flash and Javascript, Minecraft was coded in Java, Unity games are coded in C#, some people code games using PyGame, what do I use?! All the different choices may seem overwhelming at first, but, instead of having it overwhelm you, take it as a good thing. The variety of games coded in several different languages shows us that the language does not necessarily matter. You can make it big with any language. And, from personal experience of coding games in several different languages including, Java, Javascript, C#, and Python, I can tell you that the differences can be major, but the underlying concepts are the same, every time. So, with this in mind, I suggest you just start! Just start with whatever programming language you are most comfortable with, and if you don't know how to program at all, I would suggest learning something to start!

There are some very useful books out there that teach you to program by starting with games. If you go down that course, I would suggest you make sure the book is a recent copy, since languages change so much, and not to give up. Also, the beginning games are going to be very mundane. You may have to start with something as simple as a text adventure game, or guess my number, etc. Don't lose hope because these are great beginner games. And this brings me to my next subject. What games should you start with?

Which Games to Start With?
==========================

Now, there are several different games out there. I mean you have everything from Mario to Call of Duty, but as a beginning game programmer it can be overwhelming with such a variety. If I may, let me tell you one thing, start small! Taking too much at once can deter you from pursuing it further because you will be deterred by the amount of learning and the lack of progress. Instead, take joy in the little games, and when you start off with some nice things! When you code that first game, be proud of it, show it to your friends and family and show them how great of a job you did!

Now with that aside, I'm going to simply give you a list:

<ul>
  <li><b>Text Adventure:</b> This will introduce you to the game loop, important game structures, the winning and losing transitions, and game state managers, etc.</li>
  <li><b>Pong:</b> This will introduce you to real time games. Important concepts include: introduction to threads, managing several states, collision detection, simple AI.</li>
  <li><b>Arkanoid:</b> This will introduce you to: having screen boundaries, introducing random factors to change ball speed, keeping score.</li>
  <li><b>Snake:</b> This game was a doozy for me. Some concepts introduced: using a data structure as the main character, including random power ups.</li>
  <li><b>Space Invaders:</b> Concepts introduced: Path-following AI, levels, more advanced controls.</li>
  <li><b>Tetris:</b> Concepts introduced: Complex data structures for each piece, more complex scoring system, random piece generation, keeping a list of high scores, finding general methods to rotate complex structures, determining whether objects can be rotated, etc.</li>
  <li><b>2D-Platformer:</b> This can be any game, something like Mario would suffice. Concepts introduced: Implementing a scrolling camera to follow the character, more complex enemies, more complex collision detection, sprite animations, etc.</li>
</ul>

Now with a list of games in mind I will discuss the next important question, should you build your game engine, or use a prefab, like Unity?

Game Engine?
============

Now, I am biased. The way I learned was building my games from the ground up. Starting with no code, and just coding away in notepad++ until I accomplished my goal, a demonstrable game that was fun to play. In my opinion, every game programmer should know how to build a simple game engine. This way, when you are using something like Unity, you have a much fuller appreciation for the underlying concepts. Also, if something goes wrong, it is much, much easier to pinpoint where trouble may lie. Another reason, AAA games actually prefer to build there own game engines for their games. Albeit they have a whole team of programmers working to accomplish this, but understanding the whole of how it works helps to understand each individual piece of the game.

Now, I am not saying to code all the games above from scratch! Honestly, the only ones mentioned above that I have done that with are the Text Adventure, Pong, Arkanoid, Snake, Tetris, and the 2D-Platformer. So, I can tell you with confidence that most of the games above are achievable if you do wish to do that. My advice is that you at least do a few of the simpler games from the ground up so you can build an appreciation for the underlying code, and so that you can understand what goes on behind the scenes more fully.

So, with all that aside, game engine? Yes and no. I think you should do it for some of the simpler games, then, when you move up to more complex games that you may sell and market, I would suggest switching to something like Unity. It streamlines the process and makes your life a whole lot easier. So this is about all I have to say on this matter. I will give a few suggested websites for artwork and music, and then I will have a download available below of one of my custom games built from the ground up, just to give you a little hope!

Game Assets
===========

Where can you get good game assets? This is the next question that, and I promise you, will plague you when you begin to create games. Here are a few sites that should help you out!
<ul>
  <li><b>Artwork:</b><a href="http://www.2dgameartguru.com/"> 2D Game Art</a>, <a href="http://www.gameart2d.com/freebies.html">Game Art 2D</a>, <a href="https://opengameart.org/">Open Game Art</a></li>
  <li><b>Music:</b> <a href="https://freesound.org/">Free Sounds</a></li>
</ul>

Impossible Game
===============

Listed below is a game I created with some instructions on how to download it. Just another reason for you to have some hope and know that you can achieve it. Oh, and by the way, I coded this from scratch in Java. So it is possible!

Here are a couple of screenshots of gameplay.

![impossible-game-image](/public/images/impossibleGameDownload/gameplay.png)

This is the title menu, and the one below is actual gameplay.

![impossible-game-image](/public/images/impossibleGameDownload/gameplay2.png)

The game may look familiar, and that's because it is. I based it off of The World's Hardest Game, so all credit to the idea goes to the creators of The World's Hardest Game, but the artwork and the program that runs the game are all coded from scratch by me. You can download the game in the link right below, I hope you do and I hope you enjoy it!<br><br>
[Download Here]({% post_url 2017-09-21-install-the-impossible-game %})
