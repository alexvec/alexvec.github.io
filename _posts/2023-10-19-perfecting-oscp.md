---
title:  "Perfecting the OSCP with 110/100 score: Here's my journey"
date: 2023-10-19 21:42:00 +0200
img_path: /assets/img/perfecting-oscp/
categories: [certifications]
tags: [oscp,exam]
image:
  path: oscp.png
---

## Beginning with the Basics

When I first thought of obtaining the Offensive Security Certified Professional (OSCP) certification, I knew I had to first dominate the basic skills it requires. This includes a deep understanding of networking , operating systems (especially Linux), and scripting (bash and Python, at minimum). The OSCP does cover a lot of material on the basics, but I think having a strong foundational knowledge will really help to be less overwhelmed.

Another question I used to have a lot before signing up for the OSCP was, how many machines should I solve first? In my case, I think I overprepared, and solved ~50 Hack The Box machines from [TJ Null's list](https://docs.google.com/spreadsheets/u/1/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview#) before even registering for the OSCP. In hindsight, I would say solving Easy/Medium machines is more than enough to feel ready for it.

## PWK Courseware and Labs

The OSCP comes with the Penetration Testing with Kali (PWK) courseware, which I found very useful. It's very important to not rush through it, try to understand every topic and optionally, do the exercises if you're going for the 10 points bonus score. Despite some sections not being 100% applicable for the exam, there are many things you will learn new, even after having solved many machines before like I did. 

The OSCP Labs are good, but to be honest, except some must-do labs, many are not worth doing. I would put special attention on those labs which are Active Directory (AD) related. Rather than the PWK Labs, I would strongly recommend doing machines from Proving Grounds (PG). The machines here are much more similar to what you're expecting to find the real exam, they mostly don't have a CTF-like feeling, and feel like real vulnerable targets.

## Taking Notes

Taking notes is a must to be ready for the OSCP. There are plenty of templates in the Internet where you can find some ways to structure them, I personally structured my notes in two main sections: Theory and Machines solved. It can be a time consuming process, but it's well worth writing things down to better remember them, and at a later time, see how you solved a specific problem as it can be present in the exam.

## Practice, Practice, Practice

Practice makes perfect. In my opinion, the more machines you try to solve yourself (use write-ups only if you feel really stuck), the better prepared you will be. Personally, I solved around 110-120 total machines before taking my exam. Some people might be better off solving less, or some more. Before the exam I was worried if I was prepared enough, but after taking the exam I can confidently say that I was very well prepared with the number of machines I solved. 

Don't forget to **solve many AD machines** and networks too, perfect pivoting with your favourite tools (chisel, proxychains, etc.) as they are a key aspect of the exam.

## Exam Day

I scheduled my exam to start at 09:00 and, as you might know it lasts 24 hours. I slept early the night before, and tried not to stress too much. Once the exam started, I went for a standalone Linux machine. I found the low hanging-fruit fast, and pwned the machine as root in the first 40 minutes. With this confidence boost, I went for another standalone machine, spent half an hour and felt stuck. I moved to another standalone machine and this one took me ~1.5 hours to own as root. Knowing when to change target is important to keep a fresh mind in my opinion. At this point I had 40+10 (bonus)/100 score. 

Then, I decided to go for the AD and it took me ~1 hour to own all 3 machines. I had practiced many AD networks before, and I was attacking this one with the right mindset in my opinion. It was 12:20 and after solving the AD network, I had already passed the exam, with a score of 90/100 points. But I wanted to do every single machine.

I came back to the last machine feeling more relaxed, as I knew I had already passed the exam. I made sure to take screenshots of every attack vector and how I was exploiting them. Soon after, I found the way to get a shell with the low privileged user. Great, now I just needed to escalate privileges and call it a day. This wasn't going to be so easy. I found some possible vectors to which escalate privileges, but none worked. I read my notes, and I couldn't find nothing new. I spent like this many, many hours, I slept, woke up early next morning, and started observing things from a different perspective. 

It was ~8:00 and I thought maybe, I wasn't running the exploits right, so I decided to use metasploit (which is allowed to be used for one machine). I ran an attack vector I really thought should have worked when exploiting manually, but failed to work. So I set the options for the exploit, ran it, and boom! That worked. I had escalated privileges as root on the last machine. I had aced the exam with a perfect 100/100 score with a bouns 10 points, which make it a **total of 110/110**. I was ecstatic and felt I deserved it, after preparing seriously for 3 months!

![OSCP Certification](cert.png)