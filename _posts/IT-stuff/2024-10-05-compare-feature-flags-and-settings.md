---
title: Compare Feature Flags and Settings
categories:
  - it-stuff
tags:
  - software engineering
---

Almost all SaaS systems need to implement Feature Flags and Settings(or Configurations) somehow. When people talk about them, they can be mixed together. There can be some vague understanding and inefficient communication, leading to wrong decisions to be made. In this article, let’s have a simple research.

At first glance, they look very similar to each other. On second thought, you may think FF is just one type of Settings. Yes, that should make sense to lots of people. However, when it comes to a particular software project, is it always a good idea to introduce a new FF under any existing Settings? Maybe not. In fact, more often than not, a scaled SaaS system usually has a separate FF system apart from some other Settings. Why is that? That’s all based on what you require the FF to do in the system. Before you put a new FF in an existing Settings system, you want to check if the scope and pattern of the Settings system can go with the FF. If the FF is just a global boolean to turn on/off a feature regardless of any other circumstances, it won't be a good idea to introduce it into a Settings system for users. On the other side of the coin, if you require the FF to be applied according to different users, you should put it in the users Settings. The scenarios can be various in different SaaS systems. Another common example is environment variables. If you introduce a feature flag in EV, you are introducing a new setting for environments.

In summary, FF is a kind of Settings. But it doesn’t mean FF can be thrown into any Settings system. Before introducing it, make sure the purpose, scope, and target of the FF are clear to people. If you cannot find an existing Settings system in your project fitting the FF, it’s probably time to introduce a Setting system to accommodate your FF.
