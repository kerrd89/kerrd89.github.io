---
layout: post
title: "Tips for Reviewing Pull Requests"
date: 2022-09-15 18:15:05 -0700
categories: technical
cover: "/assets/pull_request_vs_merge_request.jpeg"
---

Reviewing pull requests is a vital part of the software development process. Granted, perfect pull requests do not need reviews. But we are imperfect creatures operating in a changing world.

I prepared this list for myself when I became a senior developer. A senior developer's job isn't just to produce code at volume; they should review pull requests with the extra scrutiny their experience affords them.

### Read the description.

It can be tempting to jump right into the files. I often look at the diff and go for it if it is reasonable, but I am improving. I have posted questions whose answers were prominently featured in the description many times. Someone put forth the effort to organize their thoughts and explain their changes in a way they think will help you. Not reading it is outright rude.

### If there is a ticket, read the ticket.

Tickets are not to be taken at face value, but key considerations and decisions are ideally documented on tickets. Understanding the ask helps you start your review from a zoomed-out perspective.

### Look at the file tree before looking at the individual line changes.

Knowing what you know from the description and the ticket, what files do you expect to see changed? Do you see test changes corresponding to file changes? Some things are better observed at the file-level view.

### Be respectful when posting comments.

Remember that a person sat down and made these changes and had to make hard decisions. So, be gentle when discussing those decisions.

### Getting into individual changes, be clear if feedback is a request for changes, discussion, or a nitpick.

#### Request for changes.
    Point to support, documentation, or an example elsewhere in the code. Provide resources to solve the  problem you are presenting; if you cannot, offer yourself up to help. Ideally, this means pair coding, but I have put PRs into other peoples' branches; it depends on the situation.

#### Discussions are just me seeking more information.
    Maybe my understanding from the ticket was out of date. Perhaps I have a genuine concern I am trying to flesh out. If you can lead someone to an answer by asking questions, this is often better than outright asking for changes.

#### Nitpicks are a thing that I would like to see changed, but I wouldn't block merging for my personal preferences if the author feels differently.

    I preface nitpicks with "nitpick, …" to be clear that I know it is frivolous.

### If the only comments you have are nitpicks, approve the thing.

Most of the time, if you find yourself on a team with ego-free engineers who want to write good code, those nitpicks will be addressed or explained before merging. If they aren't, it is probably because the engineer was motivated to merge a PR. If there is a pattern of not wanting to learn or discuss with/from peers, that is potentially a problem. But solving that problem in this PR review isn't my job. Our job is to solve problems, which means delivering code (with supporting tests) and moving on to different problems. Be aware when you are slowing things down.

### Move your nitpicks to the formatter or linter when possible.

In this way, it isn't me enforcing or hassling people for preferences. The larger the organization, the more democratic the decisions around these matters need to be. But in my experience, engineers love a strict formatter. Minimize the bullshit people talk about in PRs, and what is left is approving, seeking more understanding, and maybe a sprinkling of bullshit.

### Unless changes are colliding, do not worry about conflicts.

Ideally, you are structuring your code in such a way (Domain Driven Design) to minimize those conflicts. Merge conflicts can be risky; everyone should write and commit code in a way that minimizes conflicts.

#### Only change the relevant files when solving a problem.
    Making refactors that affect the whole code base might seem beneficial, but if changes create a lot of friction, they do not benefit the organization.
#### Stay within the scope of that problem. Problems can balloon if we let them.
#### Update tests or add new tests around the scope of the problem.

### Questions to ask yourself as you go through a review.
* Is the code documented and organized in ways that fit norms and conventions?
* Does where the code is located make sense? Put on your Domain Driven Design hat; do not worsen any problems, but do not hold up work for past sins.
* Are there large blocks of code that are opportunities to refactor?
* Are test cases covering all code paths? You can check this programmatically and move this to a CI step, like the linter and formatter.

I am happy to add to this over time based on feedback. [Connect with me on LinkedIn](https://www.linkedin.com/in/d-kerr/) and let me know what I missed.
