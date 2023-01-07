---
layout: post
title: Tweet ID's are More than Enough
description: 
tags: twitter scraping
---

TLDR; You can access a tweet with only the given tweet id <br><br>

While scouring through the internet for a good enough nlp dataset (tweets to be exact) for a random project, one thing that crossed my eye was the fact the most datasets that had thousands of rows of data that did not have the actual text content in them. Instead it was just a random number called as the <b>tweet_id</b>. I suppose this was most likely due to some policy restriction by twitter and as a way to reduce the dataset file size (Leaving the hard job of scraping the entire stuff to us poor souls). <br><br>
One thing one wastes a lot of time initially was on the fact that twitter posts are of the form <i><b> https://twitter.com/&lt;username&gt;/status/&lt;tweetid&gt;</b></i> which leaves us with an unknown variable i.e. the username.<br> However, the fun fact is that twitter doesn't even care about the username. You can put any existing username in that URL template, then twitter will automatically redirect to the actual username if that tweet id did not belong to the specified user.
<br><br>
For example:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Real neural networks are all in your head.</p>&mdash; Bojan Tunguz (@tunguz) <a href="https://twitter.com/tunguz/status/1611051712479121408?ref_src=twsrc%5Etfw">January 5, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

The above post is from the url <a href="https://twitter.com/tunguz/status/1611051712479121408">https://twitter.com/tunguz/status/1611051712479121408</a> but visiting <a href="https://twitter.com/acedev003/status/1611051712479121408">https://twitter.com/acedev003/status/1611051712479121408</a> also renders the same result ¯\\_(ツ)_/¯.