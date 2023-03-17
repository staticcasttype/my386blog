---
layout: post
title:  "How to create a blog post"
author: Shannon Tass
description: A sample post instructions on how to create a new blog post
image: 
---

## Steps for creating a new post.  

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