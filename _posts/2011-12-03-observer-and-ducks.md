---
layout:     post
title:      Observer and Ducks
date:       2011-12-03 03:29:00
summary:    Nice and simple example of how to implement Observer pattern in Ruby
categories: ruby
---

If you have implemented Observer pattern in any statically typed language, you know that you need an explicit Observer class/interface for the pattern, which will be extended by the actual concrete classes and some ‘update’ kind of method should be overridden in order to propagate the observations.Basically taking advantage of inheritance.

Here comes a duck type language like Ruby, as there are no types to be declared you can pass in any object to the observable and let it call the update method, you don’t need any parent Observer class. If the method is available in the object it will be called and the observation will be accomplished.

Now we can extend this more, The observable object will call any method it wants and ruby’s method_missing will take care of the methods that the individual observers don’t want to define. Cool eh? Check out this simple example:

{% highlight ruby %}

# observable.rb
class Observable
  def initialize
    @observers = []
  end
 
  def observe(o)
    @observers << o
  end
 
  def called
    puts "Observable is being called"
    @observers.each { |o| o.update }
  end
end

{% endhighlight %}

{% highlight ruby %}

# observer1.rb
class Observer1
  def initialize(obs)
    obs.observe(self)
  end
 
  def update
    puts "Okay! Observing in Observer1"
  end
end

{% endhighlight %}

{% highlight ruby %}

# observer2.rb
class Observer2
  def initialize(obs)
    obs.observe(self)
  end
 
  def update
    puts "Okay! Observing in Observer2"
  end
end

{% endhighlight %}

{% highlight ruby %}

# main.rb
$: << File.join(File.dirname(__FILE__), "/")
 
require "Observable.rb"
require "Observer1.rb"
require "Observer2.rb"
 
observable = Observable.new
o1 = Observer1.new(observable)
o2 = Observer2.new(observable)
 
observable.called

{% endhighlight %}

So I quote this again 
> If it walks like a duck and quacks like a duck, it must be a duck.