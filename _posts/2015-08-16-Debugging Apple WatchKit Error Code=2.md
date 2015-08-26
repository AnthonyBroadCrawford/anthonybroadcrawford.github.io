---
layout: post
title:  "Debugging Apple WatchKit 'Error Code=2'"
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

Yet, despite this being a core task of almost every app, it's easy to go astray and end up with errors.  Specifically the `Domain=com.apple.watchkit.errors Code=2` error.  This post will help you diagnose adn avoid it.

# Error Domain=com.apple.watchkit.errors Code=2
If you run into any kind of trouble that's going to make you pull your hair out, it's this error.  Your
[WatchKit][Watchkit] app is going to receive this error if your iOS app fails to communicate with it properly.  Specifically, it has failed to call the `reply` closure handed to it from the [WatchKit][WatchKit] app.  

This you'll find out can happen for many reasons.

- Your iOS app crashed handling the [WatchKit][Watchkit] request
- You are passing in-compatable datatypes back to the [WatchKit][Watchkit] app
- Your app is marked as unable to run in the background
- You fail to respond to the [WatchKit][Watchkit] app
- You fail to respond to the [WatchKit][Watchkit] app quickly

Each of these errors are easy to prevent, but utltimately not documented in any one single place.

# Making a call from Watchkit to your iOS app
To communicate with your iOS app from your [WatchKit][WatchKit] app, you are going to do so through the
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

This is the simple part, and shouldn't cause too many issues.  However, it's making sure you properly reply to your [WatchKit][WatchKit] that will determine if you receive the `Domain=com.apple.watchkit.errors Code=2` error or not.

# Make sure your app can run in the background
This shouldn't be an issue, as most apps out of the box are allowed to do this.  However, if for somereason your app is set to prevent background processing, this will result in your `WKInterfaceController` receiving the `Domain=com.apple.watchkit.errors Code=2` error.

To verify this, in your `targets` `plist` make sure `Application does not run in background` is set to `NO`.

# Add the handleWatchKitExtensionRequest in your AppDelegate
Implement the `handleWatchKitExtensionRequest` method in your `AppDelegate`.  This method handles all requests from your [WatchKit][WatchKit] app.  If you haven't implemented this method on your iOS apps AppDelegate your [WatchKit][WatchKit] will receive the `Domain=com.apple.watchkit.errors Code=2` error.

{% highlight swift %}
func application(application: UIApplication, handleWatchKitExtensionRequest userInfo: [NSObject : AnyObject]?, reply: (([NSObject : AnyObject]!) -> Void)!) {
  //Handle the WatchKit request here
}
{% endhighlight %}

#Always call the reply closure
Ultimately, the reason your [WatchKit][WatchKit] app is receiving the `Domain=com.apple.watchkit.errors Code=2` error is because you failed to call the `reply` closure.  So, no matter what happens, ensure you invoke the `reply` closure.

{% highlight swift %}
func application(application: UIApplication, handleWatchKitExtensionRequest userInfo: [NSObject : AnyObject]?, reply: (([NSObject : AnyObject]!) -> Void)!) {
  reply(["response":["status":"success", "data":"hello world"]])
}
{% endhighlight %}

# Running in the background
If your Watch user is glancing at their watch, most likely they aren't also looking at your iOS app.  That means your iOS app just woke up in the background, and needs to register to run in the background.  If not, iOS will most likely terminate your app before you can invoke the `reply` closure.

{% highlight swift %}
func application(application: UIApplication, handleWatchKitExtensionRequest userInfo: [NSObject : AnyObject]?, reply: (([NSObject : AnyObject]!) -> Void)!) {
  let backgroundProcessingToken = application.beginBackgroundTaskWithName("backgroundApiCall", expirationHandler: { () -> Void in
      reply(["response":["status":"error", "data":"BACKGROUND_JOB_EXPIRED__OH_NO"]])
  })

  reply(["response":["status":"success", "data":"hello world"]])
  application.endBackgroundTask(backgroundProcessingToken)
}
{% endhighlight %}

This becomes even more important if you are making an API call which can take a material amount of time.  

# Bringing it all together
Here's an example pulling all the components together between a [WatchKit][WatchKit] app and it's corresponding iOS app.

{% highlight swift %}
override func awakeWithContext(context: AnyObject?) {
     super.awakeWithContext(context)

     let request : [NSObject:AnyObject] = ["request":"glance_data"]

     WKInterfaceController.openParentApplication(request, reply: { (reply, error) -> Void in
       if(error == nil){
         //Yeah, you have a response of some sorts from your iOS app.  
       }
     })
 }
{% endhighlight %}

Here's the iOS `AppDelegate` `handleWatchKitExtensionRequest` implementation

{% highlight swift %}
func application(application: UIApplication, handleWatchKitExtensionRequest userInfo: [NSObject : AnyObject]?, reply: (([NSObject : AnyObject]!) -> Void)!) {
    let backgroundProcessingToken = application.beginBackgroundTaskWithName("backgroundApiCall", expirationHandler: { () -> Void in
        reply(["response":["error":"SOME_ERROR_CODE_INDICATING_TIMEOUT"]])
    })


    Alamofire.request(.GET, "https://api.forecast.io/forecast/[INSERT DARK SKY API CODE]/37.8267,-122.423").responseJSON(options: NSJSONReadingOptions.AllowFragments, completionHandler:{request, response, data, error in
        if(error != nil || data == nil){
            reply(["response":["error":"SOME_ERROR_CODE_INDICATING_FAILURE"]])
        }

        reply(["response":["status":"error", "data":data]])
        application.endBackgroundTask(backgroundProcessingToken)
    })
}
{% endhighlight %}

[Apple]:      http://developer.apple.com
[WatchKit]:   http://developer.apple.com/watchkit/
[Fooda]:      http://www.fooda.com
