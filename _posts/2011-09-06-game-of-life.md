---
layout:     post
title:      Game of Life in Ruby
date:       2011-09-06 05:09:00
summary:    Recently, I was asked to write Game of Life in Java in one of the interview questions, here is the ruby version of the same
categories: ruby
---

{% highlight ruby %}

=begin
Whenever we visit the cell in the pattern, we call this method 
to get the next state of the cell when the Tick happens.
Here, we add the values of all the eight cells around the visited cell
and apply the game_of_life rules as specified in the comments below.
=end
def next_gen(seed, row, col)
  next_state = 0
  (-1..1).each do |xdir|
    (-1..1).each do |ydir|
      if(xdir == 0 and ydir == 0)
        #do nothing
      else 
        if((col+xdir).between?(0,5) and (row+ydir).between?(0,3))
          next_state += seed[row+ydir][col+xdir]
        end
      end
    end
  end

  #apply rules now  
  if(seed[row][col] == 0)
    if(next_state == 3) 
      #dead cell resurrects if it has exactly 3 live neighbours
      next_state = 1
    else
      next_state = 0
    end
  else
    if(next_state == 2 or next_state == 3) 
      #live cells remains unchanged if they have 2/3 live neighbours
      next_state = 1
    else
      # or they die
      next_state = 0
    end
  end
  return next_state;
end

def display(seed)
  seed.each do |row|
    row.each do |col|
      print col
    end
    puts ""
  end
end

def onTick(seed)
  next_generation = []
  seed.each_with_index do |row, row_index|
    nextrow = []
    row.each_with_index do |cell, cell_index| 
      nextrow[cell_index]= next_gen(seed, row_index, cell_index)
    end
    next_generation[row_index]= nextrow
  end
  return next_generation
end

#Considers an extra layer of dead cells i.e 0s around the actual pattern e.g.
=begin
if the pattern is 
-xxx
xxx-

then the input must be
------
--xxx-
-xxx--
------
=end
seed = [ [0,0,0,0,0,0],[0,0,1,1,1,0],[0,1,1,1,0,0],[0,0,0,0,0,0] ]

puts "Input Pattern : "
display seed

puts "Here is the Output : "
display(onTick(seed))

{% endhighlight %}
