---
title:  "$4200+ in one month doing Bug Bounty - November Recap"
date: 2023-11-30 15:20:00 +0200
img_path: /assets/img/november-recap/
categories: [bugbounty]
tags: [bugbounty,vulnerabilities]
image:
  path: recap.png
---

This month has been the most profitable ever for me in bug bounty. $4200+ in rewards so far, here's how I've done it.

## HackerCup 2023

I embarked this month on my first ever bug bounty event, hosted by [Bugcrowd](https://www.bugcrowd.com/blog/hacker-cup-2023-hack-hack-revolution/). I felt this was a big motivation boost to start hacking on new targets and to try to keep improving. I created a team with 4 other people which have helped greatly to find new bugs and discuss them. Until now I have mostly been hacking by myself, so doing collaborations is something new for me, and which I felt really helped to find new vulnerabilities, and it did.

## Reports

At first, I started hacking on random programs, switching frequently between them. I was doing automation, trying to find vulnerabilities, and all vulnerabilities I found with automation, turned out to be duplicates. The first week of hacking seriously, we found several XSS, information disclosure and one cache poisoning vulnerabilities. In the end they all turned out to be duplicates. 

This was incredibly frustrating, as I felt all the time I spent at the beginning had been wasted. In total I had 18 duplicate reports!

Then, I decided to change my bug bounty hunting strategy, and I decided to focus deeper on one or two programs. This is where things started to change. On the first days of focusing on the programs I chose, one of my teammates found a valid Reflected XSS on a target. It motivated me to keep going, and I started better understanding how the company operated internally, and how inner applications worked together. Fuzzing on a subdomain showed me an endpoint which I hadn't seen before and which hid a secret parameter. I observed how it interacted with the application and found it was possible to create a POST based reflected XSS thanks to it.

Soon after reporting it, I received my first valid vulnerability, a Medium/P3 with a $500 payout. I kept going every day looking deeper into every asset in scope, and was constantly finding more vulnerabilities. In total we had 15 reports accepted for XSS vulnerabilities. Every vulnerability had a payout of $400-$500 which was great! 

### High/P2 IDOR report

Then I kept fuzzing every target, making sure not to miss anything and found an exposed Swagger UI endpoint, which looked something like: `https://<subdomain>/<redacted>/static/index.html`

![Swagger UI](swagger.png)

When I stumbled on this exposed Swagger UI endpoint I knew it was very likely to find juicy stuff on it. It listed a bunch of API endpoints, showing how every request should look like, and it turned out some endpoints didn't enforce authorization at all! On one endpoint I was able to list a bunch of business services, and on another endpoint I was able to update them, providing arbitrary information, and even changing access credentials! Immediately, I wrote my report for it and was rewarded $1350 for a P2.

![Accepted Reports](reports.png)

In total, I have made $4200+ in bounties on November, and I still have one report waiting to be triaged. I think my two key takeaways for my performance this month are:

- Collaboration. Discussing your thought process on a specific bug with others, and sharing ideas has really helped find more vulnerabilities and even with a % cut on the bounty, it's still 100% worth it.
- Focusing on one or two programs. I'm a lover for automation stuff, but when it comes to consistently finding valid bugs, nothing beats manual search. Automation will higlight low hanging fruits, and those are very likely to be duplicate. Manually focusing on a program and sticking to it for several days or weeks will reveal vulnerabilities which automation would have never picked up.