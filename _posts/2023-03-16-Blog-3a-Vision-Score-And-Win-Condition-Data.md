---
layout: post
title:  "Data Collection: Vision Score and Win Condition"
author: Stephanie Clark
description: I used the Riot Games API to collect match data on both low elo and professional LoL players to see if we could explore any correlation between vision scoring and win conditions.
image: /assets/images/wards.png
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

## Step 4: Requesting MatchIDs With Account Data

Next, I made two, or however many dictionaries you made to break up request times, separate lists to store the matchIDs. For ease of running, I duplicated the loops and changed the names of the lists to match.


```
amateur_matches = []
for user in range(5):
  summoner_name = summoner_info_amateur[user][0]
  region = summoner_info_amateur[user][2]
  puuid = summoner_info_amateur[user][1]
  matchlist_url = f'https://{region}.api.riotgames.com/lol/match/v5/matches/by-puuid/{puuid}/ids'
  params = {'api_key': api_key}
  response = requests.get(matchlist_url, params=params)
  match_ids = response.json()
  print(match_ids) # Use this to check if it is catching the IDs or if you get request timed-out.

  # Use the match IDs to retrieve the details for each match
  for match_id in match_ids:
      match_url = f'https://{region}.api.riotgames.com/lol/match/v5/matches/{match_id}'
      response = requests.get(match_url, params=params)
      match_data = response.json()
      amateur_matches.append(match_data)
```

Which will return a list of matchIDs that will look something like this.

NOTE: Each username will return the 20 most recent games.

```
['NA1_4603604702', 'NA1_4603585297', 'NA1_4603572865', 'NA1_4603418804', 'NA1_4603383476', 'NA1_4603354728', 'NA1_4598815302', 'NA1_4598794292', 'NA1_4598773252', 'NA1_4598748127', 'NA1_4598336448', 'NA1_4598303667', 'NA1_4598273712', 'NA1_4598013655', 'NA1_4597995969', 'NA1_4597969133', 'NA1_4597947847', 'NA1_4595651152', 'NA1_4595613524', 'NA1_4595590485']
['NA1_4603633393', 'NA1_4603559905', 'NA1_4600075100', 'NA1_4600038821', 'NA1_4599988273', 'NA1_4598417241', 'NA1_4598375527', 'NA1_4597533475', 'NA1_4597514492', 'NA1_4597504896', 'NA1_4593476789', 'NA1_4593452337', 'NA1_4593441905', 'NA1_4593418132', 'NA1_4593388278', 'NA1_4593053027', 'NA1_4593028434', 'NA1_4592926754', 'NA1_4592895152', 'NA1_4587257018']
['NA1_4545116508', 'NA1_4545069206', 'NA1_4533195909', 'NA1_4533135946', 'NA1_4531045333', 'NA1_4531032874', 'NA1_4531011847', 'NA1_4493629097', 'NA1_4493586951', 'NA1_4493540400', 'NA1_4478426048', 'NA1_4457448380', 'NA1_4457422789', 'NA1_4457375337', 'NA1_4455467146', 'NA1_4455502730', 'NA1_4455419174', 'NA1_4455434865', 'NA1_4455450913', 'NA1_4455385558']
['NA1_4603656272', 'NA1_4603553086', 'NA1_4603504636', 'NA1_4602058892', 'NA1_4601926967', 'NA1_4600402951', 'NA1_4600367079', 'NA1_4600332625', 'NA1_4600247741', 'NA1_4597876466', 'NA1_4597868737', 'NA1_4596661213', 'NA1_4596622349', 'NA1_4596597858', 'NA1_4596164485', 'NA1_4596028568', 'NA1_4596014443', 'NA1_4595335488', 'NA1_4595313623', 'NA1_4595283734']
['NA1_4600986875', 'NA1_4600944073', 'NA1_4600075100', 'NA1_4600038821', 'NA1_4599988273', 'NA1_4599933620', 'NA1_4599844392', 'NA1_4599772492', 'NA1_4599737705', 'NA1_4598417241', 'NA1_4598375527', 'NA1_4593798123', 'NA1_4593772656', 'NA1_4593476789', 'NA1_4593452337', 'NA1_4593441905', 'NA1_4593418132', 'NA1_4593388278', 'NA1_4593352175', 'NA1_4593334765']
```

Lastly, I'd like to combine all my lists into one list now so I have an easier time accessing information from one place and I'm done making requests.

```
matches = [pro_matches, amateur_matches]
```

## Step 5: Variable Selection and Data Extraction

Since I'm only interested in vision metrics and the win/loss outcome, I've skimmed through all the data recorded in {matches} and decided on a few relevant details. I made a dictionary with empty lists that I can append new data I read into it later.

```
match_dict = {'teamPosition': [], 'visionScore': [], 
            'visionWardsBoughtInGame': [], 'wardsKilled': [], 'wardsPlaced': [],
            'win' : []}
```

And then I will loop through the data in {matches} and dead it into my dictionary.

```
for elo in range(len(matches)):
  for game in range(len(matches[elo])):
    if 'info' in matches[elo][game]:
      for participant in range(len(matches[elo][game]['info']['participants'])):
        if str(matches[elo][game]['info']['participants'][participant]['puuid']) in puuids:
            match_dict['teamPosition'].append(matches[elo][game]['info']['participants'][participant]['teamPosition'])
            match_dict['visionScore'].append(matches[elo][game]['info']['participants'][participant]['visionScore'])
            match_dict['visionWardsBoughtInGame'].append(matches[elo][game]['info']['participants'][participant]['visionWardsBoughtInGame'])
            match_dict['wardsKilled'].append(matches[elo][game]['info']['participants'][participant]['wardsKilled'])
            match_dict['wardsPlaced'].append(matches[elo][game]['info']['participants'][participant]['wardsPlaced'])
            match_dict['win'].append(matches[elo][game]['info']['participants'][participant]['win'])
```

Finally, with {match_dict} all filled out, I will turn it into a data frame with pandas.

```
match_df = pd.DataFrame(match_dict)
```

{match_df} should now output

![Figure]()
