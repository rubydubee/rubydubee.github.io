---
layout:     post
title:      Heroku + Godaddy combination is crazy
date:       2012-10-23 03:04:00
summary:    Some nitty-gritties of using wildcard subdomain on Heroku and Godaddy, must read if you are using it.
categories: web
---

Well, the time for this post cannot be more dramatic; One of my heroku app is down like many others but that is not why i am writing this post and that is not the topic for the post either. You might not have got much from the post title yet, but let me tell you when you read this post you will surely agree with it!

First of all, Heroku is great! Easy deployment, easily scalable; surely one of the best cloud hosting platform for rails application. We have been using Heroku since last one year and it works! Our app was on Heroku’s Bamboo stack for quite some time. A month ago, we had a lot of traffic and decided to serve static homepage, which went well and it served its purpose. Then after couple of weeks we decided to migrate our application to Cedar stack. Again all went well! Now last week, I removed the static index page and got back to rails for serving our homepage. Didn’t work! Heroku was still serving the static page from cache, I tried redeploying the app as heroku says, “Cache will be cleared on every push”. Didn’t work! Somehow i found that we had not updated our CNAME record for “www”, so changing the record from “proxy.heroku.com” to “proxy.herokuapp.com” solved the issue for www subdomain. Awesome!

Now, we use wildcard subdomains(*.example.com) as referral links for our users and we point it to our homepage. Heroku recommends adding CNAME record for wildcard subdomains and Godaddy does not allow it, but it does allow adding A record for wildcards which works fine. Now I have “A” record for wildcard subdomain pointing to one of the IP Addresses from heroku. The problem is, it shows me the old static page! WTF? HOW? I searched and searched! Everything failed! Then i found that the homepage was being served through Varnish which is not available for Cedar stack, that means somehow the page was being served through the old bamboo app’s cache, there you go! I pushed(deployed) the bamboo app and it cleared its cache! Ta-Da everything worked! But How on earth am I supposed to get this? Something is seriously wrong!

Here are some of the takeaways : 

1. If you are using wildcard subdomain, you should always use CNAME records (link)
2. If you have to use A record, you must redeploy your bamboo app to clear Varnish cache for static assets after migrating to Cedar stack.

Hope it will help someone! Cya!