---
layout: post
title:  "How to Get a Table from a Webpage"
author: Stephanie Clark
description: A League of Legends themed data extraction technique.
image: /assets/images/championtable.jpg
---

![Figure](https://technology.riotgames.com/sites/default/files/lcu_ui_ready_check.gif)

# Queueing Up: The Overview

Today I will give an overview of how to grab an existing table from a website for use in data analysis. This is useful because using already collected data saves time, but as beginner python users we don't know how to deal with data that isn't readily available in CSV form.


![Figure](https://assets.change.org/photos/5/pd/rq/TyPDRqCIMrGqZAB-800x450-noPad.jpg?1589776095)

# The Draft: Background

I'm a huge fan of the popular MOBA game League of Legends. Say I want to analyze the roster of champions with their base stats. Perhaps this data doesn't exist in an easily consumable form already, so what are the ways I could go about obtaining this data?

![Figure](https://i.stack.imgur.com/J66uz.png)

One way is I can go into the game manually, load up the practice tool on all 162 champions, and record the champions stats at each of the 18 levels with the same runes. This method is perfectly viable, but-like most people in Data Science-I'm lazy. And since you're here, it seems as though you are too. Let's think about something else.

![Figure](/assets/images/championtable.jpg)

Eureka! We found the information online. Marvelous! But it is not a downloadable csv file. We could copy each cell of the data frame and create our own replica CSV. But I haven't forgotten to be lazy. If only there were a way we could extract this table directly from the website...


![Figure](https://miro.medium.com/max/1200/1*gON3peBAScOzoTRjZndloQ.jpeg)

# The Laning Phase: The Libraries You Need


## Steps for creating a new post.  






### Resizing images

There isn't a good way to resize images with markdown, so if you need to resize an image, use the html code to insert your figure (instead of the markdown code):

`<img src="https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg" alt="" style="width:400px;"/>`

(Width is 400 pixels)
<img src="https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg" alt="" style="width:400px;"/>


`<img src="https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg" alt="" style="width:100px;"/>`

(Width is 100 pixels)
<img src="https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg" alt="" style="width:100px;"/>

---

## Troubleshooting

Here are some things to keep in mind if your blog appearance isn't going as you planned:

**Problem:  The blog post that I created isn't appearing**

Possible Solutions: 
  - Check your date. GitHub pages won't display blog posts with future dates
  - Check the yaml header.  If there are any special characters in any of the fields, you need to use quotes around the entire field entry.  The most common culprit is the description.  If you're having trouble, try putting quotes around the entire description

---

**Problem:  I know that I made changes to a blog post but the changes aren't appearing**

Possible Solution:
  - Check the header.  If there are any special characters in any of the fields, you need to use quotes around the entire field entry.  The most common culprit is the description.  If you're having trouble, try putting quotes around the entire description.

---

**Problem:  My entire blog has wierd formatting**

Possible Solution:
  - Usually this is an address problem.  Double check your url and baseurl in the _config file