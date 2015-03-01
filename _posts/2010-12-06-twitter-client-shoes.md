---
layout:     post
title:      Twitter Client in Shoes
date:       2010-12-06 12:16:00
summary:    Here is the twitter client I am making while learning Shoes(A GUI toolkit for Ruby).
categories: ruby
---

### Prerequisites

1. I hope you have [Shoes](https://github.com/shoes/shoes/downloads) installed on your machine, operating system does not matter :)
2. oauth gem (`gem install oauth` will do the trick or `sudo` if you have to!)
3. json gem as well(to parse the response) (`gem install json`)

### Let’s start
1. Register your application at [Twitter](https://dev.twitter.com/), give some cool name like “Rubydubee”(not this x-( ).
2. Lets first write our own library (don’t worry you can just copy the [code](https://github.com/rubydubee/Twitter-Exploits/blob/master/Twitter.rb) for now :) ), But you can try to understand that… very small & simple if you know the OAuth protocol.(the oauth gem does all the magic.)
3. Let’s write our Application

{% highlight ruby %}
#Main file
 
require "Twitter.rb"
require 'json'
require 'uri'
 
Shoes.app :width=>300, :height=>400, :title=>"Rubydubee" do
  @twitter = Twitter.new "consumer_key",
                   "consumer_secret"
  @main_stack = stack do
  caption "Rubydubee : Update your status"
  image "twitter.png", :click => "#{@twitter.get_request_token.authorize_url}"
  inscription "Sign with Twitter first and then Authrize your PIN."
  button "Authorize" do
   pin = ask("Please allow access and enter the PIN : ")
   @access_token = @twitter.get_access_token(pin.chomp)
   data = @access_token.get("/account/verify_credentials.json").body
   debug("getting some data ")
   result = JSON.parse(data)
   username = result['name']
   @main_stack.append do
    caption "Howdy #{username}!"  
    edit_box :width=> 250, :height=>100 do |e|
     @count.text = e.text.size
     @str = e.text
    end
    @count = strong("0")
    para @count," characters"
    button "Tweet" do
     new_status = URI.escape(@str, Regexp.new("[^#{URI::PATTERN::UNRESERVED}]"))
     @access_token.post("/statuses/update.json?status=#{new_status}").body
     alert("Status Updated!")
    end
   end
  end
  end
end
{% endhighlight %}

The code is intuitive! isn’t it? if you need any help plz comment! 
After you run this “shoes main.rb” it will look somewhat like this :

![UI](/images/upload1.gif)

![Tweet](/images/updatedstatus.gif)

Try adding error handling, better design and UI, and of course more features.

Keep reading!!! :)