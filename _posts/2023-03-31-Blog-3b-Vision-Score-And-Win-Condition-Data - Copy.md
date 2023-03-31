---
layout: post
title:  "EDA: Vision Score and Win Condition"
author: Stephanie Clark
description: Using data collected through the Riot Games API, I perform a brief EDA of our data to search for insights.
image: /assets/images/lolmap.png
---

# Introduction

Welcome back! In our previous blog post, we discussed the importance of Vision Score in League of Legends and the steps we took to collect match data from Riot Games' API. In this post, we will delve deeper into the data and present our findings after conducting some exploratory data analysis. We will explore various aspects of the data, such as the average Vision Score and win rate by team position and Vision cd 3  Score range. By the end of this blog post, we hope to provide valuable insights into the relationship between Vision Score and winning games, which can be beneficial for players looking to improve their chances of success in League of Legends. So let's jump into the exciting world of data analysis and League of Legends!

# Data

## Figure 1: Correlation Matrix
/assets/images/corrmatrix.png

First off, I wanted to start with a correlation matrix to see if there was anything immediately unique about the data. Typically, vision score is good in games, so I was expecting each metric of vision to be positively associated with winning- however, you can see that the 'wardsKilled' metric is actually slightly negatively correlated with a win. My sample size is extremely small, but this still surprised me. I hypothesize that this could be because of players who prioritize taking down enemy wards even when they are not in a safe place, leading to a gank, a death, and gold for the enemy team.

## Figure 2: 
![Figure](/assets/images/hist.png)

Next, I was interested in the counts of each variable and thought a visual representation would be nicer than a printed list of counts. This helps you to visualize what the vision score for a typical game might look like. For example, on the first axis, the 'visionScore' histogram, we can see a peak around 20, meaning that 20 was the vision score we saw in just over 50 of the matches recorded in the dataset. SImilarly, the number of wards bought in the game on the upper right axis, 'visionWardsBoughtInGame', there were over 100 games where the player did not buy any vision wards. I suspect that this is largely skewed by the amount of non-support (utility) roles that I included in my data, since vision is largely the responsiblity of the support (utility) player.


# Reflection

There are some missing values in the data that need to be cleaned, which will put us considerably under the 200 observation expectation for this project, but this can easily be solved by throwing in a few additional summoner accounts to gain new observations from. Since I coded this extraction technique to work well with lists of any size, and multiple starting dictionaries (username/PUUID/region), it would be no problem to introduce additional observations into the data.

In my next post, I'll do a little bit of analysis on the data to see just how impactful vision is in games. As a support main, I care a lot about warding, and it is one of my top priorities when I play, so I'm interested to see if what I lack in DPS, I more than make up for with map control and itemization.

Through this portion of the project, I learned that there is a lot you can do with Riot's API. Please consider using my method (or let me know of other ways) to collect other information from matches for data analysis of what elements are conducive to game-winning conditions.
