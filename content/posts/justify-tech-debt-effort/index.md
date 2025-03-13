+++
title = "Quantifying the Business Value of Paying Off Tech Debt"
date = "2025-03-13"
author = "Safique"
description = "A hypothesis to determine the business value of working on tech debt and how it can improve development velocity and product performance."
categories = ["Software Development", "Agile", "Technical Debt"]
tags = ["Tech Debt", "Agile", "Sprint Planning", "Business Value", "Software Metrics"]
draft = false
+++


It’s a very common scenario — The product owner chooses new features regularly and seldom picks tech debt. In the end, the crashes have to be fixed, and the tech debt is growing. Those problems are slowing down testing and development._

_Besides, the team have new great ideas to optimize the software, but the product owner has difficulty to estimate their values, and priorities them._

What actions could you take to resolve the issues?

**Business will get more things delivered with the same development team and same sprint time by paying off tech debt.**

Even though product managers know this, it is really difficult for them to determine the value of working on tech debt. Instead of asking him to determine the value, I came up with a hypothesis to determine the value.

There are several software metrics like cyclomatic complexity, depth of inheritance which can be easily calculated by static code analysis with SonarQube or similar services. However, these may not make clear sense to product managers to find the business value. My hypothesis comes up with quantifiable values that help the product manager to prioritize tech debts.

Businesses will receive two major types of value by investing in paying off the tech debt.

1.  **Improvement on development and testing time.** If we analyze sprint velocity, we will see the decrease of values every sprint due to tech debt. I have devised a formula** to calculate how much extra story points could we deliver in a year. Considering we will have 1 sprint to work on tech debt in every quarter and the average performance degradation/story point decrease is 5% of the velocity of product sprint, according to the formula, we will deliver extra 3.7 sprints of work yearly.
2.  **Improvement on product performance.** Performance optimization has a direct impact on business. Most of the cases, if response time gets faster, conversion rate gets higher. Expected hike of the conversion rate is the business value.

Erasing tech debts have some qualitative values as well.

1.  Mental peace
2.  Correctness of estimation
3.  Courage to add new features

I will try to establish one of the following processes

1.  There will be 1 entire sprint in order to erase tech debts in every quarter.
2.  Allocate 20% of the sprint capacity on tech debts in every sprint.

**Formula:**
> Let, 
> Each sprint is **2 weeks** long
> 
> Each quarter has **5 product sprints** and **1 tech-debt sprint**
> 
> Story point of 1st product sprint is **v**
> 
> Average decrement per sprint is **d = _5%_** _of_ **v**
> 
> Without a tech debt sprint, in 7th sprint,
> 
> team would have been delivered **v-7d** story points
> 
> With a tech debt sprint, in 7th sprint,
> 
> team will deliver **_v_** story points.
> 
> Extra story points = **v — (v -7d) = 7d**
> 
> For 22 product sprints, team will deliver = **176d** extra story points
> 
> Sacrificing 4 product sprints will eventually benefit
> 
> = **176d — 4v**
> 
> = **176 * v * 5% — 4v**
> 
> = **7.7v — 4v**
> 
> = **3.7v** extra story points
> 

