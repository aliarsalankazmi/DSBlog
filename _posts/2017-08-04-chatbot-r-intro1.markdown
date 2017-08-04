---
layout: post
title:  "Initiating development of a chatbot with plumber and ngrok"
date:   2017-08-04 18:00:31
categories: ["Posts", "R"]
comments: true
---

Chatbots have become a rage since some time now, with [firms](https://shopbot.ebay.com/) from [various](https://www.messenger.com/t/cnn/) [sectors](https://viewfinder.expedia.com/features/introducing-expedia-bot-facebook-messenger/) investing in such bots that would reduce or remove the need of employing a call centre whilst maintaining similar levels of efficiency, if not greater. A notable example is [DoNotPay](https://donotpay-search-master.herokuapp.com/), a 'Lawyer Chatbot' which is said to give free legal aid to refugees seeking asylum in the US, Canada, and some asylum support in the UK.  


It may be that most of such companies are utilising in-house technology for chatbot development, it is also true that with the advent of developers like [API.AI](www.api.ai), [Wit.AI](www.wit.ai), and with [Facebook exposing their API](https://developers.facebook.com/docs/messenger-platform) and [Microsoft providing their SDKs](https://dev.botframework.com/) for Chatbot development, it has become much easier to craft an intelligent, engaging bot.  


Although other programming languages like Python and Javascript have a fair amount of [tutorials](https://chatbotsmagazine.com/building-a-facebook-messenger-bot-using-nodejs-heroku-api-ai-8b53c1102531) and packages that can [directly](https://github.com/conbus/fbmq) or [indirectly](http://flask.pocoo.org/) interface with Facebook's API, those for R have been rather scarce.  


Following are my limited, baby steps into the world of chatbot development using R and [ngrok](https://ngrok.com/), making use of the Facebook Chatbot Development Platform.  


Since we are using Facebook's platform, we need to first set up a Facebook page, and then set up a Facebook app - both of these are easy steps, so I am going to skip covering them in detail. We create any page on Facebook, assign it a name, and we move on to [Facebook for developers](https://developers.facebook.com), where we create a new Facebook App ID.  

<a href="{{ site.baseurl }}/assets/img/app_setup_page1.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/app_setup_page1.PNG"></a> 


Then, we add 'Messenger' as our product to the page, and generate a page access token that our chatbot will use later on.  

<a href="{{ site.baseurl }}/assets/img/app_setup_page2.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/app_setup_page2.PNG"></a> 


To be able to send real-time notifications to our chatbot (and also to verify us!), Facebook uses [Webhooks](https://developers.facebook.com/docs/graph-api/webhooks). We are required to list a URL here to which Facebook will direct any notifications we ask it to (keep your copy of [Messenger Platform Document](https://developers.facebook.com/docs/messenger-platform) at hand, folks!).


But where do we host our app such that we can communicate it via its URL?  
A possible candidate is [Heroku](https://www.heroku.com/) - we can develop a simple chatbot and deploy it to Heroku. It will be live, and thus able to receive any verification requests and notifications that Facebook might send. R does not have an official buildpack for Heroku at the time of this writing, however. There is an [unofficial buildpack](https://github.com/virtualstaticvoid/heroku-buildpack-r) on Github, but I have been unable to get it working for me *and* I am feeling lazy. So instead of heroku, I am going to rely on ngrok.  


### Set up Webhook and Complete our Verification


We need to first setup our Webhook, as Facebook will initially send a [Verification request](https://developers.facebook.com/docs/graph-api/webhooks#verification). It is only after this verification that Facebook will send notifications to our chatbot via the callback URL. In a verification request, Facebook sends 3 parameters:  


1. hub.mode (this is set to 'subscribe) 
     
2. hub.challenge (this is a random string that Facebook expects us to send back)  

3. hub.verify_token (this is the same Verification Token that we will be entering in our Webhook's setup)  
  

### Step 1: Write a script with plumber package  

I am going to quickly script a few lines that will enable us to verify our callback URL, using the [plumber](https://cran.r-project.org/web/packages/plumber/README.html) package. Plumber allows you to create APIs by merely decorating your existing R code with special annotations, and I encourage you to read their [documentation](https://www.rplumber.io/docs/index.html). I have, in my current project, found it to provide functionality similar to what flask does for Python.  

```
library(plumber)
library(httr)
library(data.table)
library(RCurl)
library(jsonlite)


#* @serializer html
#* @get /
myFunc <- function(hub.mode, hub.challenge, hub.verify_token){
if(hub.verify_token == "RRocker"){
	return(hub.challenge)
 } else{
return(paste("Verification Token mismatch", 403))
 }
}
```


### Step 2: Save the Script and Run it!  

Once you have copied the script shown above, save it as an R file, open an R session, and run the following code:  

```
r <- plumb("<path to your R file>")  

r$run(port=5000)  
```


### Step 3: Launch ngrok   

Now, you need to open your ngrok and type in this code:  

`
ngrok http 5000
`

This will show you a screen like the one below, where you can take note of the *Web Interface* and *Forwarding* URL. The web interface will allow you to inspect requests and their responses as they happen in real time, whereas the secure URL in the forwarding section will be used to register our callback URL with Facebook.  

<a href="{{ site.baseurl }}/assets/img/ngrok_r_1.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/ngrok_r_1.PNG"></a> 


### Step 4: Launch another R session and/or Ngrok Web Interface to test our script  

You may open another R session to test our script. For example, I ran the below code and inspected the response in ngrok's Web Interface as well as in R using the httr package:  

```
library(httr)  

url2 <- "https://a5da2efd.ngrok.io?hub.challenge=hubChallenger&hub.mode=subscribe&hub.verify_token=RRocker"  

res1 <- GET(url2, verbose())
```  

Keep in mind the significance of parameters shown above (after the question mark). You can see in a screenshot of ngrok's web interface an inspection of our request and the response given by our script - things are looking good!  

<a href="{{ site.baseurl }}/assets/img/ngrok_r_2.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/ngrok_r_2.PNG"></a> 


### Step 5: Copy and paste the secure URL from ngrok to Facebook's Callback URL in Webhook Setup  

Here, we simply paste in the secure URL from ngrok to Facebook. If things are alright, we will see a screen like below.  

<insert image here>
<a href="{{ site.baseurl }}/assets/img/app_success_webhook1.PNG" target="_blank"><img src="{{ site.baseurl }}/assets/img/app_success_webhook1.PNG"></a>   



### What Now?  

Admittedly, this is a very first step in our chatbot development endeavour. It is, however, a vital one. What we ought to do now is to carry out a perusal of [Facebook's documentation](https://developers.facebook.com/docs/messenger-platform) to understand how we need to handle POST requests, as it is such requests that will contain messages from other users as they try to engage with our chatbot.  

I am unsure if I will be able to follow this through any time soon, as being employed full time and still working on a startup are already overwhelming tasks! But perhaps somebody more skilled will be able to take on from here sooner than I return!  


## Reference  
  
This blog entry obtained a lot of help from [Masnun's blog post](http://masnun.com/2016/05/22/building-a-facebook-messenger-bot-with-python.html), which uses Python and ngrok for setting up an echo bot.

