---
layout: post
title: "Mastering Small Scope Programs: A Comprehensive Guide for Bug Hunting"
date: 2023-08-06
author: Akash Hamal
username: akashhamal0x01
tags: [bug bounty, methodology, api testing, bug bounty tips]
---

Hi there, I hope you’re all doing well. This is my fourth writeup, and today I will discuss how to approach small scope programs. By small scope programs, I mean programs that have no wildcards such as:

- test.com and api.test.com in scope, etc

We will start off by creating an account on the platform and logging in. With Burp Intercept On, your work is to check every functionality and click every link you see. You can perform CRUD operations on available functionalities. While doing that, Burp Suite will populate the SiteMap — meaning we’ll identify many endpoints with different parameters to test.

Before we go further, some of you might already be uninterested in testing the website or might find it boring. However, I suggest dedicating at least eight hours before moving on to another program. In these eight hours, you will learn the application’s purpose, its functionalities, and how they are supposed to work.

To start off, you can first learn about the purpose of the web application itself. For that, check:

- If they have published any blogs
- If they have a YouTube channel (can be a treasure trove)
- If they have documentation (a gold mine)

Once you start getting familiar with your target, you’ll notice you are enjoying it and results will follow if you stay persistent. Let’s assume you know most of the functionalities and are ready to take off.

## One At a Time:

Test one functionality at a time. Try to perform CRUD (Create, Read, Update, DELETE) operations on each endpoint. For example, if `/v1/user/12345` is an endpoint:

- Create: `POST /v1/user`
- Read: `GET /v1/user/12345`
- Update: `PUT /v1/user/12345` (sometimes `PATCH`)
- Delete: `DELETE /v1/user/12345`

### Example:

```
POST /v1/user HTTP/1.1
Host: test.com
Content-Type: application/json
Authorization: Bearer your_access_token

{
  "username": "newUser123",
  "password": "passw0rd",
  "email": "newuser123@test.com",
  "firstName": "New",
  "lastName": "User"
}
```

Make sure to include required headers like `Content-Type: application/json`. Many endpoints return errors if headers or values are missing.

...

## Final Words

Everything is free on the internet. It’s an ocean — swim, don’t sink. 😂

Stay persistent, don’t get distracted. Even small daily progress counts. Take care of your health too.

Finally you should:

![This is the way](https://cdn-images-1.medium.com/max/2434/0*Q5OPzDQDQIeCSvjV)

Rather than just scrolling Twitter daily and waiting for mentorship — remember: if you cannot help yourself, no one can. Those who are successful now were persistent, eager to learn, and had a vision. **This is the way.**

**How are you supposed to find vulnerabilities if you give up in a few minutes or hours, or worse, do nothing at all?**

It requires **persistence**, **patience**, and **determination**. Don’t give up easily — it may take months or years. Remember, **duplicate reports** are part of the process.

**Every failed attempt is a step toward success. Every bug you don’t find gets you closer to the ones you will. Treat every ‘failure’ as a learning opportunity.**

Thanks for reading!
