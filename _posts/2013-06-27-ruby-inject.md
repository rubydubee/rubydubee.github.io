---
layout:     post
title:      Ruby inject conditionally!
date:       2013-06-27 11:30:00
summary:    Use of inject with conditional statements in the block
categories: web
---

{% highlight ruby %}
[32] pry(main)> words = %w{mary had a little lamb}                                                                                                                                             
=> ["mary", "had", "a", "little", "lamb"]
[33] pry(main)> total = words.inject(0){ |result, word| word.size + result}                                                                                                                    
=> 18
[34] pry(main)> total = words.inject(0){ |result, word| word.size + result if word.size > 3}                                                                                                   
TypeError: nil can't be coerced into Fixnum
from (pry):52:in `+'
[35] pry(main)> 
{% endhighlight %}

This happens because inject send the result of current iteration to next iteration, so if your expression returns nil which it will when it goes to had, nil is passed to the next iteration and thus the exception! so its always a good practice to add else statement in inject! like this:

{% highlight ruby %}
[35] pry(main)> total = words.inject(0){ |result, word| word.size > 3 ? word.size + result : result}
=> 14
[36] pry(main)> 
{% endhighlight %}