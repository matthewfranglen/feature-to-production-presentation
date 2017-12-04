Overview
--------

This will cover how a feature makes it into production.
The audience is the Masters students of Sussex University.
They are good theoretically however have limited practical experience.

 - How are features created and defined
 - Implementation
    - Design
    - Coding
    - Testing
    - Review
 - Releasing
    - Hardware
    - Deployment Languages
 - Reviewing
    - User uptake
    - Feedback

Introduction
------------

I'm Matthew from Brandwatch.
Brandwatch ...

This will cover the way that ideas become reality.

 * How to choose what to build
 * How to build it
 * How to release it

Will use a project from Brandwatch to demonstrate each of the steps.

Features
--------

How do you know what to build, or what to change?

Every time you decide to build something, you have constraints.
The constraints can be things like money, time or expertise.

Even startups have constraints.
They can't build something if they don't know how.
They have to be able to sell what they create.
Choosing to build something is a constraint too - it means you can't build something else.

The best thing to build is the one that makes the most money for the least cost.
You need to find what is cheap for you and valuable to others.

Finding out what would be best to build is hard.

You can find things to build in many ways

 - You may have a strong idea about what your startup is for, and you still need to fill in some missing functionality
 - You can get ideas from existing users of your application, through bug reports and suggestions
 - You can evaluate competing products

Then each idea needs to be valued.
The best valuation is to be able to sell the product before creating it.
Everything else is an estimate.
Estimation is hard.

Finally you need to know how much work it is.

If you have all of this then you can work out which idea has the greatest benefit for the least cost.
That would be a good thing to build!

Estimating the work required for an idea is hard, and it's likely you will get involved at this point.

### UHD

At Brandwatch we get the Decahose.
This is 10% of all public tweets.
We were already collecting this data, and we wanted more value out of it.

Our main product is the ability to analyse what people are saying on social media.
It's very helpful to be able to look at how discussion around a topic has changed over time.
We can only easily provide what we collected in the past, which may not cover the topic well.

The Decahose is a statistically valid sample of all topics discussed on twitter, so using that would help a lot.
The data for this would be cheap as we already had it.
At this point I was asked about how long it would take to add this.

Exploration and Design
----------------------

So at this point I had to design a solution and determine how much work would be involved.
This is probably the most important part because a good design has such a great effect on the performance, maintainability and cost of the idea.

Implementation
--------------

This ends up being a loop.

Explore & Design
Estimate
Implement
Test
Evaluate
Repeat


