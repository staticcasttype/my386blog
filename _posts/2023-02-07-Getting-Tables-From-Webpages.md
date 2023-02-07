---
layout: post
title:  "How to Get a Table from a Webpage"
author: Stephanie Clark
description: A League of Legends themed data extraction technique.
image: /assets/images/rell.png
---

# The Overview
### Queueing Up

![Figure](https://technology.riotgames.com/sites/default/files/lcu_ui_ready_check.gif)

Today I will give an overview of how to grab an existing table from a website for use in data analysis. This is useful because using already collected data saves time, but as beginner python users we don't know how to deal with data that isn't readily available in CSV form.




# Background
### The Draft

![Figure](https://assets.change.org/photos/5/pd/rq/TyPDRqCIMrGqZAB-800x450-noPad.jpg?1589776095)

I'm a huge fan of the popular MOBA game League of Legends. Say I want to analyze the roster of champions with their base stats. Perhaps this data doesn't exist in an easily consumable form already, so what are the ways I could go about obtaining this data?

One way is I can go into the game manually, load up the practice tool on all 162 champions, and record the champions stats at each of the 18 levels with the same runes. This method is perfectly viable, but-like most people in Data Science-I'm lazy. And since you're here, it seems as though you are too. Let's consider other options.

![Figure](https://i.stack.imgur.com/J66uz.png)

Perhaps you can find the information online. Marvelous! But it is not a downloadable csv file. We could copy each cell of the data frame and create our own replica CSV. But I haven't forgotten to be lazy. If only there were a way we could extract this table directly from the website...

![Figure](/assets/images/championtable.jpg)




# The Libraries You Need
### The Laning Phase

![Figure](https://miro.medium.com/max/1200/1*gON3peBAScOzoTRjZndloQ.jpeg)

You will need two libraries to extract a table from a webpage. You will need the *pandas* and the *request* libraries, as shown below.

```
import pandas as pd
import requests
```




# The Code + Explanation
### Teamfighting for Objectives

![Figure](https://i.ytimg.com/vi/cL8PnLrwk-g/maxresdefault.jpg)




# The Final Product
### GGEZ

![Figure](https://thumbs.gfycat.com/DelayedSeparateFishingcat-mobile.mp4)