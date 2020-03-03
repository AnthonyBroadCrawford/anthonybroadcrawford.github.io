---
layout: post
title: "Webpacker can’t find application in /packs-test/manifest.json" 
date: 2020-03-02
---

I recently spent an excessive amount of time debugging a Webpacker issue on Heroku’s Continuous Integration server (that server that you can associate with a pipeline). Ultimately, there was a shockingly simple solution, so here it is, and that took I hope this helps someone else in the future. 

#Webpacker can’t find application in /packs-test/manifest.json 
With Rails6 all web applications (not api only) now leverage Webpacker to manage the JavaScript aspect of the asset pipeline. It seems to get the job done. That was until my integration tests got picked up by my continuous integration server on Heroku. 

Once those locally passing tests were committed to the repo, they began failing on Heroku with the following error. 

{% highlight bash %}
Failure/Error: <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
    
    ActionView::Template::Error:
       Webpacker can't find application in /app/public/packs-test/manifest.json. Possible causes:
       1. You want to set webpacker.yml value of compile to true for your environment
          unless you are using the `webpack -w` or the webpack-dev-server.
       2. webpack has not yet re-run to reflect updates.
       3. You have misconfigured Webpacker's config/webpacker.yml file.
       4. Your webpack configuration is not creating a manifest.
       Your manifest contains:
       {
       }

{% endhighlight %}

Searching through all the usual suspects didn't yield any significant results. Merely the typical, "it's fixed now, closing issue."  

However, I was able to dig through the continuous integration server logs and noticed that the test-run setup logs didn't state the construction of the asset pipeline. On a pure whim from that hunch, I added the following `app.json`

{% highlight json %}

 "scripts": {
    "test-setup": "bin/rails assets:precompile"
    }

{% endhighlight %}

And it did the trick!  

I'm still digging into why I needed to tell Heroku to build the asset pipeline as I haven't had to do that on any other Heroku application and/or pipeline to date.  My hunch is that it is a new behavior intersecting between Rails6, Webpacker, and changes to Heroku.  I'll continue to research and update once I know why this changed.

`¯\_(ツ)_/¯`
