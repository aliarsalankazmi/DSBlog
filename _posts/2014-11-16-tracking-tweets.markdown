---
layout: post
title:  "Tracking Tweets' Geographically"
date:   2014-11-16 19:45:31 +0530
categories: Posts
---
Muharram is a month that Shi'ite Muslims observe with sorrow and grief, mourning for the Leader of the youth of Paradise, [Imam Hussain](https://en.wikipedia.org/wiki/Husayn_ibn_Ali), who was martyred along with his family members at Karbala. Hussain, the grandson of the Holy Prophet of Islam, was wronged, oppressed, harassed, abandoned, and killed by Muslims at the 10th day of Muharram, in the [battle of Karbala](https://en.wikipedia.org/wiki/Battle_of_Karbala).  

The battle of Karbala represents the pinnacle of godliness, painting with the holy blood of Hussain the eternal struggle between right and wrong, and judging the forces of good as victor of this battle with the last prostration of Hussain to the Almighty.


The following is an attempt to track people on Twitter who tweet regarding the battle of Karbala, or Imam Hussain. Our intention was to be able to capture a large amount of tweets, which could then be mapped to their respective locations, starting from the 1st of Muharram until the 10th of Muharram.


However, it has not been quite possible to accomplish this due to the following reason: Most tweeters tweeting about the sacrifice of our Imam had their location-services switched off, meaning that we would be unable to map their location. In this respect, an algorithm was used to offset the longitude and latitude for tweeters with unknown locations. As it were to be, even this did not helped, and eventually we decided to use data only from tweeters who had enabled location tracking. This had the effect of reducing our data set from that of 54,000 tweets to only 1,052.


![alt text][tweetMap]

[tweetMap]: https://desertscrolls.files.wordpress.com/2014/11/twmh.png "Mapping Tweets Pertaining to Muharram/Ashoura"


***

**Credits**

This post was inspired by the wonderful night time [image](http://apod.nasa.gov/apod/image/0011/earthlights2_dmsp_big.jpg) of the world by NASA.

It was also inspired by the [work](http://spatial.ly/2012/06/mapping-worlds-biggest-airlines/) of James Cheshire, seeing which I realised how easy it was to produce such an image as displayed above in R.

***

InshaAllah, this could be a starting point for more, wonderful images produced by us.

A link to the code used in extracting data from Twitter, and for producing the above graphic can be found [here](https://github.com/aliarsalankazmi/tweeters-Muharram-Ashura). Please note that due to restrictions placed by Twitter on their data, it may not be possible to extract tweets for past dates.



*God’s help do we seek, and success is only through Him.*

