---
layout: post
title: "Mastering Small Scope Programs: A Comprehensive Guide for Bug Hunting"
date: 2023-08-06
---

Hi there, I hope you’re all doing well. This is my fourth writeup, and today I will discuss how to approach small scope programs. By small scope programs, I mean programs that have no wildcards such as:

- `test.com` and `api.test.com` in scope, etc

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
- Update: `PUT /v1/user/12345` (sometimes PATCH)
- Delete: `DELETE /v1/user/12345`

### Example:

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

This will create a user with the given username, password, email, firstName, and lastName values.

If there's an error, the response might help:

- Missing `location` parameter? Add it.
- `password` must contain alphanumeric characters? Adjust the value accordingly.

### DELETE Example:

DELETE /v1/user/1337 HTTP/1.1  
Host: test.com  
Authorization: Bearer your_access_token

If the user is deleted, that might indicate a serious vulnerability.

Also test for alternate HTTP methods like PATCH if PUT is available, and vice versa — logic might differ.

**Note:** `POST`, `PUT`, `PATCH` often require a valid `Content-Type` header like `application/json`. If it's missing, the site might reject your request with a `400` status.

## Hunting for CSRF

Don't just try once and move on. Evaluate all endpoints. CSRF bypasses exist and vary by context. Look at past HackerOne reports, blog posts, and tools for ideas.

## Hunting for XSS

Context matters! Payloads not reflecting in one place might appear elsewhere. Understand the DOM, injection point, and reflection path. Blind spraying won't help. Look for reflection in other locations in the app.

## Mass Assignments

Occurs when user-controlled input is blindly copied into model properties.

POST /v1/user HTTP/1.1  
Host: test.com  
Content-Type: application/json  
Authorization: Bearer your_access_token  

{
    "username": "newUser123",
    "password": "passw0rd",
    "email": "newuser123@test.com",
    "firstName": "New",
    "lastName": "User",
    "userId": 1337
}

If `userId` is accepted and overwrites or creates with a specific ID, it can lead to ATO or data corruption.

### Tools:

- Burp BApp: Param Miner
- CLI: Arjun by @s0md3v (https://github.com/s0md3v/Arjun)

## Excessive Data Exposure

REST APIs may reveal too much:

GET /v1/user/1337 HTTP/1.1  
...

{
  "id": 1337,
  "username": "JohnDoe",
  "email": "john.doe@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "age": 30,
  "height": "180 cm",
  "location": "New York, USA",
  "postalAddress": "10021-3100 New York, Park Ave",
  "passportNumber": "123456789",
  "created_at": "2021-08-05T14:45:31.000Z"
}

Sensitive fields (PII) like passport number, address, etc. should not be exposed.

## Finding Hidden Endpoints/Params

Look in:

- JS files
- Page source

### Tools:

- LinkFinder by @GerbenJavado: https://github.com/GerbenJavado/LinkFinder
- JS Link Finder (Burp BApp)

## Reading Documentation

Docs = gold mine.

- If docs say "Only owner can invite", but admin can too → vuln.
- Always reference docs in reports.

## Staying Updated

- Fork Postman collections
- Follow them on X/Twitter
- Subscribe to newsletters, changelogs, HackerOne updates

## Final Words

- Stay persistent, don’t expect instant wins
- Every attempt teaches something
- Recon isn't covered here — focus was on in-app testing
- Learn from others, follow blogs, read reports
- Don’t give up — this is a long game

Thank you for reading!