---
layout:     post
title:      Tiny MVC for J2EE
date:       2012-04-19 07:42:00
summary:    DuelGround was quite an experience for me, during its development, I wrote this MVC pattern for J2EE to use on Google App Engine 
categories: java
---

I know, you already know how to write MVC pattern using Servlet/JSP. But I didn’t and I know many of you still don’t know it! People use frameworks even if they don’t want that much complicated setup. For smaller webapps, you don’t need frameworks, they make your app heavy. So mind it and let’s start writing our own MVC pattern using Servlet/JSP. 

First of all, you need a Controller, yes only one! Let’s call it a FrontController(because some frameworks do). All the requests should come to this controller and it will then delegate these requests to individual Action(again!). Actions take care of processing a request and dispatching it to the JSP, which acts as a view.

Let’s add our FrontController in web.xml

{% highlight xml %}

# web.xml
<servlet>
    <servlet-name>FrontController</servlet-name>
    <servlet-class>com.myapp.FrontController</servlet-class>
</servlet>
 
<servlet-mapping>
    <servlet-name>FrontController</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>

{% endhighlight %}

and now lets write our FrontController, Action and a sample AccountAction which will display user’s name through account.jsp.

{% highlight java %}

// FrontController.java
package com.myapp;
 
public class FrontController extends HttpServlet{
 
    private Map<String, Action> actions = new HashMap<String, Action>();
    
    public void init()
    {
        this.actions.put("/account.do", new AccountAction());
    }
    
    public void takeAction(HttpServletRequest  request, HttpServletResponse response) 
            throws ServletException, IOException
    {
 
        String reqUri = request.getRequestURI();
 
        Action action = (Action)actions.get(reqUri);
 
        action.process(request, response);
    }
    
    public void doPost(HttpServletRequest  request, HttpServletResponse response) throws ServletException, IOException
    {
        takeAction(request, response);
    }
 
    public void doGet(HttpServletRequest  request, HttpServletResponse response) throws ServletException, IOException
    {
        takeAction(request, response);
    }
    
}

{% endhighlight %}

Here we map a request to its corresponding action, I have done this using requestURI(), you can use different techniques as well, like sending hidden parameter “action”. But this way looks much cooler to me.

{% highlight java %}

// Action.java
public interface Action
{
    public void process(HttpServletRequest request, HttpServletResponse response) 
                    throws ServletException, IOException;
}

{% endhighlight %}

{% highlight java %}

// AccountAction.java
public class AccountAction implements Action
{
 
    @Override
    public void process(HttpServletRequest request, HttpServletResponse response)
      throws ServletException, IOException {
    
        //do your processing here, take values from DB, may be!
        SomeBean user = new SomeBean("Paddy")
    
        //Very important. we are setting attribute to the request and forwarding that request.
        // forward will keep the request intact unlike redirect!
        request.setAttribute("user", user);
        ServletContext context = request.getSession().getServletContext();
        context.getRequestDispatcher("/WEB-INF/users/account.jsp").forward(request, response);  
    }
}

{% endhighlight %}

AccountAction process your business logic, it will communicate with your Database layer and create a bean of your data(or may be multiple beans). The request will now be dispatched to the jsp file(your View) and along with it goes your data(bean) which is set on the request through setAttribute(..)

{% highlight jsp %}

// account.jsp
Howdy, ${user.name}!

{% endhighlight %}

Try this! Extending this application is very easy and maintaining it will be even more easier! Happy Coding!