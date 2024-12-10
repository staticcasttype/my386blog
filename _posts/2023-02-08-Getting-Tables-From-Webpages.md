---
layout: post
title:  "How to Get a Table from a Webpage"
author: Stephanie Andersen
description: A League of Legends themed data extraction technique.
image: /assets/images/rell.png
---

# Queueing Up: The Overview

![Figure](https://technology.riotgames.com/sites/default/files/lcu_ui_ready_check.gif)

Today I will give an overview of how to grab an existing table from a website for use in data analysis. This is useful because using already collected data saves time, but as beginner python users we don't know how to deal with data that isn't readily available in CSV form.




# The Draft: Background

![Figure](https://assets.change.org/photos/5/pd/rq/TyPDRqCIMrGqZAB-800x450-noPad.jpg?1589776095)

I'm a huge fan of the popular MOBA game League of Legends. Say I want to analyze the roster of champions with their base stats. Perhaps this data doesn't exist in an easily consumable form already, so what are the ways I could go about obtaining this data?

One way is I can go into the game manually, load up the practice tool on all 162 champions, and record the champions stats at each of the 18 levels with the same runes. This method is perfectly viable, but-like most people in Data Science-I'm lazy. And since you're here, it seems as though you are too. Let's consider other options.

![Figure](https://i.stack.imgur.com/J66uz.png)

Perhaps you can find the information online. Marvelous! It's even sorted out...

![Figure](https://i.imgur.com/ngq1E56.png)

...but it is not a downloadable csv file. We could copy each cell of the data frame and create our own replica CSV. But I haven't forgotten to be lazy. If only there were a way we could extract this table directly from the website...




# The Laning Phase: The Libraries You Need

![Figure](https://miro.medium.com/max/1200/1*gON3peBAScOzoTRjZndloQ.jpeg)

You will need two libraries to extract a table from a webpage. You will need the *pandas* and the *request* libraries, as shown below.

```
import pandas as pd
import requests
```

### Pandas
A software library written for Python. Enables better data manipulation and analysis.

### Requests
Requests is a HTTP library for the Python programming language. The goal of the project is to make HTTP requests simpler and more human-friendly.





# Teamfighting for Objectives: The Code + Explanation

![Figure](https://i.ytimg.com/vi/cL8PnLrwk-g/maxresdefault.jpg)

```
url = '<insert URL of the webpage here>'

# Make a request to the website and retrieve the HTML content
response = requests.get(url)
html_content = response.content
```

You can use the pandas library in Python to extract tables from a webpage. One way to do this is to use the read_html function from the pandas library, which parses an HTML string and extracts any tables found.

```
# Use pandas to parse the HTML content and extract any tables
tables = pd.read_html(html_content)
```

The `read_html` function returns a list of DataFrames, so you can access the first table like this:

```
df = tables[0]
```


# GGEZ: The Final Product

![Figure](https://cdn.sanity.io/images/ccckgjf9/production/9bb232cf222c6059cc960b9da174277016209bfb-1250x703.png?w=1920&h=1080&fit=max&auto=format)

If you've followed the code above, then you now should have a workable pandas dataframe to manipulate your data with, as shown.

![Figure](https://i.imgur.com/pwcp1ZP.png)

Happy Coding!