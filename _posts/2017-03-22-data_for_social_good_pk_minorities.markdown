---
layout: post
title:  "Data Analytics for Societal Good"
date:   2017-03-22
categories: ["Posts", "R"]
comments: true
---


At my workplace, employees celebrate a month of Data Analysis for Societal good, every year. During this time, we try to help NPOs (Non-Profit Organisations) in gaining insights from their data, for free. Whilst we are engaged in this practice at workplace, I needed to do something at a personal level too. So I thought about analysing attacks on minorities in PK, following which I could contact any NPOs and share insights with them, to help them better raise awareness
.  

One of the minorities in PK are Shia Muslims, who are often targeted for their faith (*although this is, by no means, a problem limited to PK*). Target killing Shia professionals (Doctors, Administrators, Educators, etc.) and detonating bombs in their religious gatherings and marketplaces have [become the norm](http://www.shaheedfoundation.co.uk/shia-killing.asp). To help the bereaved families of the victims of such attacks, [Shaheed Foundation in the UK](http://www.shaheedfoundation.co.uk/projects.asp#fin) and [Pakistan](http://www.shaheedfoundation.org/) have been working on various projects, including those about [Education](http://www.shaheedfoundation.co.uk/projects.asp#edu), [Family Support](http://www.shaheedfoundation.co.uk/projects.asp#fam), 
[Medical](http://www.shaheedfoundation.co.uk/projects.asp#med), and [Micro-finance/Self-fund](http://www.shaheedfoundation.co.uk/projects.asp#fin). 

The Foundation has shared a link to a flatfile database, which was formed by a third party and supposedly captures most of attacks on Shia Muslims across Pakistan from 2012 until Mid 2015 (the data go back to 1970s, but the third party have specifically asked not to use pre-2012 data for Statistical analysis. What follows is my attempt at gaining insights from the data.


### Basic Reporting

<a href="{{ site.baseurl }}/assets/img/sk_mar17_provComp_g1.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_provComp_g1.png"></a> 

Graph above shows the number of Shias killed and injured in attacks for each province in Pakistan. Clearly, the Sindh has the highest number of Shias killed, followed by Balochistan, although for number of injuries, Balochistan ranks higher than Sindh. Also for injuries, we see other provinces like Punjab, FATA, and Khyber Pakhtunkhwa have similar figures, although they vary for number of deaths.
Figures from Sindh are unsurprising, as one of its main city has been notorious for targeted killings, until the army took action recently against terrorists. On the other hand, the high number of injuries in Balochistan can be attributed to the large number of bomb detonations and suicide attacks.  


<a href="{{ site.baseurl }}/assets/img/sk_mar17_provComp_g2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_provComp_g2.png"></a> 

If we calculate the proportion of killings and injuries for each province, we see that Gilgit Baltistan and Sindh have had more killings than other provinces during 2012-2015.  


<a href="{{ site.baseurl }}/assets/img/sk_mar17_dist_g3.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_dist_g3.png"></a> 


We can plot the distribution of killings (excluding injuries) for each province with a Violin plot, with a white point superimposed, showing the mean number of killings. We can further break down the violin plots for quartiles to see how the data distribution varies within each, whilst showing the raw data points in white and the mean number of killings in yellow.  


<a href="{{ site.baseurl }}/assets/img/sk_mar17_dist_g3.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_dist_g3.png"></a> 
<a href="{{ site.baseurl }}/assets/img/sk_mar17_dist_g4.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_dist_g4.png"></a> 


We can see that Sindh and Balochistan have the highest number of killings in Quartile 1, although the mean number of killings is highest for Balochistan in each quartile (except in Quartile 2, wherein it is the second highest). For Sindh, most of the data points actually fall within the 1st Quartile.    


It is often the case that during Religious festivities or gatherings, attacks on Shias are reported to increase. Muharram is such a month when Shia Muslims gather in large numbers, so it may be that they are increasingly targeted during this time. Following is a graph that shows a time series of Shia killings with the month of Muharram superimposed from each year (2012 - 2015).  

<a href="{{ site.baseurl }}/assets/img/sk_mar17_ts_g5.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_ts_g5.png"></a> 


After superimposing Muharram months, only Punjab and Khyber Pakhtunkhwa stand out as provinces where at least one spike in number of killings is seen during Muharram. But the chart above only accounts for the number of killings. We can, instead, sum up the number of Shia Muslims killed and injured to calculate the number of casualties, and then subtract each month's 'distance' from the month of Muharram to see if attacks increase close to or during the month of Muharram.  

For this, I first generated a simple time series plot, as shown below.  

 
<a href="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6a0.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6a0.png"></a> 


But I felt the above was slightly difficult to look at, because I wanted to present the information in a different way: Muharram is the 1st month of the Islamic calendar (which comprises of 12 months), and I needed to show the flow of months such that the 2nd or 3rd months are as close to Muharram as the 11th or 12th months. This was possible to achieve if we changed to polar coordinates, ending up with the following circular time-series plot.  


<a href="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6b.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6a.png"></a> 


With this plot, I think it becomes easier to spot any surge or drop in the GAM smoother. As it appears, Punjab, Sindh, and FATA do appear to have more data points in or close to Muharram, with the smoother slightly shifting up in the case of Punjab. We can further split the smoother on years.  


<a href="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6d.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_cts_g6c.png"></a> 


Now, it appears that for years prior to 2014, casualties in or close to Muharram were higher than they were in 2014. This is not applicable to Sindh, however, as we see that the trend remains constant throughout the years, with very little difference (if any) in the number of casualties.  


### Inference: Permutation Testing 

I was recently introduced to Permutation Testing whilst reading the [Modern Dive - An Introduction to Statistical and Data Sciences via R](https://ismayc.github.io/moderndiver-book/7-hypo.html). Having specialised in Data Mining, I had not had to dwell so much on checking assumptions for normality, learning the various distributions, etc. - the focus would be on prediction rather than inference. But Permutation Testing and Bootstrapped Confidence Intervals were intuitive to me, so I took to reading about it.  

Following my reading, I thought about comparing the mean number of casualties from Muharram and non-Muharram months, assuming an exchangeable sequence of random variables (or that future samples will be similar to the present sample).  

Null Hypothesis: There is *no* difference in the average number of casualties in Muharram and non-Muharram months
Alternative Hypothesis: There is a difference in the average number of casualties in Muharram and non-Muharram months  

The average number of casualties in Muharram (from 2012 to 2015) was 5.34, whereas for other months this figure is 3.45; the difference is 1.89. Doing permutation testing on this with 50,000 repetitions, the following distribution of the differences in means is produced, with the original difference highlighted with a red line.  

<a href="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7a.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7a.png"></a> 

The p-value achieved here is 0.075, which is higher than our significance level of 0.05, hence we are unable to reject the Null Hypothesis.  


### Bootstrapped Confidence Intervals

I then thought about calculating the 95% Confidence Intervals to identify the range in which the mean number of casualties in provinces (regardless of the month) will fall. This was done with bootstrapping the data for each province. The following shows a histogram of means from each bootstrapped sample (total 10,000).  

<a href="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7b.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7b.png"></a> 

<a href="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7c.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/sk_mar17_hyptst_g7c.png"></a> 

Sindh again comes out as having a very small confidence interval, showing that on average a person or two of Shia Muslims are either killed or injured. The confidence interval is highest for Balochistan and FATA (Federally Administered Tribal Areas), for which the range of casualties on average is from 6 to 15, and 5 to 17, respectively.


The code used in this post can be found [here](https://github.com/aliarsalankazmi/skPk_analysis/blob/master/excelDB_analysis_v1.txt).

