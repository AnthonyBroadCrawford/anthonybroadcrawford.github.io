---
layout: post
title: "Webpacker ...... why?" 
date: 2020-02-26
---

File this under grumpy older man yells at a cloud, Webpacker, currently not a fan, and it is not a good fit for me and my current project.  

The obvious question is, "why am I using it?" however, I didn't choose the Webpacker life.  Webpacker is now a default solution for anyone using Rails 6.0  or higher.  Whether it is due to me not thoroughly RTFM (reading the fucking manual) or not currently authoring a SPA (single page application) product Webpacker is continuously in the way.

For example, a vanilla implementation of a new Rails6 project won't work on Heroku continuous integration services, and you need a lot of mucking around to make Webpacker work.  Even worse, there is almost no documentation on this, and my StackOverflow compatriots have struggled too.  This experience is the opposite of what you expect from Rails and Heroku.  Both are famous for merely just working.  

In the broader sense, I'm not exactly sure what problem it is solving for Rails write large.  How it is better than the previous solution, and what should I be taking advantage of?  Why is this not clearer?

Are most modern-day websites that heavy on client-side implementation (React, Ember, et al.)?  Or am I only missing a fundamental use-case for a Bootstrap and JQuery website Rails app?

I'll most likely rip Webpacker out here fairly soon. The only reason it is still in my app is to spin up the Rails specific JavaScript files used for Rails-based Ajax forms and links.  

Grumpy older man yells at a cloud ... 
