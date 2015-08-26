---
layout: post
title:  "Debugging WatchKit iOS handoff"
date:   2015-08-21 21:11:51
categories: Electrical-Engineering Apple iOS WatchKit Swift
---

I recently built an Apple iOS [WatchKit][WatchKit] app and part of that was improving
the user experience by leveraging Apple's [Handoff][Handoff] technology.

As seems to be the `pattern` lately with [Apple's][Apple] [WatchKit][WatchKit] documentation, this turned
out to take a few hours longer than it should have.

If you are trying to enable [Apple's][Apple] [Handoff][Handoff] and it isn't working, it's most
likely because one of the three steps below wasn't completed.

# Three simple steps to handoff
In the end, it's actually quite simple and [Handoff][Handoff] comes down to a few small changes

- Declaring what activities your iOS app can have "[Handed off][Handoff]"
- Handing off from your [WatchKit][WatchKit] app to your iOS app
- Receiving and processing the [Handed off][Handoff] in your iOS app

Although this example is from [WatchKit][WatchKit] to iOS, you could do this from iOS to OSX.

# Declaring activities
Ultimately, not just "anything" can be handed off to your iOS app.  In order to get the hand off
icon to appear on the iOS app, your iOS app must `declare` which `activities` it can handle.

To declare activities, you add the `NSUserActivityTypes` key to your apps plist file as an array,
and then give meaningful names to the activities your app can handle.

![NSUserActivityTypes Screenshot]({{ site.url }}/assets/images/NSUserActivityTypes-info.plist.png)

Keep in mind, these are just symbolic names that you will reference from the [WatchKit][WatchKit] app
and nothing more.  So name them whatever makes the most sense to your app and these activities.

# Handing off to your iOS app

In our [WatchKit][WatchKit] example below, when a customer slides up the [WatchKit][WatchKit]
app's Glance, we are going to initiate a hand off to iOS

{% highlight swift %}
override func awakeWithContext(context: AnyObject?) {
  super.awakeWithContext(context)

  let userInfo : [NSObject:AnyObject] = ["handoff_activity":"login"]

  updateUserActivity("com.YourApp.YourDomain.loginHandoffAcivity",
    userInfo: userInfo,
    webpageURL: nil
  )
}
{% endhighlight %}

At this point, [Handoff][Handoff] still won't work, as we haven't "caught" the
`activity` handed to our iOS app.  However, this completes the`request` to
[Handoff][Handoff] from the [WatchKit][WatchKit] app.

# Preparing your app for iOS Handoff
First you need to update your iOS app `AppDelegate`.  Specifically, you will need to add the `continueUserActivity` method.

{% highlight swift %}
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]!) -> Void) -> Bool {
    restorationHandler([self.window!.rootViewController!])
    return true
}
{% endhighlight %}

There are a few things in the above code snippet to notice, specifically the call to `restorationHandler`.  The `restorationHandler` closure is the final step to make the [Handoff][Handoff] icon appear
on the iPhone's lock screen.

# Bringing it all together

These have been trivial examples, so ultimately you're most likely going to do something like
the following

{% highlight swift %}
override func awakeWithContext(context: AnyObject?) {
  super.awakeWithContext(context)

  let request : [NSObject:AnyObject] = ["request":"user_logged_in"]

  //Send a request to your iOS app, most likely to fetch some remote data
  WKInterfaceController.openParentApplication(request, reply: { (reply, error) -> Void in
    //This is a hackish simplified short hand, you'll need to verify the response in the reply hash and parse out if the user has logged in
    let user_logged_in = reply["response"]

    if(user_logged_in == false){
      //you can populate this hash with any relevant handoff or user data
      let userInfo : [NSObject:AnyObject] = ["handoff_activity":"login"]

      updateUserActivity("com.YourApp.YourDomain.loginHandoffAcivity",
        userInfo: userInfo,
        webpageURL: nil
      )

      //inform the user on your Glance UI that they need to log
      some_func_to_display_the_login_ui_vs_the_normal_ui()
    }
  })
}
{% endhighlight %}

[Apple]:      http://developer.apple.com
[WatchKit]:   http://developer.apple.com/watchkit/
[Fooda]:      http://www.fooda.com
[Handoff]:    https://developer.apple.com/handoff/
