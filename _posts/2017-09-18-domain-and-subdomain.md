---
layout: post
title: Domain and Subdomain
comments: true
tags:
- Etc
---

What is the difference between domain and subdomain? Let's consider `myapp.com` and `welcome.myapp.com`. `myapp` is the mid level domain while `.com` is the top level domain and `welcome.myapp.com` is the subdomain while `myapp.com` is the domain. If we want to deploy an app to this domain, the best practice is that we redirect `myapp.com` to `www.myapp.com` so that `www` becomes a default subdomain. However, what if we would like to deploy multiple apps to the domain? Simply, we can treat other subdomains similar to `www`.
- ```www.myapp.com``` is a landing page using wordpress or something else
- ```app.myapp.com``` is where your app exists
- ```api.myapp.com``` provides api docs for devs or open source community
- ```blog.myapp.com``` provides or links to a blog service such as Medium or Ghost
- ```support.myapp.com``` provides or links to a customer service such as UserVoice or GetSatisfaction

# Your Own Server
Set up nginx.conf. See more at 
- <https://stackoverflow.com/a/9905617>

# Wildcard Domains
It supports that no matter what subdomains are requested, you allow all ```*.myapp.com``` and redirect to your app.

# On Rails
You can support personalized domains. See more at  
- <http://railscasts.com/episodes/221-subdomains-in-rails-3>
- <https://richonrails.com/articles/basic-subdomains-in-ruby-on-rails>
