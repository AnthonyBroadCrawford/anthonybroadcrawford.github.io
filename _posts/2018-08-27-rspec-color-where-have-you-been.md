---
layout: post
title: "RSpec color, where have you been?"
date: 2018-08-27 24:00:00
categories: blog
---

I've been using [Ruby on Rails](https://www.rubyonrails.org) for a long time.  I first adopted [Ruby on Rails](https://www.rubyonrails.org) back in 2008 when the [Rails](https://www.rubyonrails.org) team released their first public version, 0.8.  I remember being blown away at the time. I had just co-founded my first company [Within3](https://www.within3.com) and realized that Microsoft technologies were not going to be a good fit for us.  We prototyped a few things in Rails and never looked back.  However, it wasn’t just Rails that blew me away, I was also blown away by the available testing frameworks.  

You see, I was very fortunate to have been personally mentored by [Uncle Bob Martin](https://en.wikipedia.org/wiki/Robert_C._Martin), [Michael Feathers](https://www.linkedin.com/in/michaelfeathers/), and a few of the other folks at Object Mentor when I was at [Progressive Insurance](https://www.progressive.com) around 2001.  Not only did they teach me agile, they taught me the values of [Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) and then [Behavior Driven Development](https://en.wikipedia.org/wiki/Behavior-driven_development).  Both practices have had a big impact on how I develop systems.

The whole point of me writing any of this, is I just stumbled across a feature in [RSpec](https://rspec.info) that I previously hadn’t known existed.  It’s the following options when executing your test suite that has me excited

``` bash

> $ rspec —color —format doc

```
It’s nothing major, except that it outputs your tests execution results in a very expressive manner.  When coming up to speed on a new code base or re-familiarizing yourself with some of your old code, this could be a great way to understand the intended behaviors of the system.  


``` bash

> $ rspec —color —format doc

SolidityKoans::Instructor
	When a course is  misconfigured
		Should raise an ConfigurationException
		Should log to the debug log file
		Should inform the user about the error on STDOUT

	When a course is properly configured
		Should execute the Solidity tests associated with that course
		Should parse output for compilation errors
		Should parse output for test results
		Should inform the user about any compilation errors
		Should inform the user about any failing tests


Finished in 0.0034 seconds (files took 0.28784 seconds to load)
8 examples, 0 failures

```

Give it a shot, I really like it!
