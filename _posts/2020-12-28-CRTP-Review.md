---
title: "CRTP Exam/Course Review"
date: 2020-12-28 00:00:00 -0500
categories: [Education, Guides]
tags: [crtp, certification, active-directory, red-team, course-review]
---

A little bit about my experience with Attacking & Defending Active Directory course and Certified Red Team Professional (CRTP) exam.

CRTP Exam/Course Review
=======================

Introduction
------------

A bit over a year I have passed my OSCP and started my career in penetration
testing, saying that I will be mostly comparing CRTP to OSCP. As most (who have
taken OSCP before the 2020 update) know, there was not a whole lot of material
and machines that covered Active Directory (AD) environment and attacks. Knowing
that AD is the most used environment in organizations, I started looking for
another course which would help me to acquire and develop my knowledge in that
area and fill in the gap.

Across my search I then ran into pentesteracademy.com which offered Attacking
and Defending Active Directory course, which upon completion offered exam which
grants one with Certified Red Team Professional certificate. The price of the
course was reasonable (\$249 USD at the time) for 30 days lab time and a few
reviews I read also had good things to say about the course. I then signed up
for 30 days, which also came with 1 free exam attempt, and began the course.

Course Material
---------------

The course material focuses purely on AD vulnerabilities via misconfigurations
rather than CVEs like OSCP. The course is also based on learning new material
rather than being challenged like PKW. The course is beginner friendly as the
author, Nikhil Mittal, takes a very detailed approach explaining every step of
the way throughout the course. Author’s approach ensures that you learn and
understand the concepts that are being taught.

The learning material comes in video format (26 videos at the time of doing the
course), along with a lab manual in PDF. The sum of the video time is about 14
hours and took me about 5 days to complete. A wide range of topics is covered
such as Domain Enumeration, Local Privilege Escalation, Domain Privilege
Escalation, Lateral Movement, Domain Persistence, Cross Forest Attacks, and
Detection and Defence, more about which can be found
[here](https://www.pentesteracademy.com/activedirectorylab).

One thing that I found just a little irritating, is that lab manual does get an
update every now and then and new tools and techniques are included in it.
However, videos do not get updated and some of them can definitely use an edit.
Nevertheless, I do understand that can be tedious to re-record the video every
single time new content comes out, especially in environment such as AD.

I believe that 30-days time period is sufficient enough if you already have some
background in penetration testing and your weekday evenings are somewhat free. I
went over the videos twice and did the whole lab twice as well.

Lab Environment
---------------

I have to say, I did find the lab environment very well done and for the most
part pretty stable. Although when something did mess up, me being in North
America, the customer service was a bit delayed once the day went into
afternoon.

The Lab environment consist of several forests interconnected together, with
each forest having a couple of different domains. One starts as a low privilege
user on a box containing all the tools one needs for exploitation. As the videos
take you through the exploitation process step by step, it is pretty hard to get
off course and not successfully complete the achievement goals.

I would recommend watching videos and doing the labs at the same time to grasp
the concepts and solidify your knowledge better.

Exam
----

Exam follows the OSCP time model and a student is given 24 hours to get code
execution on 5 different machines. Unlike OSCP, the machines are sequential and
interconnected. The exam is pretty much based on course material with just a
little twist. Some of the concepts may need to be applied a little differently
than they were in the course. The CRTP exam focuses more on exploitation and
code execution rather than on persistence. Well, I guess let me tell you about
my attempts.

Once my lab time was almost done, I felt confident enough to take the exam. To
myself I gave an 8-hour window to finish the exam and go about my day. Little
did I know then. My first box I owned within the first hour which made me feel
pretty good about myself. My second box took a bit longer, I realized that for
some reason the tools were not giving me the results I wanted. I wasted some
time opening new Powershell windows and re-running the exact same tool/command
before achieving the desired result. (\*\* Note to self, always rerun tools
multiple times in case you do not get the desired results. Not all environments
are the same, not all tools are consistent). An hour and a half after the first
box, I owned the second box and this is where the “nightmare” began. Sometime at
the beginning of the exam I ran bloodhound, to see potential attacks paths. I
knew exact attack path from machine\#3 to the rest of the environment, however I
was knee deep stuck on the way to machine \#3. I spent about 6 hours trying to
get a foot hold to that machine and was almost convinced something was wrong in
the environment and it was not me doing something wrong. I ended up giving up as
I had to wake up early the next day.

When you fail an exam attempt Pentester Academy gives you a 30-day cool down
period before you may be able to take another attempt. During these 30 days, I
re-watched all the videos and reviewed all my notes and still did not get where
I went wrong. I decided that on the second attempt I will not start exploiting
right away but rather will thoroughly enumerate the environment. As I started my
second attempt, I gave myself the same goal of 8 hours. This time I paid much
more attention to the enumeration and found the tiny detail I missed from my
first attempt. 7.5 hours into the second attempt, all 5 machines had code
execution achieved on them.

Summary
-------

Attacking and Defending Active Directory course and CRTP exam provide good
ground knowledge of AD penetration testing along with being very affordable. I
think it is also a much more beginner friendly than OSCP. Depending on how
quickly one grasps new concept, I believe 30-day lab time is more than enough to
complete the course and attempt to do the exam. I really enjoyed the course and
the quality of the material (for the most part). I would recommend this course
to anyone wanting to learn about AD and perhaps even getting it before their
OSCP.
