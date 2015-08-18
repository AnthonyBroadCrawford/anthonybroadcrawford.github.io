---
layout: post
title:  "Making an Apple Watchkit App: API calls"
date:   2015-08-16 21:11:51
categories: WatchKit iOS Apple Swift API
tags: WatchKit iOS Apple Swift API
---

I recently built an Apple [WatchKit][WatchKit] app for my company [Fooda][Fooda].  The app allows our
customers to track their food delivery once they've placed an order.  Our app, like most apps
needed to ask our iOS app to make a remote http call to our api to retrieve the necessary info
to display the data.

Given almost all Watchkit apps will need to do exactly this, it should have been a walk in the park
that is thoroughly documented by [Apple][Apple].  However, I discovered this is in fact not the case
and if this is your first time building a [WatchKit][WatchKit] app, it's ultimately simple with a few gotchas.  

This post is going to be one of a few posts documenting the gotchas I encountered and comprehensive solutions to
resolve them.

# Error Domain=com.apple.watchkit.errors Code=2
If you run into any kind of trouble that's going to make you tear your hair out, it's this error.  Your
[WatchKit][Watchkit] app is going to receive this error if your iOS app fails to communicate with it properly.  
This you'll find out can happen for many reasons.

- Your iOS app crashed handling the [WatchKit][Watchkit] request
- You are passing in-compatable datatypes back to the [WatchKit][Watchkit] app
- Your app is marked as unable to run in the background
- You fail to respond to the [WatchKit][Watchkit] app
- You fail to respond to the [WatchKit][Watchkit] app quickly

Each of these errors are easy to prevent, but utltimately not documented in any one single place.

# Making a call from Watchkit to your iOS app
To communicate with your iOS app from your [WatchKit][WatchKit] app you are going to do so through the
`WKInterfaceController`.  This function allows you to package up a request in the form of a dictionary and
a closure that is passed to your iOS app.

{% highlight swift %}
override func awakeWithContext(context: AnyObject?) {
     super.awakeWithContext(context)

     let request : [NSObject:AnyObject] = ["request":"glance_data"]

     WKInterfaceController.openParentApplication(request, reply: { (reply, error) -> Void in

     })
 }
{% endhighlight %}



{% highlight swift linenos %}
let request = ["request":"app_data"]

WKInterfaceController.openParentApplication(request, reply: { (reply, error) -> Void in


})
{% endhighlight %}


{% highlight swift %}
func application(application: UIApplication, handleWatchKitExtensionRequest userInfo: [NSObject : AnyObject]?, reply: (([NSObject : AnyObject]!) -> Void)!) {
    let backgroundProcessingToken = application.beginBackgroundTaskWithName("backgroundApiCall", expirationHandler: { () -> Void in
        reply(["response":["error":"SOME_ERROR_CODE_INDICATING_TIMEOUT"]])
    })

    request(.GET, "https://api.forecast.io/forecast/[INSERT DARK SKY API CODE]/37.8267,-122.423").responseJSON(options: NSJSONReadingOptions.AllowFragments, completionHandler:{request, response, data, error in
        if(error != nil || data == nil){
            reply(["response":["error":"SOME_ERROR_CODE_INDICATING_FAILURE"]])
        }

        if let json = data as? NSDictionary {
            reply(["response":["data":json]])
        }

        application.endBackgroundTask(backgroundProcessingToken)
    })
}
{% endhighlight %}

[Apple]:      http://developer.apple.com
[WatchKit]:   http://developer.apple.com/watchkit/
[Fooda]:      http://www.fooda.com
