---
layout: post
title: DLCL122-2016 - Liz's Blog Post, Exercise 5
date: '2016-02-28'
tags: DLCL122-2016
author: lizfischer
---

#Exercise 5: Comparing
For Exercise 5, we looked at a corpus of text in the [Voyant Tools](http://voyant-tools.org/). I chose to do the entirety of the text [this fragment](https://searchworks.stanford.edu/view/9932113) is from, "De rerum naturis". For me, this was a three step process. You can see the results in full [here](http://voyant-tools.org/?corpus=1456134952218.8278&stopList=1456724379762wi).
## Process
### Getting the text
In a previous exercise, I found an [online edtion](https://www.mun.ca/rabanus/text.html) of "De rerum naturis". There are 22 books and 2 prefaces; that isn't all that much text to copy/paste/save by hand, but I'm a computer scientist so I must [automate](http://www.xkcd.com/974/) [whenever](http://www.xkcd.com/1319/) [possible](http://www.xkcd.com/1205/). So I wrote a Python script to pull down all the text for me!

My favorite library for simple web scraping is absolutely [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/). It's easy to pick up and works wonderfully on (at least mostly) well-structured pages, like the one I was dealing with. Basically, the flow goes like this: navigate to the page you want to start on, gather all the links in a dictionary which maps the link text to a URL; loop through the links, each time pull all the text out of ```<p>``` tags and save it in a text file.
```python
BASE_URL = "https://www.mun.ca/rabanus/" 

# Soup Helper
def make_soup(url):
    html = urlopen(url).read()
    return BeautifulSoup(html, "lxml")

# Get edition text of a page
def parse_page(link):
    soup = make_soup(link)
    pars = soup.findAll("p")
    text = ""    
    for p in pars:    
        text += p.text
    return text

# Get all the books (dict from name to link)
def get_books():
    soup = make_soup(BASE_URL+"text.html")
    books = dict()
    for l in soup.findAll("a"):
        title = l.contents[0].replace(" ", "") # Remove all spaces from titles
        link = BASE_URL + l["href"]
        books[title] = link
    return books
```

You can see my full scraping script [here](https://gist.github.com/lizfischer/cbf4c71eea8be043368a).

### Cleaning the text
When I took a peek at the files my scraper had acquired for me, I found a lot of text like ```eo quod #39b# 39b post``` (I found out later that these are references to manuscript folios). I knew this would mess with my textual analysis later, since Voyant would count ```#39b#``` as a word. I added a function to my scraper to remove everything between ```#```s and any numbers. This wouldn't take care of everything that was not supposed to be part of the text, but it's close enough for me.

```python
# Remove text between '#'s, and numbers
def stripCharacters(text, outfile):
    f = open(outfile, "wb")
    outtext = re.sub('\#\w*#', '', text)
    outtext = re.sub('\d', '', outtext)
    f.write(outtext)
    f.close()
```

The second task I consider part of cleaning the data was done in Voyant itself. Voyant allows users to apply a [stop word](https://en.wikipedia.org/wiki/Stop_words) fiter to their corpus, effectively removing the most common words of a language from analysis and visualization. The tool comes pre-equipped with stop word lists for many modern languages, but none for Latin. Luckily it allows customization of the lists. I found [two](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin) [lists](https://wiki.digitalclassicist.org/Stopwords_for_Greek_and_Latin) of stop words via Google and imported them into Voyant.

### Analysing the text
No quantitative textual analysis would be complete without a word cloud; luckily, it's one of the default features of Voyant.
![word cloud](http://i.imgur.com/zypqWY3.png)

Now, as you may be able to see, my stop word list was not perfect. Latin is a hard language to compile stop words for, since a single word has so many different forms. The word cloud is not very interesting, that aside. What **is** interesting is one of the less flashy tools modules:

![distinctive words](http://i.imgur.com/ZuFem5T.png)

## Thoughts on Voyant
