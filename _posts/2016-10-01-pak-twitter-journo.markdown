---
layout: post
title:  "Watching the Watchers - An Analysis of Pakistani Journalists using their Twitter Data"
date:   2016-10-1 15:45:31 +0530
categories: ["Posts"]
comments: true
---


Journalism in Pakistan has, in the recent years, become quite a lustrous industry, with a considerable fan following which mimics that of the Film and Drama Industry. Indeed, some Pakistani journalists are admired and followed passionately by large segments of our society, putting a lot of (extra) power in their hands, insofar as steering public perception of various topics is concerned.   

With great power, comes great responsibility, and journalists are, thus, expected to be impartial in their analyses. Important to stress here, I think, is that impartiality should not denote neutrality _to_ truth - that would be a disservice to public. Scholars like [Edward S. Herman](https://en.wikipedia.org/wiki/Edward_S._Herman) and [Noam Chomsky](https://en.wikipedia.org/wiki/Noam_Chomsky) come to mind who have [criticised](https://en.wikipedia.org/wiki/Propaganda_model) such a notion of [Journalistic Objectivity](https://en.wikipedia.org/wiki/Journalistic_objectivity).  

But can the presence of journalists on Social Media, where they share their views freely, be utilised to find out their inclinations? Or can such data be used to find out anything that might be beneficial to know?  
For this post, I have extracted data of most notable Pakistani journalists from their Twitter interactions, to see if any interesting insights can be gleaned.  




### Basic Analyses  

To begin, we may look at the number of followers and the number of people followed by Pakistani journalists.

<a href="{{ site.baseurl }}/assets/img/j_graph1.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph1.png"></a>

We can see outliers in the graph, some of whom I have highlighted. Hamid Mir and Mubasher Lucman have the highest number of followers whilst they follow a low number of people. Other, similar journalists are Shahzeb Khanzada, Kamran Khan, Kashif Abbasi. On the other hand, Raza Rumi follows the highest number of people with relatively less number of people following him. Journalists similar in this respect are Shaheen Sehbai, Shahid Masood, Asad Kharal.  

It may also be interesting to see who are the people that the aforesaid Pakistani journalists follow (onwards, such people will be called 'friends'). In the following graph, I plot the percentage of 'friends' that are Pakistani journalists from the same group that is in consideration. In other words, for each journalist, what is the percentage of friends that are journalists from the same set of people.  

<a href="{{ site.baseurl }}/assets/img/j_graph2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph2.png"></a>

It emerges that Ansar Abbasi, Babar Sattar, Wajih Sani, follow the highest number of people from the same list of Pakistani journalists. Specifically, about 38% of friends of Ansar Abbasi's are Pakistani journalists that we are currently analysing. In contrast, Ahmed Arif Nizami, Dr. Danish, and Raza Rumi follow the least number of from our list of journalists.  


If we look at this data from another perspective, and ask, "Which of these journalists are followed by most other Pakistani journalists", the following graph can help find an answer.  

<a href="{{ site.baseurl }}/assets/img/j_graph3.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph3.png"></a>


We see that Shaheen Sehbai, Raza Rumi, and Azhar Abbas are the journalists who are friends to (that is, are followed by) most other Pakistani journalists. Dr. Danish, Ali Salman Alvi, and Ahmed Arif Nizami appear to have the least number of Pakistani Journalist followers. Of course, this can be influenced by several factors, such as the visibility and activity of a journalist.  

To get a sense of how active a journalist is - where active is defined in terms of tweets per day - we generate the following graph:  

<a href="{{ site.baseurl }}/assets/img/j_graph4.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph4.png"></a>

Here, we see *Zarrar Khuhro* leading with almost 90 tweets a day (!), followed by Raza Rumi and Abbas Nasir, who generally tweet around 40 tweets a day. Ahmed Arif Nizami, Saleem Safi, and Nadeem Malik are found at the end of the trail.  






### Visualising Pakistani Journalists on Twitter as a Network

Taking advantage of the list of people who each journalist follows, we can plot this relationship as an edge/straight line connecting the various vertices (or points) that are journalists. This Network graph can be visualised in a [way](https://en.wikipedia.org/wiki/Force-directed_graph_drawing#CITEREFFruchtermanReingold1991) such that the most highly connected nodes/points are placed at the centre, and the less connected nodes at the outer areas.  

<a href="{{ site.baseurl }}/assets/img/j_graph5.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph5.png"></a>

The graph above places certain journalists, like Dr. Danish, away from the core of the graph, indicating that such journalists have only a few connections with other journalists, as was also shown in one of the graphs prior to this one.  
In the present case, however, the graph does not provide very useful insights - in cases as this, an alternative graph that _may_ be useful is a [Hive plot](http://mkweb.bcgsc.ca/linnet/), or a [BioFabric plot](http://www.biofabric.org/gallery/pages/SuperQuickBioFabric.html).  







### Correspondence Analysis: Do any Journalists follow more people from a particular Political Party?

Since our data contain the 'friends' of all journalists in consideration, we can split the friends by their affiliation to any of the 3 Political parties in Pakistan (PML - N, PTI, PPP), and perform [Correspondence Analysis](https://en.wikipedia.org/wiki/Correspondence_analysis) on the counts' data. Perhaps with this we can see some inclinations/interests of journalists to/for a political party, if they follow members from one party exclusively.  

<a href="{{ site.baseurl }}/assets/img/j_graph6.2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph6.2.png"></a>
  
The analysis summarises the information in our data set in a two-dimensional graph, the horizontal demarcation (supported by horizontal dotted line) of which accounts for 87% of the information, whilst the vertical accounts for 9%. We see that the horizontal line separates Political parties from Journalists, and the vertical line separates 'Others' (all friends who are neither politicians nor journalists) from Politicians and Journalists.   

Since most information is explained by the horizontal separation, however, we shall explain the graph in terms of Political parties and Journalists: Journalists like Ansar Abbasi, Babar Sattar, and Wajih Sani are present below the horizontal line, in the extreme end of the 'Journalist Domain'. This indicates that these tweeters follow Pakistani journalists more than they follow any political figures. On the other hand, Kashif Abbasi is close to the 'Political Domain', indicating that he may have more political figures as friends, in comparison to other journalists. We also see that Dr. Danish, Ahmed Arif Nizami, and Talat Hussain cluster around the 'Others Domain', indicating that these follow neither many Politicians nor a lot of Journalists.  

We can remove people from the 'Others' category so that we only end up with people followed by our journalists who are either journalists or politicians.  


<a href="{{ site.baseurl }}/assets/img/j_graph7.2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph7.2.png"></a>

This time, the horizontal separation explains 62% of information, separating PTI from PML-N and PPP, whereas the vertical separation explains 27% of information, separating all political parties from journalists. It is apparent that Dr. Danish and Ahmed Arif Nizami follow more people from the PTI, in comparison to others. In contrast, Gharidah Farooqi and Cyril Almeida seem to group close to the 'PPP Domain'. Others like Kashif Abbasi seem to group close to domains of PPP and PML N, but also close to Journalists (in comparison to others). Other journalists like Wusat Ullah Khan and Wajih Sani are close to the Journalist domain, indicating they mostly follow journalists rather than political figures.  







### Sentiment Scoring - What sentiments are expressed in tweets liked by each Journalist?

So far we have analysed quantitative data about our journalists. We can conduct a basic Sentiment analysis on tweets liked by each journalist to see if these are generally of a negative or positive sentiment. This will be basic insofar as it will be a binary matching of words in the liked tweets to determine if more words appear in a list of 'negative words' (such as bad, poor, etc.) or 'positive words' (great, awesome, etc.).  
_A caveat, here, is that our data on liked tweets has been limited to 200 most recent tweets._  


<a href="{{ site.baseurl }}/assets/img/j_graph9.jpeg" target="_blank"><img src="{{ site.baseurl }}/assets/img/j_graph9.jpeg"></a>


And we achieve a slightly surprising result: journalists like Waseem Badami, Hassan Nisar, Mubasher Lucman appear to like recent tweets that are positive, whilst Raza Rumi, Murtaza Ali Shah, Moeed Pirzada, and Zarrar Khuhro have been liking tweets that are negative, lately.  
*These data were collected during the Uri incident.*  







### Topic Modelling

Now that we know, generally, which journalists have been recently liking positive and negative tweets, it will be interesting to find out what these tweets are about. [Topic modelling](https://en.wikipedia.org/wiki/Topic_model) is technique that can find out associations between various words and extract the key words out of these, trying to provide some sense to the analyst about what the contents/topics of these tweets are. This is a sophisticated, Mathematical technique, which won't be explained in this document due to the limited scope of this post.  
It is also noteworthy that although Topic modelling as an exploratory analysis tool requires us to carefully choose the number of topics we need to identify, we have chosen this number quite arbitrarily, just to demonstrate its utility in a quick fashion.   

Below, we present 7 topics identified for each journalist who were present at the extreme ends of our Sentiment graph:  


#### Moeed Pirzada

<a href="{{ site.baseurl }}/assets/img/Moeed Pirzada_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Moeed Pirzada_wc.png"></a>

#### Murtaza Ali Shah

<a href="{{ site.baseurl }}/assets/img/Murtaza Ali Shah_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Murtaza Ali Shah_wc.png"></a>

#### Raza Rumi

<a href="{{ site.baseurl }}/assets/img/Raza Ahmad Rumi_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Raza Ahmad Rumi_wc.png"></a>

#### Zarrar Khuhro

<a href="{{ site.baseurl }}/assets/img/Zarrar Khuhro_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Zarrar Khuhro_wc.png"></a>

#### Fahd Hussain

<a href="{{ site.baseurl }}/assets/img/Fahd Hussain_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Fahd Husain_wc.png"></a>

#### Hassan Nisar

<a href="{{ site.baseurl }}/assets/img/Hassan Nisar_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Hassan Nisar_wc.png"></a>

#### Meher Abbasi

<a href="{{ site.baseurl }}/assets/img/Meher Bokhari_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Meher Bokhari_wc.png"></a>

#### Waseem Badami

<a href="{{ site.baseurl }}/assets/img/Waseem Badami_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Waseem Badami_wc.png"></a>

#### Nasim Zehra

<a href="{{ site.baseurl }}/assets/img/Nasim Zehra_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Nasim Zehra_wc.png"></a>

#### Shaheen Sehbai

<a href="{{ site.baseurl }}/assets/img/Shaheen Sehbai_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Shaheen Sehbai_wc.png"></a>

#### Mubasher Lucman

<a href="{{ site.baseurl }}/assets/img/Mubasher Lucman_wc.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/Mubasher Lucman_wc.png"></a>


It is interesting how much information about our journalists can be gleaned from Twitter data alone.  







### Conclusion

Whilst Journalism has been flourishing in Pakistan in its traditional form, [Data Journalism](https://en.wikipedia.org/wiki/Data_journalism) has been less prevalent. The utility of Data Journalism can, however, not be ignored, as seen in this blog post. And although the current post focused on Journalists alone, it is indeed possible to gain some insights on Political figures in Pakistan, by analysing their data.  


It is viable. There certainly is potential in it. And perhaps it is a need of the hour too.


