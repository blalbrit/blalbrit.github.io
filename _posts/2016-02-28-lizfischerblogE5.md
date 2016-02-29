---
layout: post
title: DLCL122-2016 - Liz's Blog Post, Exercise 5
date: '2016-02-28'
tags: DLCL122-2016
author: lizfischer
---

#Exercise 5: Comparing
## Getting the text
[online text](https://www.mun.ca/rabanus/text.html) 

[Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/)

[Full scraping script here](https://gist.github.com/lizfischer/cbf4c71eea8be043368a)

## Cleaning the text
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

## Analysing the text
I found two lists of stop words via Google ([[1]](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin) [[2]](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin))

![word cloud](http://i.imgur.com/zypqWY3.png)

![distinctive words](http://i.imgur.com/ZuFem5T.png)
