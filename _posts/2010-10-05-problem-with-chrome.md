---
layout:     post
title:      Just another problem with Chrome
date:       2010-10-05 05:04:00
summary:    Chrome has one serious problem when calculating number of pixels in one percent.
categories: web
---

Writing this post cuz i thought it would be useful for some web UI designers out there who deal with percent level details e.g Statistic Apps, graphs, Maps…

Chrome has one serious problem with calculating number of pixels in one percent. I was designing some UI in which i needed to divide some area in 50 parts, so i used 2% width for each part. now consider following scenario :

<blockquote>
<p>
If the total width of the area is 625px, 4% = 25px, so if you are using 1%=6px then in every 4 percent you should add 1px, Now firefox will do exactly the same. But this is not the case with Chrome, Chrome will allocate 13px to first 25 parts, and 12px to remaining 25 parts which is weird. Now if you are designing some statistics app which uses percent scale, then you are completely screwed up.
</p>
</blockquote>

Beware, Bye.