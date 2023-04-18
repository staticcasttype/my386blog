---
layout: post
title:  "Vision Score and Win Condition: Supports Shouldn't Ward?!"
author: Stephanie Clark
description: The "ultimate" answer to how warding affects your win rate, and why you shouldn't prioritize vision as a support.
image: /assets/images/keriasaysnotowards.png
---

Welcome back! In my [previous blog post](https://staticcasttype.github.io/my386blog/2023/03/31/Blog-3b-Vision-Score-And-Win-Condition-Data-Copy.html) I explored the vision score data I collected from Riot Games API. Today I am here to explain the conclusions and present my final data in a clean way.

# Gameplay Background

If you are unfamiliar with League of Legends (if you have followed my blog so far, you should be well aware of the game), vision score is important because it gives your team an edge to know where the enemy team might be, and if you can secure objectives that will give your team a buff. When I was first coming into this dataset, I was expecting a boring data process to fulfill my class assignment, since of course, vision always helps your team win, right?

Maybe not.

# Role Responsibility

Typically, it is the support's job to maintain vision throughout the map. As a support main myself, I make sure to prioritize my vision score because I believe it means I'm helping my team. However, when we separate each vision metric along each role and compare the averages for wins and losses, what we find is surprising.

# Average Vision Metric of Wins and Losses Against Every Role

![Figure](https://raw.githubusercontent.com/staticcasttype/my386blog/main/assets/images/datastorytelling.png)

Support is the only role where having a higher vision score, with the exception of "Vision Wards Bought", is more correlated with a loss than a win. I have a few theories on why this is the case.

### Theory 1: With Great Priority Comes Great Diminishing Returns

Vision is an essential part of the game, that definitely needs priority. That being said, when a support player prioritizes warding river over helping their ADC fight a 2v1, they are- in technical terms- inting. There are often times when a support player wanders away from a teamfight to place or kill a ward, and while they boost their vision score by one or two points, they end up throwing a teamfight that costs them the entire game.

I love warding. It's my favorite part about playing support, and I think that is why so many other support players enjoy the role. But when we prioritize our silly little lamps over our teammates, we pave the way for our failure. Warding shouldn't be ignored- but it should be done wisely and at appropriate times, with a bigger mindset that allows your teammates to sometimes take priority.

### Theory 2: A Thoughtless Ward is a Useless Ward

Another theory I have is that, in an effort to boost vision score, support players might place their wards in useless locations. If dragon is spawning in 30 seconds, and the support wastes all of their wards in topside enemy jungle, they will earn 3-6 extra points for vision score, but profit their team nothing.

It is important to know when objectives are spawning, and to be aware of enemy appearances and disappearances so you can safeguard your team from untimely ganks, and to help your team plan a fight around a drake, Rift Herald, or Baron.

### Additional Thoughts

One metric not unique to the support role, but that I thought was interesting to point out and explain, was that the "Wards Killed" metric was actually negatively correlated with winning the game. I think it is valuable to notice that this is likely due to the temptation to kill a ward when it might be unsafe to do so. If there is a ward nearby, particularly a fresh ward that was only recently placed, there are enemies nearby watching it who may be trying you bait you into an unfavorable situation.

Players are too willing to facecheck bushes and stick around to hit control wards 4 times when they are isolated, forcing the rest of their team to manage a 4v5 while they wait for their respawn timer to run out.

# Conclusions

Today we learned that higher vision score is correlated with a higher winrate for every role except support, and then we discussed a few reasons why support shouldn't put so much priority on vision to, in theory, win more games.

However, vision score is only one tiny aspect of the game and *only* changing how you approach warding won't win you more games. League of Legends is a complicated game that is always experiencing updates, champion nerfs and buffs, and adding new champions and items every couple of weeks. However, becoming aware of bad habits that may contribute to your abysmal winrate can only serve to make you a more well-rounded player who is able to sharpen their mind on the game's intricaties and make more thoughtful plays. 

# Resources

If you would like to explore this data yourself, you can see the dataset I collected from scraping Riot's API as well as my own EDA and manipulation of the data [here](https://github.com/staticcasttype/blog3). And if you play League, comment below what role you main and how much you think about vision score!
