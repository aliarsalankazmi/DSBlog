---
layout: post
title:  "Emotion Detection for Journalism with Microsoft Emotion API"
date:   2016-11-12 10:45:31 +0530
categories: ["Posts", "R"]
comments: true
---


Microsoft provides some sophisticated Machine Learning tools via its <a href="https://www.microsoft.com/cognitive-services" target="_blank">Cognitive Services</a>: Microsoft enables us to access several of its APIs in order to leverage Advanced Analyses. One of these is the <a href="https://www.microsoft.com/cognitive-services/en-us/emotion-api" target="_blank">Emotion API</a> which can be fed pictures or videos that will be analysed and emotions detected therein will be returned to the user.   

Emotion Detection can have a variety of applications, one of them being to Data Journalism. Ben Heubl, for example, <a href="https://benheubl.github.io/data%20analysis/fr/" target="_blank">applied</a> this technique to the Presidential Election Debates between Hillary Clinton and Donald Trump. And whilst Emotion Detection is not a 100% accurate, it does enable Data Journalists to supplement their repertoire with an interesting capability.  

In this (short) post, I shall be attempting to use MS Emotion API to detect emotions in instances where the conventional Journalistic reporting is limited: Meetings where microphones are supposed to be switched off, and only cameras are on. Examples are occasionally the meetings between the Pakistani Prime Minister Nawaz Sharif and Raheel Sharif, the Chief of Army Staff. (I have been told that) In such gatherings, journalists are sometimes requested to use only a Video camera and not a microphone to record discussions. Press Releases are subsequently made, outlining the topics of discussion.  

For our first run, I have passed the following clip to the MS Emotion API (sourced from the PM Office's Facebook Page).  

<iframe width="560" height="315" src="https://www.youtube.com/embed/vwqMIMInmgw" frameborder="0" allowfullscreen></iframe>

After MS Cognitive Services process this video, a result is returned containing emotions ascribed to each face observed in the video. Emotions identified include Neutral, Anger, and Contempt. Scores for each emotion are normalised to lie in the range of 0 - 1.  

<a href="{{ site.baseurl }}/assets/img/nawaz_raheel_meeting_g1.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/nawaz_raheel_meeting_g1.png"></a>    
The chart above is a Stacked Area chart, faceted for each attendee (Nawaz and Raheel); the legend shows colour coding for each emotion. You will notice a break in the chart for Nawaz Sharif, which denotes the shift of camera to Raheel in the actual video.  
We see that for Nawaz Sharif, most prevalent emotions are 'Neutral' and 'Sadness'. For Raheel Sharif, 'Neutral' emotion is the most prevailing, but we see bouts of 'Anger' too. Since the technology is not perfect (nor is the COAS very expressive in current example), we also notice some amount of 'Happiness' for both Nawaz and Raheel.   

I thought we could also analyse emotions of the PM from one of his addresses to the (Pakistan) nation. Availability of HQ videos was limited, in my finding, and most videos were clips from News Channels that often 'polluted' the scene with advertisements. Nevertheless, following is a relatively clean video.  

<iframe width="560" height="315" src="https://www.youtube.com/embed/zOY1y-KKlb0" frameborder="0" allowfullscreen></iframe>  

The following chart shows an un-smoothed version of a line plot, showing emotions as lines coloured distinctly for each emotion.  

<a href="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g1.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g1.png"></a>   

We see that, again, most dominant emotion is 'Neutral', but there are portions of 'Anger', 'Sadness', and even 'Surprise'. This can be modified to produce smoothed lines that more clearly show changes or rise and fall of emotions.  

<a href="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g2.png"></a>   

Now, we can see that the most notable emotions are of neutrality and anger. These scores can be averaged too:  

<a href="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g3.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/nawaz_19Aug2013_g3.png"></a>   


So, a very quick tutorial of a technology that can be very relevant to Data Journalists in Pakistan, particularly to quantify emotive responses in situations where journalists are not allowed to record conversations. It can also be used to study emotions of various political leaders in interviews, to then report how emotions of politicians might be aroused as they are asked difficult questions.  


The `R` code used to perform this analysis and visualisation can be found <a href="https://raw.githubusercontent.com/aliarsalankazmi/Facereading-with-MS-Emotion-API/master/Scripts/Nawaz%20Raheel%20Analyses_Final.txt" target="_blank">here</a>.  
I would like to acknowledge the kind help offered by my brother in collecting videos that were analysed (which can be somewhat difficult as the Emotion API has a limit of 100Mb on the size of an uploaded video).
