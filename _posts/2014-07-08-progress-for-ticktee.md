---
layout: post
title: ! "Progress for TickTee"
categories: program ticktee
---

Today i just finished the authentication using devise. Now the user need to login to modify their project and cannot see the project belongs to other user.

Some Notice:

1. use belongsto with hasmany or has_one together

2. userprojectspath(user, project) and userprojectpath(user, project) is quite different.

3. When database changes and push to heroku, the database may need to be reset with following step:
{% highlight java %}
heroku login
heroku pg:reset DATABASE
heroku run rake db:migrate
{% endhighlight %}
4. Testing Devise with capybara and RSpec is a little complex. But Thanks, Schneems, your article give me some tips [speed up capybara tests with device][capybara-url].

[capybara-url]: http://www.schneems.com/post/15948562424/speed-up-capybara-tests-with-devise/
