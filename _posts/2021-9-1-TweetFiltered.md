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

### My second goal

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

The final part of the TweetFiltered project consists of a python script, and a couple of folders (to store the original images and the filtered images). You can find the full repo [HERE](https://github.com/vilkata41/filterApplication). Now, I'll just cover the functionality of the script.<br><br>

I start off by instantiating the API deployed to GCP and mediapipe (the face mesh library I used) drawing utils, and face mesh:<br>
```
    mp_drawing = mp.solutions.drawing_utils
    mp_face_mesh = mp.solutions.face_mesh
    api_all_url = "https://tweetfilteredrestapi.ey.r.appspot.com/api/tweetfilteredV1/all"
    response = requests.get(api_all_url)
```
I, then extract all files (if there are any media files uploaded with the tweet, of course) and store them to `/imgs/` or `/vids/` folders, depending on the content.<br>

```
    for post in response.json():
        if post['media'] is not None:
            for m in post['media']:
                IMAGE_URLS.append(m)

    for img in IMAGE_URLS:
        filename = img.split("/")[-1] # we get the name

        r = requests.get(img, stream = True)

        if r.status_code == 200 and not "?tag=12" in filename: # if it exists and is not a video (videos have ?tag=12 in the end)
            filename = "imgs/" + filename
            r.raw.decode_content = True
	    
# the file is written

            with open(filename, 'wb') as f:
                shutil.copyfileobj(r.raw, f)

        elif r.status_code == 200 and "?tag=12" in filename: # else if it is a video
            filename = "vids/" + filename.strip("?tag=12")
            r.raw.decode_content = True
	    
# write the file in another folder

            with open(filename, 'wb') as f:
                shutil.copyfileobj(r.raw, f)

        else:
            print("problem")
```

I store the image paths into a list and then process that list by editing each picture that is on it. Firstly, I have a picture with all face landmarks. Secondly, I draw a rectangle around the face with the help of key facial landmarks on another picture. Lastly, I resize the filter and add it to the picture. All three product pictures are saved in the `/tmp/` folder of the project.<br>

```
# we draw dots on all face landmarks and save the final product as an annotated_image.
        for face_landmarks in results.multi_face_landmarks:
            print('face_landmarks:', face_landmarks)
            mp_drawing.draw_landmarks(
                image=annotated_image,
                landmark_list=face_landmarks,
                landmark_drawing_spec=DrawingSpec(color=(100, 100, 0), thickness=2, circle_radius=1)
            )
	    
# more calculation code - you can check it in the full repo

cv2.rectangle(square_image,
                          pt1 = (left[0], int(top[1] * 0.9)), pt2 = (right[0], bot[1]),
                          color = (255,0,255),
                          thickness = 10
                          )

            ironman_filter = cv2.resize(ironman_filter, (filter_width, filter_height)) # the filter is resized according to the face details

            x_offset = int(top_left[0])
            y_offset = int(top_left[1])

            y1, y2 = y_offset, y_offset + ironman_filter.shape[0]
            x1, x2 = x_offset, x_offset + ironman_filter.shape[1]

            alpha_s = ironman_filter[:, :, 3] / 255.0
            alpha_l = 1.0 - alpha_s

            for c in range(0, 3):
                filtered_image[y1:y2, x1:x2, c] = (alpha_s * ironman_filter[:, :, c] + alpha_l * filtered_image[y1:y2, x1:x2, c]) # we overlay the filter
```

Finally, we use the opencv (cv2) to write the products in the tmp folder:
```
cv2.imwrite('tmp/annotated_image' + str(idx) + '.png', annotated_image)
            cv2.imwrite('tmp/filtered_image' + str(idx) + '.png', filtered_image)
            cv2.imwrite('tmp/square_image' + str(idx) + '.png', square_image)
```

This concludes the third, and last part of the TweetFiltered project i worked on for 2 months.<br><br>

## Conclusions

This summer project was a challenge for me, a yet-to-be second year university student.<br>
There might have been a couple of difficulties and parts that may have frustrated me but my overall experience was great. I am extremely grateful that I had the opportunity to learn crucial sides of programming this early in my journey. Things that I'll likely learn in year 3 or 4 in university. I suppose I was lucky this summer, being given this project, and dealing with it on my own.<br><br>

Of course, it wasn't completely on my own - we all shared our experiences, we got some help from our tutor. And honestly, I was on the brink of desperately asking someone for help but figured things out in the end.<br>
While asking would've saved me a lot of time, I'm certainly glad that I figured the problems out on my own because that is one of the best ways to actually memorize the matter covered.<br><br>

I am most definitely willing to work on another project like this. I believe I am ready for yet another challenge. Something to take me out of my comfort zone. Something to push me towards the improvement of my skills.<br><br>

### Vilian P.<br><br>

P.S. Thank you, Amine and the boys. You are some real ones!
