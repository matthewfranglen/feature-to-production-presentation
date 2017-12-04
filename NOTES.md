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

The desired design for this is quite loose.
We just need to be able to determine how long it would take to build.
Estimation is hard though, so this investigation can take some time.

The first thing to understand is the idea.
This sounds simple but it is not.
You need to understand the technical details of the idea.

You then need to work out how that can fit within the existing system.
If you are creating something fresh then the existing system is the world.
When adding to an application it is about understanding where the idea will fit.
The placement depends on what the idea requires, and what it provides.

### UHD

So at Brandwatch the system works using queries.
A query is a way to get information about a topic of discussion.
To display the details of a discussion we read the query.

We wanted to add the Decahose data to what the user can see.
So altering the code that returns the data is required.
Since we want to be secure, and to alter the data that is returned, we can sit between the security boundary and the original query reader.

### Internal Design

Once you have identified the location of the change, you then need to work out what you actually need to change.

A poor design for the change can have a great impact on the performance of the application.
If you operate over and store data inefficiently then your application can become very slow as soon as you get a large amount of data.
Making a lot of network requests is also very time consuming.

Since this is a course on computer science I am going to assume you already know about what efficient code is.
At this level of design you are dealing with entire data structures and algorithms as blocks.
Just consider what you need, and then choose the best performing parts.

It can help to think of this like a flow chart.
The boxes are things that produce, transforming or store data.
The arrows are the flows of data.

### UHD

The Decahose is only 10% of the public tweets.
The historic data we have may be complete.
We can't just mix them as that would under represent the twitter conversations.

We need to be able to boost the twitter data to 100%, without altering anything else.
Since we are dealing with queries that means we need two queries to allow us to alter one but not the other.

This means that our service needs to be able to take two queries, boost the twitter data, and then merge the results.
If we treat boosting and joining as queries that reference other queries then we are always dealing with queries which means that the code that calls us does not need to be aware of the change.

If we think of regular Queries as leaf nodes and the Join and Boost queries as internal nodes we can see that we have a graph.
Since we control the creation of it, we can guarantee that it is a Directed Acyclic Graph.
This means we can operate over it recursively - a Join query does not have to know the type of queries it operates over.

The final thing to consider is that the requests for data desire different things.
It would be totally impractical to return all of the data available and have the front end perform aggregations over it.
So instead the database is used to do this.

The aggregations that are performed may need to be joined or boosted in different ways.
It's not appropriate to blindly boost the number of unique authors for example.

### Estimation

It's really hard.
You have to try to:

 - Split the work into manageable units
 - Determine the size of each unit
 - Add it all up
 - Vastly increase the total
 - Still be wrong

Finding an accurate way to estimate implementation of an idea is really hard.
If you find a solution to this then you can make millions.

It's hard because you are always doing something new.
If it is something old then you can use your old solution for it.
That means you are always estimating something you don't know how to do.

Implementation
--------------

This ends up being a loop.

Explore & Design
Estimate
Implement
Test
Evaluate
Repeat


