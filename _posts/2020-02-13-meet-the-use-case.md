---
layout: post
title: "Meet the use-case, optimize the use-case, then scale the use-case" 
date: 2020-02-13
---
I sometimes catch myself trying to optimize early. It's the dreamer in me.  Within a codebase, this can manifest as pushing too hard on making the code [dry](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).  For design, maybe it's accounting for all the edge-cases around accessibility, or the use-cases I'm scoping for any given product release.  We as makers and builders, want to release beautiful things. It's human.

The thinking in and of itself isn't a bad exercise; in fact, I encourage it.  What isn't ideal and can be costly in aggregate is the lack of restraint in executing on those edge-cases.  Ultimately, almost every product release is an experiment, no matter how certain you are pre-release.  You honestly have no idea if it is going to work or work as you intended.  Statistically speaking, the odds are never in your favor.  

That is why speed to market with minimal execution to validate your hypothesis is the priority above all else.  If you accept that as fact, that means optimizing the code, design, or product beyond the bare minimum is statistically most likely wasted effort.  

To keep myself honest, I've adopted a simple mantra across all aspects of product development. 

* Meet the use-case 
* Optimize the use-case 
* Scale the use-case 

To make sure I'm putting the optimal focus on the right things at the right time.  But what does that mean in practice?  

When I'm writing software, this translates to my code being written for short term usage: more significant controllers, less encapsulation, some duplicate code.  However, the critical point is that it works and will allow the business to validate/invalidate the hypothesis. 

Once you have released, you can start to get feedback and modify the feature to better resonate with the launch customer segment.  That feedback allows you to hone the user experience and thus the code.  Now you can begin to see more evident domain patterns, and the code may follow.

This method isn't a silver bullet.  Nor should it be used as an excuse to author code irresponsibly (think security around customer data); however, it should help act as a reminder to optimize the system only once you know it is going to stick around for a while.  
