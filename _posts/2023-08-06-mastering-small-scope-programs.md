---
layout: post
title: "Mastering Small Scope Programs: A Comprehensive Guide for Bug Hunting"
date: 2023-08-06
---

Hi there, I hope you’re all doing well. This is my fourth writeup, and today I will discuss how to approach small scope programs.  
By small scope programs, I mean programs that have no wildcards such as:

- `test.com` and `api.test.com` in scope, etc

We will start off by creating an account on the platform and logging in.  
With Burp Intercept On, your work is to check every functionality and click every link you see.  
You can perform CRUD operations which are available on such functionalities.  
While you are doing that, Burp Suite will populate the SiteMap — meaning we will be identifying many endpoints with different parameters which we can later test on.

Before we go further, some of you might already be uninterested in testing the website or might find it boring.  
However, I would suggest dedicating at least eight hours before moving on to another program.  
In these eight hours, you will learn the application’s purpose, its various functionalities, and how they are supposed to work.

To start off, you can first learn about the purpose of the web application itself. For that, you need to find out:

- If they have published any blogs  
- If they have a YouTube channel (which can sometimes be a treasure trove)  
- If they have documentation (which can be a gold mine)

---

This is where the fun begins — once you start getting familiar with your target you will notice that you are enjoying it actually, and there will be some positive results soon if you stay persistent.  
Let’s assume you know the purpose of most functionalities and the application and now you are ready to take off.

## One At a Time:

Test functionality one at a time. Try to perform CRUD (Create, Read, Update, DELETE) operations on every endpoint.  
Assuming `/v1/user/12345` is the endpoint, we can perform each operation like this:

- Create: `POST /v1/user HTTP/1.1`
- Read: `GET /v1/user/12345 HTTP/1.1`
- Update: `PUT /v1/user/12345 HTTP/1.1`, sometimes `PATCH` is used to update also
- Delete: `DELETE /v1/user/12345 HTTP/1.1`

**Note:** The `POST`, `PUT`, `PATCH` methods require HTTP parameters with correct value in request body so the backend can process your requests.  
Let’s say to create a user we can issue the following HTTP request on test.com:

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

It will create a user with given `username`, `password`, `email`, `firstName` and `lastName` values.

Also if there is any error after you issue HTTP request, the response might contain additional detail of what went wrong:

- `missing "location" parameter` — usually means you need to add a location parameter  
- `"password"` must contain alphanumeric characters — means the value in the password parameter needs to be alphanumeric

But these are just the basics, and you will learn eventually if you keep learning every day.

---

Same thing with DELETE requests. For example:

```
DELETE /v1/user/1337 HTTP/1.1
Host: test.com
Authorization: Bearer your_access_token
```

If it goes through and user with `userId` 1337 is deleted, then you are lucky to have found a serious vulnerability.

Many times, you have no idea if the endpoint supports other HTTP request methods.  
Since you don’t test other methods, you end up losing a lot of bugs.

Suppose you only see `PUT /v1/user/123445` to update users — but it might also support `PATCH /v1/user/123445`.  
Which means maybe you can add extra parameters or find IDOR if `PUT` is not vulnerable since the logic can differ.

So you should test every endpoint with different HTTP request methods so you don’t miss any vulnerabilities.

**Note:** `POST`, `PUT`, `PATCH` methods can require a valid Content-Type header.  
In this context: `Content-Type: application/json`.  
If that header is missing, your request may return `400 Bad Request`.

---

## Hunting for CSRF:

Usually most hunters simply check for CSRF once and move on.  
But every endpoint needs to be tested with every possible CSRF bypass.  
Some endpoints might not validate CSRF tokens — automate your testing.

There are many CSRF bypass techniques available on Twitter, HackerOne reports, etc. Google is your friend.

---

## Hunting for XSS:

Context of reflection matters.

Most hunters blindly spray payloads without understanding reflection points.  
You may test the "name" field and see no reflection, but it might show up somewhere else in the app where sanitization is missing.

That’s where XSS could happen. So trace all reflection points thoroughly.

---

## Mass Assignments:

It occurs when the application blindly copies user input into model properties, letting an attacker overwrite data.

Let’s look at the earlier HTTP request:

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

Now modify it by adding:

```json
"userId": 1337
```

If this causes the user to be created/overwritten using ID 1337, that’s a serious vulnerability.

Tools:

- Param Miner (Burp BApp Store)
- [Arjun by @s0md3v](https://github.com/s0md3v/Arjun)

---

## Excessive Data Exposure:

REST APIs are known for leaking more data than needed.

Say you're reading another user’s profile:

```
GET /v1/user/1337 HTTP/1.1
Host: test.com
Authorization: Bearer your_access_token
```

Example response:

```json
{
  "id": 1337,
  "username": "JohnDoe",
  "email": "john@example.com",
  "passportNumber": "123456789",
  "postalAddress": "NYC, USA",
  "location": "New York",
  "age": 30
}
```

This discloses PII which should not be exposed.

---

