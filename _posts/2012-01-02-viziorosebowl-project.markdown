---
layout: post
title: "VIZIORoseBowl Project"
date: 2012-01-02 09:04
comments: true
categories: vizio, rosebowl, ruby, rails
---

As I'm writing this post right now, my latest project is getting launched for viewing eyes.
Social Print Studio was commissioned by Vizio to setup a social media display
at their booth at the 2012 Rose Bowl that pulls in photos from twitter and instagram realtime
and visualizes the photos on displays.

Additionally, if the tag #VizioRoseBowl is used, then the photos are automatically printed as
well on fast high quality photo printers. Overall a pretty nifty, novelty project for the fans,
but an interesting project nonetheless.


I used Twitter's and Instagram's APIs, specifically their realtime hashtag related APIs. Interestingly
enough, their realtime api's work pretty differently, which was a pain to handle, but not a really big
issue in the long run. In terms of technologies, the project is in Rails, with a EventMachine process
running in a rake task to support Twitter's Streaming API which requires a open http streaming connection
at all times. To handle photos, I use paperclip to upload them to s3 as they come, and depending on the
photo source, do some grunt work to get the right photos uploaded (mainly an issue for twitter since
they have no centralized media upload).

I use resque to queue up jobs for workers. This REALLY helps with scalability, and heroku makes it easy
to turn on and off workers on the fly with **heroku ps:scale worker+1** or **worker-1**. Love resque and heroku
for how well they work together on this. 

Throwing this thing into production within a week and a half has been a stressful but fun experience
altogether, definitely better than sitting around at home doing nothing this break. It's always
interesting throwing things into production and watching the system take on a large load of incoming data,
especially since sometimes, unexpected breakage can occur, eg. race conditions between when a record gets
saved and when its amazon url is actually produced, and the necessity to fix it and redeploy as quickly
as possible.

The visualizer is very similar to the one we did for marcfam, except this time around, we threw in
realtime api's vs a pull every x minutes.

Did I mention heroku rocks? Heroku has deploying releases all figured out, server downtime between
deploys is almost unnoticeable.
