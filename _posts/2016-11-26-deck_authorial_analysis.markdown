---
layout: post
title:  "Deck on an Authorial Analysis of the Prime Minister's Speeches"
date:   2016-11-26 10:45:31 +0530
categories: ["Posts", "R"]
comments: true
---


I recently attempted a Stylistic Analysis of the Pakistani Prime Minister's speeches, in order to identify if his speeches exhibit different Stylistic patterns. Large differences would indicate possibilities of different writers.  

Background to this task: Some significant events in Pakistan had prompted the Prime Minister to address the nation. The speech was later leaked on Social Media, and supposedly presented the Prime Minister as discussing diction for his speech with his special aides.  
This was met with criticism by some Paksitani News Channels, which felt that at a time when the government is embroiled in corruption and mismanagement, the attitude depicted in the leaked clip is irresponsible.  

The <a href="https://aliarsalankazmi.github.io/Deck-on-Analysis-of-the-PM-s-speeches/index.html#1" target="_blank">deck</a> linked to, summarises this task I took upon myself.  
The original post can be accessed from <a href="http://aliarsalankazmi.github.io/blog_DA/posts/r/2016/11/18/authorial_analysis_pm.html" target="_blank">here</a>.  
 
The code that goes into this deck is presented below.   
 
<pre>---
---
title       : How Many Writers does the Prime Minister have for his Speeches?
subtitle    : An Authorial Analysis
author      : Ali Arsalan Kazmi
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
--- .break

## Background

--- &vcenter

A series of events in Pakistan prompted the Prime Minister to address the nation...



--- bg:url(http://blogs.reuters.com/great-debate/files/2013/10/pm-sharif.jpg);background-size:cover

<span style="color:white">After the speech, however, an Audio clip was leaked, allegedly of the PM taking advice on diction from his aides</span>

---

## Surprise & Criticism
   

![](http://www.northeastern.edu/careers/wp-content/uploads/2014/07/social-media.jpg)


--- &vcenter

This was followed by another major event...

--- bg:url(https://cdn1.tnwcdn.com/wp-content/blogs.dir/1/files/2016/04/mossackfonseca-1100x735.jpg);background-size:cover


--- &vcenter

Another speech was delivered...

--- &vcenter 

And an Audio clip supposedly for this speech was also leaked...

---

## Questions asked in News Channels & Social Media

>- Does the Prime Minister consult his advisors on diction for every speech or for ones delivered during extraordinary circumstances only?  
>- From his advisors, does the PM rely more on his family members?  
>- Has the PM's reliance on certain family members and aides increased only recently, as his government has come under increasing fire for involvement in large scale corruption?  
>- Is it possible to obtain information of such collaborations for other speeches of the PM?


--- .break

## Motivation

--- bg:url(https://thedailychapter.files.wordpress.com/2012/01/hand-writing-quill-pen.jpg);background-size:cover

--- 

<br>
<br>
<br>
Using Traditional Journalism to identify if different authors are involved in the PM's speeches is likely to be:  
>- perilous
>- time-consuming

Data Journalism, on the other hand, can attempt to answer this question relatively comfortably with:  
>- Stylistic Analysis/<a href="https://en.wikipedia.org/wiki/Stylometry" target="_blank">Stylometry</a>

--- .break

## Procedure

--- 

<br>
<br> 

>- Scrap Speech Data from the <a href="http://pmo.gov.pk/pm_speeches.php" target="_blank">PM Office's website</a>
    - English Speeches
    - Around 60 in total
>- Clean Data
    - Semi-automatic
>- Apply Stylometric Techniques
    - Various
    - <a href="https://sites.google.com/site/computationalstylistics/projects/testing-consensus-networks" target="_blank">Boostrap Consensus Network</a> (due to Professor <a href="https://sites.google.com/site/computationalstylistics/home" target="_blank">Eder</a>)
    - Visualise
>- Details included <a href="http://aliarsalankazmi.github.io/blog_DA/posts/r/2016/11/18/authorial_analysis_pm.html" target="_blank">here</a>

--- .break

## Datasets

---

## Datasets

Different datasets genereted using:

>- Parts of Speech (POS) Tagging
>- Word Bigrams
>- Character 4-grams


--- .break

### Bootstrap Consensus Network using POS Bigrams

---

![](http://aliarsalankazmi.github.io/blog_DA/assets/img/bcn_pos2a_small.png)

--- .break

### Heatmap using Most Frequent Word Bigrams

--- 
![](http://aliarsalankazmi.github.io/blog_DA/assets/img/heatmap_mfwbi_1_slide.png)


--- .break

### Bootstrap Consensus Network using Most Frequent Character 4-grams

--- 
![](http://aliarsalankazmi.github.io/blog_DA/assets/img/bcn_char4g_1.png)


---

## Two Authors?

<br>

>- Almost all analyses point to two separated clusters of speeches
>- Does depend significantly on amount of data
>- Indicates there may be two (or more) authors
>- Blog post can be accessed <a href="http://aliarsalankazmi.github.io/blog_DA/posts/r/2016/11/18/authorial_analysis_pm.html" target="_blank">here</a>

---</pre>




