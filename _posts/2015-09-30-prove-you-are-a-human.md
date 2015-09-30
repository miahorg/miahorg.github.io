---
layout: post
title:  "Prove You Are a Human"
date:   2015-09-30 07:27:14
categories: datascience
---
Many readings I've done have all directed me to a particular site. At [Kaggle][kaggle] they host host challenges where you are to analyze and optimize a dataset for a particular answer. Most are sponsored for cash prizes. One in particular is used as an example so often that it's nearly become a trope of data science texts to use the analysis of survivors of the Titanic.

Wanting to read on this challenge a bit more I found myself needing to register an account on the site. This is normal internet behavior and they want to be able to see what content I'm interested in (they are data nerds after all). I love that setting up an account is about as hard as clicking whatever social media icon you feel like at the time so click away I did.

The account is set up and magically my "gravatar" appears as my profile picture. The picture I was using was, well, ghastly so I wanted to update this. This took some doing as apparently Gravatar is a part of Wordpress.com is a part of something else, guarded by a tiger, in the basement. Short story: I figured it out. Then I edited some of the other profile details on the page. Click a link and I'm presented with "Sorry this account has been deleted."

On this notice page is the text: "Prove that you are a data scientist: What is the sum of the multiples of 3 and 5 in 3209?"

Alright, I guess this is a dick move on Kaggle's part. I just wanted to read some more about the Titanic dataset. But alright, I'll find a few minutes to do math for fun, I guess... Honestly this was more interesting than it should have been for a trite hassle. I reached for Rstudio and dabbled with a for loop for a bit.

{% highlight r %}
vec1 <- c() # empty vector
for (i in 1:3029) { # create range
  if (i %% 3 == 0 | i %% 5 == 0) { # test to see if multiples of 3 and 5
    vec1 <- c(vec1, i) # append values to vector
  }
}
sum(vec1) # find sum of values
{% endhighlight %}

That got me the correct answer for the site and I continued browsing away. Then a doubt creeped into my mind. I had just a few nights ago polished off Garrett Grolemund's [Hands-On Programming with R][hap] and I remembered the chapter on vectorizing your R code for speed. Oh no, I accidentally learned something! Damnit! So I vectorized my solution.

{% highlight r %}
count <- c(1:3029) # create vector with all potential values
sum(count[count %% 3 == 0 | count %% 5 == 0]) # sum only selection
{% endhighlight %}

Nerding out further I ran system.time() as a profiler on both methods. Not that it took a long time to complete, we are doing science here. The for loop method to .07 seconds which isn't too shabby. The vectorized version took .01 seconds. That is one hell of a speed up! Finding myself to be unsatiated in my nerdity I plowed on to see how python would fare in the same challenge.

{% highlight python %}
vec1 = []
for x in range(1, 3210):
	if (x % 3 == 0 or x % 5 == 0):
		vec1.append(x)
sum(vec1)
{% endhighlight %}

If this looks familiar it's because it is. The profiler says that this found the correct solution in .018 seconds. That's nearly as fast as the vectorized R. Can we vectorize the python? [Will it blend?][wib]

{% highlight python %}
sum(np.array(filter(lambda x: x % 3 == 0 or x % 5 == 0, np.array(range(1,3210)))))
{% endhighlight %}

In fact, it does blend. But there was not a crazy performance gain to be had with this size of a dataset. The result was .018 seconds just like before. I'm fairly certain that a larger set returned would see those numbers diverge over time. Moral of the story? Loops in R are demonstrably slower than using vector operations. That's just Rs jam though and the need for speed isn't all consuming.

Humans are nerds - I think this proves that I'm fairly human.

[kaggle]: https://www.kaggle.com
[hap]: http://shop.oreilly.com/product/0636920028574.do
[wib]: http://www.willitblend.com/