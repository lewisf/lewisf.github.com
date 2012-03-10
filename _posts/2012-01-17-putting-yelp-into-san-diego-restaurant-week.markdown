---
layout: post
title: "Putting Yelp into San Diego Restaurant Week"
date: 2012-01-17 11:22
comments: true
categories: [Programming, Code, San Diego, Restaurant Week, Yelp]
---

This past weekend, a friend asked me where I wanted to go for San Diego Restaurant Week,
so I opened up the restaurant week listing page, took a quick glance, and said to myself,
"what the hell, how do I know which restaurant to look at if there are no reviews or
ratings." I guess we've all been spoiled by yelp, we only put money where the reviews are
at nowadays.

{% img https://s3.amazonaws.com/lewisissocool/before.png Before %}

So I set out to fix it, by creating a javascript bookmarklet that automatically uses 
Yelp's API to search each listing based on the name and neighborhood/location. 
This was the end result:

{% img https://s3.amazonaws.com/lewisissocool/after.png After %}

Pretty fun side project, and over 100 people have already used it within a day of me launching
it to the public! Anyway, this is why I love these little half-day programming projects -- it
makes me smile that with some thinking and thousands of keypresses, I can help many strangers
out all while satisfying my thirst for knowledge. 

**Special thanks to Andrew S. over at Yelp for raising my Yelp API request limit :)**
