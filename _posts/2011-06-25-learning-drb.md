---
layout:     post
title:      Learning DRb
date:       2011-06-25 10:31:00
summary:    Trying to create a tiny ruby application which will let you run a terminal command on another machine from your machine.
categories: ruby
---

Hello all,

Its been a hectic month for me, many things happened, good and bad(especially about home loan, I am going to write a blog post about that in Vartool.). One of the good things that happened is that I met few Ruby enthusiasts and I have decided to write some cool Ruby program again! So without wasting anymore time, lets go…

#### Scenario
I am working on my computer(A) and my teammate is working on a shared computer(B), Now i have some urgent work on computer B, lets say i have to read some code i have written yesterday. Now I can’t go to the machine and copy that code(because the machine is remotely located .. geographically) and i can’t remotely access it through remote desktop because i don’t want to disturb him. What should I do?

Well don’t worry, NaradMuni is here.

#### What is NaradMuni 
NaradMuni is very small (tiny) ruby application which will let you run a terminal command on another machine from your machine.

#### How 
Using DRb. Well, I have written a DRb server and client, you run the server on the shared machine(Computer B from the scenario) and client on your machine. The client will accept the commands you want to run on the server and send it to the server using DRb. Server then runs the command using IO.popen and return the result to the client. Simple!

Here is an output, Server is running on my machine(pdandwate) and client on another(kalea, thanks to Kale for letting me use his machine ;) ) :

![Output](/images/padd_drb.png)

<del>I will be committing the code soon</del>(I lost that code, but its pretty easy, you can figure it out, google for DRb), till then Cya, bye!