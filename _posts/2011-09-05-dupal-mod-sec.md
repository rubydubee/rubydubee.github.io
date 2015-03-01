---
layout:     post
title:      Drupal 7 + mod_sec
date:       2011-09-05 05:46:00
summary:    Drupal 7 and mod_sec doesn't go well together, if your IP is getting banned by your site, here is a solution.
categories: web
---

While developing one of my friend’s website in Drupal 7(this was the first time I have installed Drupal 7 on shared host), My IP Address was being continuously blocked by Mod\_Security. So I asked for the logs and found that some rule in Mod\_Security (id=950004) is blocking jquery.cookie.js. I googled it and found that many people are having the same issue.
So here is the solution that worked for me(Use this if you don’t want to waste your time fighting with your shared webhost.) :

1. Rename your jquery.cookie.js to something like jquery-c.js.
2. You will also need to change the mapping in system.module - find “jquery.cookie” and replace with “jquery-c”.

Hope that will help.