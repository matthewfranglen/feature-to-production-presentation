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

### UHD

I came up with an estimate for the Decahose work.
It was wrong.

If I had given an accurate estimate then the work may not have been done.
The size of the estimate is the life or death of the idea.

At this point the project proceeded so lets talk about getting the work done.

Implementation
--------------

The way in which ideas have been built has changed over the years.

### Waterfall

At the beginning it was the norm to work in stages and for progress to be measured by completing stages.
You would never return to a previous stage.

Broadly speaking the stages were:

 - Design
 - Implementation
 - Testing
 - Release

This was termed waterfall development, as it is very easy to go down a waterfall and much harder to go back up.
This mirrors the idea that you progress through these stages and don't need to return to earlier ones.

This system seems to mirror civil engineering in the real world.
When you build a bridge you don't need to redesign it half way through building it.
However the idea of a bridge is an old idea - you are just making a new one in a different place.
How similar is software development to that?

### Knowledge

The problem with that approach is the amount of knowledge you have of the product that you are about to build.
You know the least about it at the start.
You know the most at the end.
You are most able to design it when you have finished building it.

That means that the design you will come up with when you have the least knowledge will be the worst design.
Since you will be building new things each time you won't be able to just copy an existing blueprint.

Fundamentally, ideas are usually poorly understood and woefully incomplete.
A computer cannot work with ambiguous code, it must be precise.
The act of implementing the idea requires the idea to become more precise.

### Agile

It would be best to postpone design until the end.
Obviously you can't do this entirely.
You can do this partially.

You need a high level design.
Once you have that you can then focus on the part you will build next.
When you have done that you can think about the next section.
Your design for the next section will be improved by the knowledge about the idea you gained by making the current section.

This ends up being a loop.

 - Explore & Design
 - Estimate
 - Implement
 - Test
 - Evaluate
 - Repeat

The aim is to work in complete, releasable, chunks.

#### Explore / Design / Estimate

When you are in this loop the work has already been broken down into sections.
These sections are sometimes called stories.

To implement a story you need to determine what is required, design a solution for that, and then estimate it.
This is the same process that was done for the overall idea.
This time it is done on a far smaller and more specific area.

The reason that you do this is to determine if the story is specific enough, and to schedule it.
If the story looks like a lot of work then it probably needs to be split up further.
The aim is to get small stories that you can complete quickly.

Small stories allow for measurable progress and avoid getting bogged down by surprises.
A story should be a single piece of functionality that would be useful to the user.
This tends to limit how small they can be.
If you don't provide user value then what are you going to evaluate?

#### Implement

Actually writing the code will be one of the most enjoyable parts.
If your team is well managed you should be able to code in solid blocks of time with a minimum of interruption.
You should be able to achieve flow.

There may be pressure to deliver the story as fast as possible.
Remember that you, or a person much like you, will have to maintain this code in future.
Be nice to those future people and try to write good code.

Good code is hard to define.
You are often pulled between three conflicting desires:

 - Maintainable code - easy to understand and modify
 - Efficient code - fast or low resource usage but hard to modify
 - Quickly written code - tends to be low performance and hard to maintain

Try to tend towards maintainable code.
Maintainable code can be made faster, or expanded later.
Performance issues are rarely prevented by blind optimisations.
When performance is an issue you profile to find the slow code and then optimise it, the speed of most of the code is largely irrelevant.

"Programs must be written for people to read, and only incidentally for machines to execute"

#### Test

Implementation and testing usually go hand in hand.
Testing usually treats code as a black box, and the different styles of testing vary the size of the box.

A unit test will test an individual class or single piece of functionality.
Other classes that are used are replaced with mimics, or mocks, which can be controlled.

It will test that when given certain inputs, you observe certain effects on the system.
Some of this testing will be about returned data.
Some of it will be to test that the class correctly invokes methods on other classes.

An integration test expands this to multiple classes working together.
The aim of an integration test is to complement the unit tests by testing the connections between classes.

An end to end test expands this to the entire application.
At this point you can describe the test in terms of the behaviour of the user, so these tests establish that the observable behaviour is correct.
The only thing that needs to be mocked is external systems which your application interacts with.
You don't want the failure of a remote system to affect your tests.

#### Evaluate

The evaluation stage is where the implementation can end, when it is judged to be good enough.
The ideal evaluation is a demo of the application covering the new functionality.
Since you probably haven't written everything when its time to evaluate, you can fill out any missing parts with an implementation which just mimics enough of the missing behaviour.

It is quite normal for the idea to change as soon as the running system is observed.
New ideas can be triggered, or adjustments to existing thoughts.
This is another reason not to design everything at the beginning - the idea is being explored by implementing it.
