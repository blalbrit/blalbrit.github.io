---
layout: post
title: DLCL122-2016 - Liz's Blog Post, Exercise 4
date: '2016-02-14'
tags: DLCL122-2016
author: lizfischer
---

#Exercise 4: Comparing

Structure
```&lt;div1 type="book" n="2"&gt;
	&lt;div2 type="chapter" n="1"&gt;
		&lt;p n="1"&gt;
		&lt;/p&gt;
		&lt;p n="2"&gt;
		&lt;/p&gt;
	&lt;/div&gt;
	&lt;div2 type="chapter" n="2"&gt;
	&lt;/div&gt;
&lt;/div&gt;```

Bad

```xml
<app>
	<rdg wit="#stanfordMS"> filius cham[.]</rdg>
	<rdg wit="#monumenta.ch">, filius Cham,</rdg>
	<rdg wit="#parkerMS">filius cham[.]</rdg>
</app>```

Good
	<app>
		<rdg wit="#stanfordMS">iafeth dictus</rdg>
		<rdg wit="#monumenta.ch">Iaphet dictus</rdg>
		<rdg wit="#parkerMS">iapheth dict[us]</rdg>
	</app>
