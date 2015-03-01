---
layout:     post
title:      Flash cookies finally found its use!
date:       2012-07-17 05:03:00
summary:    Flash cookies are pretty useful in Rails, here are couple more amazing use cases.
categories: rails
---

Ola folks!

We have all used [flash cookies](http://guides.rubyonrails.org/action_controller_overview.html#the-flash) in rails and I am sure you liked them. Let me tell you, after reading this, you will like them even more!

Flash is a part of session which will be cleared with every request, so whatever values you have set in the flash will not be available on the next request. That is why most rails developers use it to store error/success messages for that request. Let me show another couple of more useful use cases where you can use flash storage :).

1. You have an internal groups or communities which user can join. Now a guest user comes to a group page and clicks on join. User will be redirected to the Login page and after login, he will have automatically be joined the group and redirected to the group page again. How will you achieve this? One simple and working approach is to append request parameter like `group_id` to login request, which doesn’t look good(I don’t find it attractive to have a `group_id` variable attached to login url). The other option is to set session cookie for `group_id`. The drawback is, if user changed his mind, he will not login and browse around the site. But let’s say without closing the browser, he logs in after a while! Boom! He is now a part of that Group and he will be redirected to groups page because the session cookie was there and not cleared when he left the login page. So there is your use case, you want a storage which will be cleared if user decides not to login and browse different page instead. So instead of setting session cookie, set a flash!
2. This use case is for developers using authlogic for user authentication. When user enters wrong credentials on his login page, he is redirected to `/user_sessions` (i.e. POST request to `user_sessions_controller`) and he will be shown the error messages and login page will be rendered there. All is well, but the URL is still `/users_sessions`, which it should not be(it should be `/login` or something). So now I went inside `user_sessions_controller` and for failure condition, redirected the request to `login_path` instead of rendering the login form. What bad happened is I lost the `@user_session` object and thus all the `error_messages`. So there is your use case. Set the `@user_session` object in `flash` and retrieve it on login( `new` method ).

Thanks for reading this far!