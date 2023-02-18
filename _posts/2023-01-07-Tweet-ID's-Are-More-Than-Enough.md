---
layout: post
title: Tweet ID's are More than Enough
description: 
tags: twitter scraping
---

TLDR; You can access a tweet with only the given tweet id <br><br>

While scouring through the internet for suitable NLP datasets (tweets to be exact), one thing that crossed my eye was the fact the most datasets did not have the actual text content in them. Instead it was just a random number called as the <b>tweet_id</b>. I suppose this was most likely due to some policy restriction by twitter and as a way to reduce the dataset file size (Leaving the hard job of scraping the entire stuff to us poor souls). 


One thing one wastes a lot of time initially was on the fact that twitter posts are of the form <i><b> https://twitter.com/&lt;username&gt;/status/&lt;tweetid&gt;</b></i> which leaves us with an unknown variable i.e. the username.


However, the fun fact is that twitter doesn't even care about the username. You can put any existing username in that URL template, and twitter will automatically redirect to the actual username if that tweet id did not belong to the specified user.
<br><br>
For example:

<div class="img_parent">
<a href="https://twitter.com/tunguz/status/1611051712479121408">
<img src="{{ "/assets/images/2023-01-07-Tweet-ID's-Are-More-Than-Enough/tweet.png" | relative_url }}" >
</a>
</div>


The above post is from the tweet <a href="https://twitter.com/tunguz/status/1611051712479121408">/tunguz/status/1611051712479121408</a>, but as we can see this does not have any affect because <a href="https://twitter.com/acedev003/status/1611051712479121408">acedev003/status/1611051712479121408</a> also renders the same result ¯\\_(ツ)_/¯.