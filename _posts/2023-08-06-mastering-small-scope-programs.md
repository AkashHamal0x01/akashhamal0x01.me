---
layout: post
title: "Mastering Small Scope Programs: A Comprehensive Guide for Bug Hunting"
date: 2023-08-06
author: Akash Hamal
username: akashhamal0x01
tags: [bug bounty, methodology, api testing,bug bounty tips]
---

Hi there, I hope you’re all doing well. This is my fourth writeup, and today I will discuss how to approach small scope programs. By small scope programs, I mean programs that have no wildcards such as:

* test.com and api.test.com in scope , etc

We will start off by creating an account on platform and logging in. With Burp Intercept On, now your work is to check every functionality and click every link you see. You can perform CRUD operations which are available on such functionalities , while you are doing that Burp Suite will populate the SiteMap which means we will be identifying many endpoints with different parameters which we can later test on.

Before we go further, some of you might already be uninterested in testing the website or might find it boring. However, I would suggest dedicating at least eight hours before moving on to another program. In these eight hours, you will learn the application’s purpose, its various functionalities, and how they are supposed to work.

To start off, you can first learn about the purpose of the web application itself. For that, you need to find out :

* If they have published any blogs
* If they have a Youtube channel (which can sometimes be a treasure trove)
* If they have documentation (which can be a gold mine)

This is where the fun begins, once you start getting familiar with your target you will notice that you are enjoying it actually and there will be some positive results soon if you stay persistent. Lets assume you know the purpose of most of functionalities and the application and now you are ready to take off.

...

**Final Words**

Everything is free on internet, its an ocean where you can swim unless you don’t know ofc😂 but eventually you will learn xD

Stay Persistent, don’t get distracted. Even a bit of progress every day counts.

Take care of health too

Finally you should :

![This is the way](https://cdn-images-1.medium.com/max/2434/0*Q5OPzDQDQIeCSvjV)

Rather than just scrolling twitter daily and waiting for someone to mentor. Remember If you cannot help yourself, no one can. Those who are successful now were persistent; they were eager to learn and had a vision. This is the way.

***How you are supposed to find vulnerabilities if you just give up in some minutes or some hrs, or the worst is doing nothing and sitting just thinking lol.***

It requires **persistence**, **patience** and **determination. Don’t give up easily**, it might take months, years. Remember **Duplicate reports** are part of the process.

**Every failed attempt is a step towards success. Every bug that you don’t find gets you closer to the bugs you will eventually find. Treat every ‘failure’ as a learning opportunity.**

And Of course I didn’t cover SQLI, SSRF, etc other categories as I believe most of you are familiar with these categories. You should try everything on each parameters and verify how they are handling inputs and check for different vulnerabilities.

I believe I covered some basic topics in this writeup. I did not cover much recon as we dived into the application and I hope they are helpful to the community. I might add more based on the feedback I receive from the readers.

Thank you for reading!

