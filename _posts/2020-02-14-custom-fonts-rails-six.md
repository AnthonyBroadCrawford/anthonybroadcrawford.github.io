---
layout: post
title: "Custom fonts with Rails6" 
date: 2020-02-14
---

I’m working on a project that is leveraging Rails (version six) for the backend and front end (no requirement to be a single page application). Upgrading from Rails 5.X to Rails 6.X It’s been an incredibly enjoyable experience thus far.

However, with this newer version of Rails, some fundamental changes came to how the framework handles assets such as JavaScript and how it packages up the asset pipeline vs. JavaScript. Short story long, there wasn’t clear documentation on how to best manage, serve, and reference proprietary fonts in this new Rails world.

So I found a simple path and made it work. I’m not sure if my solution is the best; however, it’s simple and does not require you to bastardize the asset pipeline or webpacker in any way.

My first question; Where to store the fonts. Considering the fonts will not change, I decided to not have them compiled into the asset pipeline. It seemed like an unnecessary optimization. So I placed them in the public folder, like this ...

{% highlight files %}
public 
-- fonts
-- -- feather
-- -- -- Feather.svg
-- -- -- Feather.ttf
-- -- -- Feather.woff
{% endhighlight %}

Then in my CSS files, I reference them with the following

{% highlight css %}
@font-face {
  font-family: 'Feather';
    src:
        url('../fonts/feather/Feather.ttf?sdxovp') format('truetype'),
        url('../fonts/feather/Feather.woff?sdxovp') format('woff'),
        url('../fonts/feather/Feather.svg?sdxovp#Feather') format('svg');
    font-weight: normal;
    font-style: normal;
}
{% endhighlight %}

Given I side-step the asset pipeline  and webpacker I can leave the 3rd party css files as is.  This leaves one less moving part should I ever take an upgrade from feather.

I found a lot of other solutions when poking around the web.  Most leaned into the asset pipeline and webpacker; however, they introduced a lot of unnatural moving parts and generally seemed excessively complicated (to this webpacker novice).  Hopefully this helps save someone a few hours of poking around the web.
