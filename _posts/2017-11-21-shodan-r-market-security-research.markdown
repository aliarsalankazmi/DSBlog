---
layout: post
title: "Shodan - A tool for Security and Market Research"
date: 2017-11-21
categories: ["Posts", "R"]
comments: true
---

So you have a knack for all things data, know a dozen different Machine Learning algorithms, are a skillful user of Big Data tools, and have started learning Deep Learning &mdash; not much else there is to Data Science, right?  
Well, I would submit that knowing ML algorithms and tools does not bring about the same Eureka Effect that, perhaps, a noteworthy change of data brings. Yes, we have Sound and Text data, but they are quite old too.  
How about getting data of publicly accessible Internet connected devices, their ports usage, certificates issued, etc. using a **search engine**: [Shodan](www.shodan.io).

Shodan is no stranger to Security Analysts in the West, and perhaps, to an extent, to Data Scientists as well. It is less known amongst Security and Data Science professionals in the Middle East, however. Also, perhaps what is less known about Shodan is that it was originally developed as a Market Research tool. Shodan was developed by [John Matherly](https://twitter.com/achillean?lang=en) as a search engine for Internet accessible devices. It has crawlers placed at various geological locations that crawl the internet, obtaining meta data of any web-enabled device. A quote from John's book, [Complete Guide to Shodan](https://leanpub.com/shodan): "Web search engines, such as Google and Bing, are great for finding websites. But what if you're interested in finding computers running a certain piece of software (such as Apache)? Or if you want to know which version of Microsoft IIS is the most popular? Or you want to see how many anonymous FTP servers there are? Maybe a new vulnerability came out and you want to see how many hosts it could infect? Traditional web search engines don't let you answer those questions".  
**Shodan does!**   


### Introductory Usage


For a start: Let's head to [shodan](www.shodan.io) and see if we can find some publicly exposed cameras: often, we have non-technical people install camera devices that are also connected to the Internet. Although they do install them, they are unaware of how to safeguard their devices from unauthorised access.  
A simple query of `webcamxp country:"GB"` returns 36 results of webcams, some of which are publicly accessible. The following shows a webcam placed at some roundabout in the UK.  

<a href="{{ site.baseurl }}/assets/img/shodan_post1_publicCam1.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_post1_publicCam1.PNG"></a> 


But this was from the UK. Let us head over to the Middle East and try some other searches!   


I know that Bitcoin is getting famous amongst residents of UAE; let's try searching for Bitcoin on port 8333, which is [designated](https://bitcoin.org/en/full-node#network-configuration) for running a Bitcoin client. We have two results, one in Dubai and one in Abu Dhabi.

<a href="{{ site.baseurl }}/assets/img/shodan_post1_bitcoinUAE1.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_post1_bitcoinUAE1.PNG"></a>


Interesting, isn't it?  
But it is cumbersome to repeatedly go to the Web Interface, enter in our query, and then export the results (for the latter, you will have to become a member, which involves a minimal fee). Shodan does include a Command Line Interface, as well as an API. But since this blog is about all things `R`, I shall be utilising the [`shodan`](https://github.com/hrbrmstr/shodan) package developed by [Bob Rudis](https://twitter.com/hrbrmstr). I feel if `R` had a hall of fame, Bob deserves to be inlcuded in it for his valuable contributions.  


### Using the R package for Shodan


#### MongoDBs


Let's do a quick search for Mongo DB in the UAE.

```
# Loading required packages

library(shodan)
library(data.table)
library(hrbrthemes)
library(tidyverse)
library(jsonlite)
library(extrafont)
library(viridis)

# Our search query:
uae_mongodb <- shodan_search("product:mongodb country:AE")

uae_mongodb$matches %>% 
	select(as.numeric(version), org) %>% 
	add_column(city = uae_mongodb$matches$location$city) %>%
	mutate(vers = gsub("(^[[:digit:]]+\\.)[[:digit:]]+\\..+$", "\\1", version)) %>%
	group_by(city, org, vers) %>%
	count() %>%
	ggplot(data = ., aes(x = vers, y = n)) +
	geom_col() +
	facet_wrap(~ city) +
	ggtitle("Counts of MongoDB instances per Version and City") +
	labs(y = "Counts", x = "MongoDB Version") +
	theme_ipsum()

```

<a href="{{ site.baseurl }}/assets/img/shodan_g1.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g1.png"></a>


This was not so useful, as we only had 6 results from our search, with only 1 city identified (Dubai). One search result did not have any city information, but the IP address was available. Using Censys.io, it takes us to [Emircom's IoT Portal](http://iot.emircom.com/#/intro). They have an 80MB database, and as per this [view](https://www.shodan.io/host/213.42.180.195), some additional parameters like 'listDatabases' also have a value to them (201 in this case). I don't think they would like to expose such information to the general public.   

Another one [here](https://www.shodan.io/host/94.200.190.157) seems to be exposing some info too.  Using Censys.io again, it seems this IP address is related to some Medical Imaging Information Management System.  

A couple of quick notes here:   

1. I could have parsed this additional info using what is returned by `shodan` to `R`, but it seems the returned JSON does not follow conventions for a JSON format, so a conversion fails.  

2. I think one can try to connect to the aforesaid MongoDB instances using a client. But unless one has an account with privileges, (s)he cannot perform actions on the database.  


This was more of a security perspective. How about some market research?


#### Apache


We can search for Apache without using any product filter. This will return any banners that contain the word Apache. I am going to search the first 100 pages, since I am just giving an overview. 

```
uae_apache <- shodan_search("apache country:AE", page = 100)
rDf <- uae_apache$matches %>% 
	select(version, org, port, product, domains) %>% 
	add_column(city = uae_apache$matches$location$city)

rDf %>% mutate(vers = gsub("(^[[:digit:]]+\\.[[:digit:]]+)\\..+$", "\\1", version)) %>%
	group_by(vers, city) %>%
	count() %>%
	ggplot(data = ., aes(x = reorder(vers, n), y = n)) +
	geom_col() +
	facet_wrap(~ city) +
	ggtitle("Counts of Apache products per (trimmed) Version and City", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "Version Number") +
	theme_ipsum()

```

<a href="{{ site.baseurl }}/assets/img/shodan_g2.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g2.png"></a>


Such info, albeit obtained from a sample of the results, could be useful for Market Research. We could also count instances of Apache against any organisations identified:  

```
rDf %>% group_by(org) %>%
	count() %>%
	ggplot(data = ., aes(x = reorder(org, n), y = n)) +
	geom_col() +
	coord_flip() +
	ggtitle("Counts of Apache products per Organisation", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "") +
	theme_ipsum()

```

<a href="{{ site.baseurl }}/assets/img/shodan_g3.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g3.png"></a>



#### IBM Server

As another example, let's search for IBM servers accessible to Shodan. I am just going to sample the results, rather than obtain all of them:  


```
uae_ibm <- shodan_search("product:IBM country:AE")
rDf <- uae_ibm$matches %>% 
	select(version, org, port, product, info, ip_str) %>% 
	add_column(city = uae_ibm$matches$location$city) %>%
	add_column(certif_issuerC = uae_ibm$matches$ssl$cert$issuer$C, 
		   certif_issuerO = uae_ibm$matches$ssl$cert$issuer$O, 
		   certif_obtainer = uae_ibm$matches$ssl$cert$subject$O)

rDf %>% mutate(vers = gsub("(^[[:digit:]]+\\.[[:digit:]]+)\\..+$", "\\1", version)) %>%
	group_by(vers, product) %>%
	count() %>%
	ggplot(data = ., aes(x = reorder(vers, n), y = n)) +
	geom_col() +
	facet_wrap(~ product) +
	ggtitle("Counts of IBM products per (trimmed) Version", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "Version Number") +
	theme_ipsum()

```

<a href="{{ site.baseurl }}/assets/img/shodan_g4.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g4.png"></a>



```
rDf %>%
	group_by(org) %>%
	count() %>%
	ggplot(data = ., aes(x = reorder(org,n), y = n)) +
	geom_col() +
	coord_flip() +
	ggtitle("Counts of IBM products per Organisation", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "Organisation") +
	theme_ipsum()

```


<a href="{{ site.baseurl }}/assets/img/shodan_g5.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g5.png"></a>



```
rDf %>%
	select(certif_issuerO, certif_obtainer) %>%
	gather() %>%
	mutate(certif = ifelse(key == "certif_issuerO", "Certificate Issuer", "Certificate Obtainer")) %>%
	group_by(certif, value) %>%
	summarise(counts = n()) %>%
	ggplot(data = ., aes(x = reorder(value, counts), y = counts)) + 
	geom_col() +
	coord_flip() +
	ggtitle("Counts of Certificate Owners and Issuers for IBM products in the UAE", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "") +
	facet_wrap(~ certif, ncol = 1, scales = "free") +
	theme_ipsum()


```

<a href="{{ site.baseurl }}/assets/img/shodan_g6.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g6.png"></a>



#### Industrial Control Systems (ICS)  


Let's try searching for IoT devices in the UAE.


```
uae_ics <- shodan_search("category:ics country:AE")
rDf <- uae_ics$matches %>% 
	select(version, org, port, product, devicetype, os, isp, ip_str) %>% 
	add_column(city = uae_ics$matches$location$city)

rDf %>% group_by(product) %>%
	count() %>%
	ggplot(data = ., aes(x = reorder(product, n), y = n)) +
	geom_col() +
	coord_flip() +
	ggtitle("Counts of ICS products in the UAE", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "Products") +
	theme_ipsum()
```

<a href="{{ site.baseurl }}/assets/img/shodan_g7.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g7.png"></a>


```
rDf %>% group_by(org) %>%
	count() %>%
	filter(!is.na(org)) %>%
	ggplot(data = ., aes(x = reorder(org, n), y = n)) +
	geom_col() +
	coord_flip() +
	ggtitle("Counts of ICS Products used by various Orgs in the UAE", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Count of ICS Products", x = "Organisation") +
	theme_ipsum()

```

<a href="{{ site.baseurl }}/assets/img/shodan_g8.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g8.png"></a>


In case you are wondering, Niagara Fox and Lantronix are IoT platforms that enable machines/devices to speak to each other seamlessly for purposes such as building automation, smart cities, etc.   
We also see some well-known organisations; let's try checking the IP address of AVAST software. Plugging the IP address into [RIPE](https://www.ripe.net/), leads us to Holborn, London. But the port that was accessed by Shodan for AVAST was 44818, which is used for EtherNet, and I am unsure if this should be of concern.  

 

#### Checking Hotels

```
uae_hotels <- shodan_search("org:Hotel country:AE")
rDf <- uae_hotels$matches %>% 
	select(version, org, port, product, devicetype, os, isp, ip_str) %>% 
	add_column(city = uae_hotels$matches$location$city)

rDf %>% group_by(org) %>%
	count() %>%
	filter(!is.na(org)) %>%
	ggplot(data = ., aes(x = reorder(org, n), y = n)) +
	geom_col() +
	coord_flip() +
	ggtitle("Counts of Various Products used by various Hotels in the UAE", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Count of Various Products", x = "Hotels") +
	theme_ipsum()


```

<a href="{{ site.baseurl }}/assets/img/shodan_g9.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g9.png"></a>



So, of these, let's try to see what ports are being run/used at the Conference Palace and the Shangri-La Hotels.  


```
rDf %>% filter(org %in% c("Conference Palace Hotel", "Shangri-La Hotel")) %>%
	group_by(org, port) %>%
	count() %>%
	filter(!is.na(org)) %>%
	ggplot(data = ., aes(x = reorder(port, n), y = n)) +
	geom_col() +
	coord_flip() +
	facet_wrap(~ org) +
	ggtitle("Counts of Various Ports used by Conference and Shangri-La Hotels in the UAE", 
		subtitle = "Graph is based on a sample of data") +
	labs(y = "Counts", x = "Ports") +
	theme_ipsum()
```

<a href="{{ site.baseurl }}/assets/img/shodan_g10.png" target="_blank"><img src="{{ site.baseurl }}/assets/img/shodan_g10.png"></a>


We see, interestingly, there is not an overlap in the ports being used at the two hotels. We also see a good amount of Port 179 usage at the Conference Palace Hotel. According to [this link](https://isc.sans.edu/forums/diary/Cyber+Security+Awareness+Month+Day+23+port+179+TCP+Border+Gateway+Protocol/7435/), it may be advisable not to rely on the said port. We also see Port 8008 being used: this is for Chromecast purposes.  

For the Shangri-La, we see little data, but one of the ports which caught my attention is 1440, for Sonos. Apparently, this is a [Home Wireless Sound system](https://www.sonos.com/en-gb/system).  



#### Conclusion

We have literally had a glimpse of the capabilities and usage of Shodan for Security and Market Research, and as it is obvious, Shodan's use as a Market Research tool is very promising. That Shodan is useful for Security Research is, I think, a side-effect. Nonetheless, it is not so simple to use Shodan for Market Research, and this is because unlike in other fields where application of Data Science/Mining requires primarily a decent knowledge of Statistics, I think with Shodan the Data Scientist would require a deep familiarity with Computer Science and Networking. This is not too hard to come by, but it may not be the repertoire of Data Scientists in Marketing Industry!  

