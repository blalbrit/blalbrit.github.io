---
layout: post
title: DLCL122-2016 - Liz's Blog Post, Exercise 5
date: '2016-02-28'
tags: DLCL122-2016
author: lizfischer
---

#Exercise 5: Comparing
For Exercise 5, we looked at a corpus of text in the [Voyant Tools](http://voyant-tools.org/). I chose to do the entirety of the text [this fragment](https://searchworks.stanford.edu/view/9932113) is from, "De rerum naturis". For me, this was a three step process.
### Getting the text
In a previous exercise, I found an [online edtion](https://www.mun.ca/rabanus/text.html) of "De rerum naturis". There are 22 books and 2 prefaces; that isn't all that much text to copy/paste/save by hand, but I'm a computer scientist so I must [automate](http://www.xkcd.com/974/) [whenever](http://www.xkcd.com/1319/) [possible](http://www.xkcd.com/1205/). So I wrote a Python script to pull down all the text for me!

My favorite library for simple web scraping is absolutely [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/). It's easy to pick up and works wonderfully on (at least mostly) well-structured pages, like the one I was dealing with. 

You can see and download my full scraping script [here](https://gist.github.com/lizfischer/cbf4c71eea8be043368a).

### Cleaning the text
When I took a peek at my 
numerical references between number signs (#) are to manuscript folios
```python
# Remove text between '#'s, and numbers
def stripCharacters(text, outfile):
    f = open(outfile, "wb")
    outtext = re.sub('\#\w*#', '', text)
    outtext = re.sub('\d', '', outtext)
    f.write(outtext)
    f.close()
```

### Analysing the text
I found two lists of stop words via Google ([[1]](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin) [[2]](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin))

![word cloud](http://i.imgur.com/zypqWY3.png)

![distinctive words](http://i.imgur.com/ZuFem5T.png)
