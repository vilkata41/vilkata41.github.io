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
						.setOAuthConsumerSecret(ImportantConstants.CONSUMER_SECRET)		// We'd need the consumer key and secret AND
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

## My second goal

The easiest (personal opinion, of course) part was over, things were about to get hot in there...<br><br>

What I had to do now is just build on top of the barebones. Easy. Right? **Wrong!**<br>
I had to use the functionality of my twitter information gathering application and build my own API with the help of [Java Spring Boot](https://spring.io/projects/spring-boot). After this was done, I just had to deploy it to a cloud service - more specifically, Google Cloud Platform and run the API in order for my third, and final part of the project to be doable.<br><br>

I'll be completely honest... It didn't sound difficult at all. I just had to learn what Spring Boot was and upload it to that GCP. However, I faced a whole lot of unexpected problems when trying to figure it out on my own.<br>
Learning Spring Boot was fine. Coding it was also fine. I read a couple of articles, watched some tutorials and it cleared up for me.<br><br>

Thus, I coded it up...<br>
I had a TweetFilteredAPIApplication that ran the whole API with the help of Spring Boot. My API was layered (sort of unnecessary for smaller projects like mine but really useful for larger projects) - I made a Base (functionality) layer, a Service layer, and a Controller layer. <br>
The base layer consists of the Post class and the TwitterInfo class. The Post class makes it easy to access different posts from across the entire project and the TwitterInfo consists of the already covered barebones project *(with some modifications on it)*.<br><br>

The Service layer feels unnecessary for my smaller project but I included it anyway. It drains the information from the base layer and later hands it to the Controller layer. That Controller layer maps the information to a specific page (with the `@RequestMapping` and `@GetMapping` annotations) and returns the results on that page.<br>
To find more information regarding the coding side of the second part, feel free to check out [THIS](https://github.com/vilkata41/tweetfilteredAPI) repo.<br><br>

This seemed fine so far... but when deploying time came, so did all my major problems with this project.<br>
It was a little difficult for me to find relevant information for my case so I decided I'd learn on the fly. I consider this both a bad decision and a good decision. A bad decision for my nerves, and a good decision for my experience.<br><br>

What made it difficult for me is having to learn most of the xml, maven, gcp file organisation, and some console programming.<br>
I didn't understand much of any of these when I started but problems started coming one after the other. Right as I thought it was finally ready, another problem would appear and I'd have to deal with it.<br><br>

Misunderstanding the difference between JAR files and WAR files was also a part of my problems.<br><br>

However, a couple of days later I fixed my final problem with this and the API was fully functional and deployed to Google Cloud Platform's AppEngine.<br>
You can check the `getPosts()` mapping [HERE](https://tweetfilteredrestapi.ey.r.appspot.com/api/tweetfilteredV1/all) and the `getRecent()` [HERE](https://tweetfilteredrestapi.ey.r.appspot.com/api/tweetfilteredV1/recent).<br><br>

### My third goal

