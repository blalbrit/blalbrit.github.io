---
layout: post
title: DLCL122-2016 - Liz's Blog Post, Exercise 4
date: '2016-02-14'
tags: DLCL122-2016
author: lizfischer
---

#Exercise 4: Comparing

In this exercise, we were charged with the task of preparing two versions of a text for the Versioning Machine. The text I marked up was from a manuscript fragment of Rabanus Maurus' "De rerum naturis", which I previous transcribed in T-PEN. The second version of the text I used was an online edition which I used to assit my transcription. The turned out to be a mistake. Because I used the edition text to help expand the Latin abbreviations, and because the edition text was not at all abbreviated, the "differences" in the text were for the most part simply places where something had been abbreviated in the manuscript text. This is much less interesting than comparing two manuscript witnesses would be. So, I found the same fragment of text in a manuscript at the Parker library (CCCC MS 11) and transcribed *it* in T-PEN, and finally adding a third witness to the TEI document for Versioning Machine.

### Document Structure

The fragment is from book 2, chapters 1 and 2. I used these as the top two levels of organization for the TEI encoding. Getting a breakdown smaller than that was not so clear, as there are no "paragraphs" in the Stanford manuscript. There are, however, periodic red-filled capital letters. The Stanford manuscript is the one I care about most, so I chose the red letters as a way to further decompose the text despite it seeming arbitrary from the point of view of the other two witnesses. 

```xml
<div1 type="book" n="2">
	<div2 type="chapter" n="1">
		<p n="1">
		</p>
		<p n="2">
		</p>
	</div>
	<div2 type="chapter" n="2">
	</div>
</div>
```

### Defining Difference

As I first did the Stanford MS and the edition text before adding the second manuscript, the only differences in the texts were abbreviation, capitialization, and punctuation differences. In order to have differences to look at, I counted all three of these things as proper differences. When I added the second manuscript witness, it produced instances like this:

```xml
<app>
	<rdg wit="#stanfordMS"> filius cham[.]</rdg>
	<rdg wit="#monumenta.ch">, filius Cham,</rdg>
	<rdg wit="#parkerMS">filius cham[.]</rdg>
</app>
```
To me, this seemed pretty uninteresting. The abbreviation differences in the manuscripts was the truly interesting piece. Differences in capitialization and punctuation occured fairly frequently, and were detracting from the abbreviation differences. For example:
```xml
<app>
	<rdg wit="#stanfordMS">iafeth dictus</rdg>
	<rdg wit="#monumenta.ch">Iaphet dictus</rdg>
	<rdg wit="#parkerMS">iapheth dict[us]</rdg>
</app>
```

Due to time constraints, I have not yest revised the document to reflect my new feelings on what constitutes a textual difference in this text. That's the other thing about TEI encoding-- it takes *forever* to do and, to the computer scientist in me, looks like something which cries out for automation.
