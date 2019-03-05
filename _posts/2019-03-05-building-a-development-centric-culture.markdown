---
layout: post
title: "Building a Development First Culture"
date: 2019-01-07 18:15:05 -0700
categories: culture
cover: "/assets/development.jpg"
---

Too often, companies are reacting to problems instead of proactively preventing them.

This leads to production and QA thinking their job is to play a game of Where's Waldo with product bugs, finding them or things which look like them in the wild. Developers are left playing a game of whack-a-mole with those same bugs, some of which disappear before you have time to address them.

Once a company finds itself in this situation it can be hard to break the cycle.  Developers are encouraged to support and prioritizing bugs, inevitably rejecting the underlying issues.  Bugs are symptoms of problems or inefficiencies of a system bubbling their way to the surface.  Who doesn't love a good bug? It provides a easy to understand reward cycle that can trick humans into thinking they are doing something.  Identify bug -> fix bug -> receive boost from 'accomplishing' something.  As we know, the human reward system can pursue short term goals at the expense of clear long term benefits.  This is why the cycle becomes so hard to break, because everyone _feels_ like they are accomplishing something.  It requires top down commitment and organizational discipline.

If you find youself in a position where your product progress has halted because you are spending all development resources fixing problems instead of planning to avoid them, here are some simple steps to help shift that momentum.

## Clearly define practices around patching bugs ##

What is a bug?

If a bug is defined as anything which impacts customer experience, a company will invest an immense amount of effort into chasing those bugs.  If a bug is defined as behavior not adhering to a product spec, that puts the owness back on the planning and architecture process instead of a developer.

How do you define the severity and/or impact of a bug?  If a bug impacts a small fraction of users, but requires significant developer time, should you patch the bug?  Is there a better long term solution that you could include in the next formal release?  Is there a short term work-around to avoid distracting development team from long term priorities?

Without metrics around occurances and impacts of bugs every issue becomes a fire drill.  In my experience, patches are often regressions or hasty fixes to problems that don't scale.  If you have the choice of moving forward or backwards out of a sticky situation, always go forward.  This might mean asking customers to stomach a mild inconvenience for a month, but in the long term the strategy is rewarded with the velocity you gain from your development cycle.

What is a worth while bug fix? How much risk does it introduce?  Are you comfortable with that risk?  Are there ways to mitigate that risk, or test around those risk scenarios?

Even if a bug is impacting users it might not be a good idea to fix it. If the fix introduces risk, might have significant side affects, or hasn't been sufficiently tested, then there is a real chance it will create as many bugs as it fixes.  Having strong QA processes to lean on makes this assessment much easier but it is not an exact science.  Trust the people closest to the problem and incentivize long term progress.

Keep in mind, when it comes to culture it is a matter of momentum.  It won't change overnight no matter how hard you try. Often times it can take months before changes start to be felt. Changing shared workflows, collective opinions, and unspoken norms can even initially have a detrimental affect on culture. Back to human psychology, we are programmed to resist change. Life is founded on stability and being comfortable in a situation is based on our ability to intuit situations without any cognitive load as they take fold.  It is important to remember that this change can feel just like stress to employees.

In summary, if developers are consistently patching problems instead of architecting solutions after root cause analysis, the development process can lose momentum fast.

## Clearly define your product ##

If your developers are defining a product, something has gone wrong.  Some developers can handle the full scope of work but the vast majority cannot.  If you cannot clearly articulate every aspect of a product or a feature to a developer, you are not ready for development.  That sounds harsh but the cost in wasted development hours and rework is too much to avoid if you want to maintain momentum with development.

Even if you have found a confident developer who says they can do it all, _you shouldn't_ want a developer defining your product.  You want people closest to the problem or to the customer defining your product.  You want those people interacting with designers to plan out user flows to optimize for your goals before a developer even gets involved.  Yes, getting developers exposure to customers should be an ongoing goal, but you can't rely on them for this perspective.  It also seperates concerns, so now you can maintain a positive project based relationship with a developer and iterate with a designer on loose prototypes.

## Identify and Quantify Waste ##

Wasting time and money at a start-up is an inevitably.  As a company is growing so does it problems.  They expand, morph, shrink, and can even take over a company if you let them.  Successful companies build mechanisms and a culture around quantifying, prioritizing, and addressing that waste.

As an example, lets say you are wasting significant developer time supporting deployments of a specific product. You can put processes in to track that expense or even go a step further to allocate that expense to portions of the business.  How will you know if you are making money as a company if this expense is not included in your final cost of goods sold?  From there, you can identify top causes and address them once they have a large enough impact.  The details around these situations can determine whether it makes sense to tweak the business or change it entirely.  Without information to inform those decisions companies are left blindfolded, swinging blindly, hoping to break open the pinata that is success.
