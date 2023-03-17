---
layout: post
title:  "Data Collection: Vision Score and Win Condition"
author: Shannon Tass
description: I used the Riot Games API to collect match data on both low elo and professional LoL players to see if we could explore any correlation between vision scoring and win conditions.
image: /assets/images/ward.png
---

# Introduction

The competitive world of League of Legends is a game of strategy, teamwork, and skill. As any player knows, vision control is an integral part of winning games, and the game provides a metric for measuring it: the Vision Score. In this blog post, we will explore whether there is a correlation between Vision Score and winning games. To do this, we collected data from Riot Games' API and will analyze it in later blog posts. Today, we will discuss the steps we took to collect and process the data, and later, the insights we uncovered and the implications for players looking to improve their chances of winning. Let's dive into the world of data analysis and League of Legends!

![Figure](https://prod.assets.earlygamecdn.com/images/League-ward-guide.png?transform=article_webp&x=0.5&y=0.5)

# Data Collection

## Tools

I used Python to complete this project as well as the Requests package, and the RiotWatcher package made specifically to call data from Riot's API.

## Step 1: Obtain the API Key

It's easy to obtain an API key from Riot Games. At the site https://developer.riotgames.com/ you can request an API key that will expire 24 hours after you request it. So over the course of a week I've had to request a new one a couple of times.

![Figure](https://apipheny.io/wp-content/uploads/2020/04/1-6.jpg)

You can also find all the different endpoints available at https://developer.riotgames.com/apis. This allows you to find the code to query it yourself, or you can even use it to generate the information directly for you if you want the output fast.

## Step 2: Setting Up Your Code

You will need to perform an install of the RiotWatcher package.

```
pip install riotwatcher
```

Then you will need the following libraries.

```
from riotwatcher import LolWatcher, ApiError
import pandas as pd
import requests
```

## Step 3: Get Account Data

Since Riot doesn't let you pull all accounts (nor do we want that) you must preselect the accounts that you want to review their match history. I've chosen 10 accounts we will pull 20 games each from for a total of 200 matches (observations) to look at. 

The accounts are a mix of high elo and low elo, or more simply, good players and bad players.

To get this data, you need to request the selected accounts' PUUIDs, a unique identifier linked to eaah account. You can do this with your account, I am just using my account name, 'staticcasttype'. Notice I directly included the API key I used because it is expired already.

```
api_key = 'RGAPI-b011ea28-c1b1-4d7b-8dca-0426325bb69a'
watcher = LolWatcher(api_key)
server = 'na1'
summoner_name = 'staticcasttype'

summoner = watcher.summoner.by_name(server, summoner_name)
puuid = summoner['puuid']
```

After obtaining the PUUIDs for each account, I made a dictionary of the summoner username, the PUUID, and their region, so I could iterate through each account and put their information into a data frame easier and more quickly than doing that separately. However, since the Riot API limits your requests, it is a good idea to break up your data so you don't get invalid request returns. I divided my accounts between low and high elo.

```
summoner_info_amateur = {0 : ['staticcasttype', 'smgDcZQRcPVnIJJUIdn-cyLhMtOzWW1pYub1fkK_inI8_Wh2hFK7q3deneMNRnYxhMa5Pvw-u9MGdQ', 'americas'],
                  1 : ['binchicken', 'vZOoJBb5_Wy6LJCr0jcS4GWfdg9CH5gBJJDEHdCBMLM5vjUSARdZ4oWH-6wqjCs-rxlIusxDIFUFHQ', 'americas'],
                  2 : ['Ryechus', 'HDC_2SprVQSVjgRt3rOJnY-tM_xenBR76x0fb1P5KODbLcN0ZPiI3zZJ1a5UkJXE8pqRxmX_AJQ3eQ', 'americas'],
                  3 : ['alphabetcat', 'hTuGHUYHBLnVZzV_XzBhTbVS1AkbXIttIxB6UsLat58TN3OuchROJHTHxeHL31Nz2GmsL8TrhiXleg', 'americas'],
                  4 : ['white ibis', 'CcEZ4CeuM5hZn9GQW_QUKkljXhHkQlzJ6mOGA36urZ9QlRi33OI7I9H4oTUoCxQ_v-BLThQQy3m6JA', 'americas'],
}
```
I also made a list of all the PUUIDs for easier reference later on.

```
puuids = ['HXXcmPG5mKETeTvMkA_T3rv5gKGhz8wogsCUWv0aNdKKrSBKe20_NokHFi3KUqexkA-Jsmqr7KRohg', '9mTvANPkfm_ehL1RWAavzJZ7Ctd9xGnThpgN8QZLISdr-KMEFPK_c3sorNJ7SSMnkxDmhQpJLUhETA',
          'NHvBUFjBoPkeg2YNKtbGnZAJ57tr5RanqJksuMely_FoK8HETjCht6J4utQOD8gbvBOKP0Gqg4r1PQ', 'JVoKe_2_L1Jz4Yosn2nyQJcDR0xF1jvZaCj9Sa9TKsy72G_8wsGmT9yIzyUYtkHqMsvcbQ3sA2Onzw'
          '2hYuhnqt0QGURvf_ij-cK2dZambeerWYG29To0PxXKBBz36lP_zALzT9q80fMLfiJilZB0Yri4rTqg', 'smgDcZQRcPVnIJJUIdn-cyLhMtOzWW1pYub1fkK_inI8_Wh2hFK7q3deneMNRnYxhMa5Pvw-u9MGdQ',
          'vZOoJBb5_Wy6LJCr0jcS4GWfdg9CH5gBJJDEHdCBMLM5vjUSARdZ4oWH-6wqjCs-rxlIusxDIFUFHQ', 'HDC_2SprVQSVjgRt3rOJnY-tM_xenBR76x0fb1P5KODbLcN0ZPiI3zZJ1a5UkJXE8pqRxmX_AJQ3eQ',
          'hTuGHUYHBLnVZzV_XzBhTbVS1AkbXIttIxB6UsLat58TN3OuchROJHTHxeHL31Nz2GmsL8TrhiXleg', 'CcEZ4CeuM5hZn9GQW_QUKkljXhHkQlzJ6mOGA36urZ9QlRi33OI7I9H4oTUoCxQ_v-BLThQQy3m6JA']
```

## Making Match ID Requests With Account Data

Next, I made two, or however many dictionaries you made to break up request times, separate lists to store the matchIDs.














1. Create a new file in the "_posts" folder called 'YYYY-MM-DD-post-name.md', where YYYY is the year (2023), MM numeric month (01-12), and DD is the numeric day of the month (01-31).  The "post-name" is a short name for the new post with - between words.  **You must use this name convention for all new posts.**  

2.  Make the YML heading.  All pages in the site need to start with a YML heading.  For posts you should use
```
---
layout: post
title:  "Post Name"
author: Your name
description: Short yet informative description
image: /assets/images/blog-image.jpg
---
```
Note that the layout should stay as "post", but all the other fields you need to update with the information for your particular blog post.  The blog image should be a .jpg or .png file that you should add to the folder "assets/images".  Don't make it too large or the page will take longer to load (500-800 KB is a good size).  The file path should be okay to leave as "/assets/images/" in the header area.  

3.  Write the body of the blog using markdown.  For more information about writing with markdown, see the following 
* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
* [Markdown Guide](https://www.markdownguide.org/cheat-sheet/)
* [GitHub Flavored Markdown Spec](https://github.github.com/gfm/)

## Adding Images
Images for the blog will generally but put into the 'assets/images' folder.  You can aslo create a subfolder for each blog post if you'd like to keep your figures organized.  **I've found that in the body of the post, it works better to use a url pointing to the figure's location.**  In order to find the appropriate url, navigate to the figure in the repository and find the "download" url.  The correct url will typically have the work "raw" either at the beginning or as `/raw/` in the middle somewhere. For example:

```
![Figure](https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg)

OR

![Figure](https://github.com/esnt/my386blog/raw/main/assets/images/default.jpg)
```

![Figure](https://raw.githubusercontent.com/esnt/my386blog/main/assets/images/default.jpg)

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