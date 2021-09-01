---
layout: post
title: TweetFiltered Project
---

## This summer...

*I wanted to do something different.<br>
I wanted to challenge myself. <br><br>*

To see if I can improve with drastic changes in the difficulty of my projects.<br>
To see how far I can go with as little help as possible.<br>
**SPOILER!** It wasn't easy but I made it in the end.<br><br>

This story begins in the end of Year 1 in university... One of our tutors saw potential in me and four other boys and offered to give us each a project **(way more difficult than those given to us in university)**, to monitor us, and to give us help if we truly needed it. All of this for free!<br><br>

So in the beginning of the summer all of us got our projects in detail. That's how the notion about **TweetFiltered** was born.<br>
Storytime's over, let's get factual...<br><br>

### My first goal

Firstly, I had to apply for the Twitter API with a twitter account specifically made for this project. After *(and of course, IF)* I was accepted, I had to create the barebones of my application's data withdrawal side in Java.<br>
This means that I coded up the functionality consisting of downloading specific posts - ones the tweetfiltered account has been **mentioned in**. The app accessed information as follows:<br>
* tweet creator (username)
* text of the tweet
* media of the tweet

Here, I'll tell you only about the basics (more detail about the barebones you can find in [THIS](https://github.com/vilkata41/tweetfiltered) repo).<br><br>

At first, we load our twitter account in the project, so we can access the data:<br>
```

ConfigurationBuilder configurationBuilder = new ConfigurationBuilder(); 		 // Set up the configuration builder
		configurationBuilder.setDebugEnabled(true).setOAuthConsumerKey(ImportantConstants.CONSUMER_KEY) 	// For everything to work properly with Twitter's API
						.setOAuthConsumerSecret(ImportantConstants.CONSUMER_SECRET)			 // We'd need the consumer key and secret AND
						.setOAuthAccessToken(ImportantConstants.ACCESS_TOKEN)			// The access token and secret.
						.setOAuthAccessTokenSecret(ImportantConstants.TOKEN_SECRET);
		
		Twitter twitter = new TwitterFactory(configurationBuilder.build()).getInstance();   // Getting the instance for the twitter account.
		
		try {
			User user = twitter.verifyCredentials(); // Getting the actual tweetfiltered account on twitter.

      // More code here...
```
As one can notice, I've kept the twitter tokens into a separate class (there is a sample version for it - everyone can fill it with their own credentials).<br>
The second part of the tweetfiltered barebones Java project makes use of simple functions such as `getUser()`, `getName()`, `getText()`, `getMediaEntities()`, etc.<br><br>

Being finished with this functionality meant that I was also done with part 1 of my entire project.<br><br>

