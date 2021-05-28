---
layout: post
title: "Hack-the-Box Pro Labs: Offshore Review"
date: 2021-05-28
---

Depositing my 2 cents into the Offshore Account.

Hack-the-Box Pro Labs: Offshore Review
=======================

Introduction
------------

This review has been long over due, as I finished the lab about a month and a half ago;
but between work, life and these crazy times it actually took me longer than expected to get to writing this.
At the end of 2020, I have finished CRTP course and spent a couple of months without doing any courses or labs but rather going over my notes and solidifying what I have learned in the course.
However, came 2021 and I realized I have not done any infrastructure assessment for a while (Life threw more and more web application tests at me). 
So, I got a bit of an itch for another infrastructure environment to pwn and to further employ the skills/knowledge that I have obtained during CRTP. 
I then headed to HTB and looked over the pro-labs that they had to offer. 
I ended up putting my finger on Offshore as I have read about and heard of it being a pretty real-life "corporate" environment. Hack The Box also rates Offshore as intermediate lab.
By having prior OSCP and CRTP Experience, doing some vulnhub/HTB boxes here and there and being a fulltime pentester, I believed I had enough to knowledge for this lab not to be too challenging (and it was not).
Pricing for HTB labs was justifiable; at the time of signing up it was 80GBP for setup fees I believe and 20GBP a month for subscription.
In March 2021, I have signed up for the lab time and began my journey, which I believe made Pro Labs my favorite content that HTB puts out.

Lab Environment
---------------

**Overall**

The lab environment in my opinion is very well set up, from DMZ all the way to the last subnet/domain. Also, I found on US side of the labs it's much less busy than on EU side. Less people access US lab so that environment is much more enjoyable. I only ran into remnants of other players twice, I think. 
As HTB mentions "Offshore Pro Lab has been designed to appeal to a wide variety of users, everyone from junior-level penetration testers to seasoned cybersecurity professionals as well as infosec hobbyists and even blue teamers; there is something for everyone."
I think that description does truly caption the essense of the lab. The lab started with getting access to a pivoting machine, which opens the door to the first domain. From my OSCP times, I was personally very used to proxychains in order to reach "inside" targets.
However, I wanted to explore other options and ended up learning to work with sshuttle and chisel, which in my opion are great tools and alternatives to proxychains when it comes to pivoting.

The lab seemed quite linear for the most part, as in you break into one system and find something there for the next, although there were a few "odd challenges" which did not relate to the linear path at all.
As usual enumeration is key, but experience obtained from previous assessments/labs/certificates is also key; along with some patience and research of course.
Safe to say a lot of what I've learned during CRTP came in very handy during Offshore, most of the AD related misconfiguration and weaknesses were pretty familiar and made it a breeze to go through.
Some examples of these: user privileges, trust-boundaries, sensitive domain groups.
When it came to Linux boxes, I think they were pretty OSCP style, although some of them had really, really cool challenges.
I also really enjoyed where creators hid some of the flags. Unlike OSCP boxes or free HTB boxes I have encountered, looking for Offshore flag was quite a goose chase.
Another thing I enjoyed is, looking for alternative tools and recompiling existent tools in order to evade AV protection. This again put a few new tools into my arsenal.

One goal I wanted to accomplish, but failed to squeeze it's full potential was learning and using Covenant framework. The shells were rather finicky and did not last or when they lasted some of the commands did not execute.
I ended up only using Covenant on 1/4 of the windows machines (as I did not want to pay for more time than I had to) then I got fed up and just resorted back to MSF/nc. 

**The Good**
I am not into CTF challenges, but ippsec did an AMAZING job at his challenge. Although I did lose my patience with it couple of times at the end I totally loved.

**The Bad**
As much of an amazing experience that Offshore was, there was a box where you either had to write a script to automate the process or you would be stuck in a robot loop attempting to exploit it. The exploit chance for that box was about 1/50, as i discussed it with numerous users.
However, I am not the biggest fans of CTFs as I mentioned above and I don't think I would have time to exploit this type of box on a regular assessment, therefore I really did not like it. However, to each their own right?

Overall, it took me just over 2 months to finish the box and I think that's what the timing would be for most people with full time jobs and other life responsibilities.

Summary
-------

Overall, I think Hack The Box (specifically mrb3n) has done an impressive job with this lab, immitating a real-life Active Directory exploitation. 
I am definitely looking forward to taking other labs from the platform and hopefully more of them from mrb3n. 
