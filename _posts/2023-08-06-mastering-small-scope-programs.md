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

---

This is where the fun begins — once you start getting familiar with your target, you’ll notice you are enjoying it and results will follow if you stay persistent. Let’s assume you know most of the functionalities and the application, and now you are ready to take off.

## 🧪 One At a Time:

Test functionality one at a time. Try to perform CRUD (Create, Read, Update, DELETE) operations on each endpoint.

Assuming `/v1/user/12345` is the endpoint:

- **Create:** `POST /v1/user HTTP/1.1`
- **Read:** `GET /v1/user/12345 HTTP/1.1`
- **Update:** `PUT /v1/user/12345 HTTP/1.1` (sometimes `PATCH`)
- **Delete:** `DELETE /v1/user/12345 HTTP/1.1`

---

### Example Request:

`POST /v1/user HTTP/1.1`  
`Host: test.com`  
`Content-Type: application/json`  
`Authorization: Bearer your_access_token`  

```json
{
  "username": "newUser123",
  "password": "passw0rd",
  "email": "newuser123@test.com",
  "firstName": "New",
  "lastName": "User"
}
```

---

If there's an error, the response might help:

- `Missing location parameter`
- `"password"` must contain alphanumeric characters, etc

---

### DELETE Request Example:

`DELETE /v1/user/1337 HTTP/1.1`  
`Host: test.com`  
`Authorization: Bearer your_access_token`

If the user is deleted, that might indicate a serious vulnerability.

Also test for alternate HTTP methods like `PATCH` if `PUT` is available, and vice versa — logic may differ.

---

### Note:

`POST`, `PUT`, and `PATCH` often require the header:  
`Content-Type: application/json`

If it’s missing, the site might reject your request with a `400`.

---

## 🎯 Hunting for CSRF

Most hunters just try once and move on. But you should evaluate **all endpoints** for CSRF bypasses.

Look at HackerOne reports, community blogs, or Twitter to find tested techniques.

---

## 💉 Hunting for XSS

Context of reflection matters. Don’t blindly spray payloads. Look for:

- Injection points
- Output encoding
- Reflection paths

Just because one page escapes input doesn’t mean others will.

---

## 🧵 Mass Assignment

Occurs when the app copies input data directly into model properties.

Try modifying:

```json
{
  "username": "newUser123",
  "password": "passw0rd",
  "userId": 1337
}
```

If you’re able to override an ID or create a user with a specific ID, it can result in ATO or account overwrite.

Tools:

- Param Miner (Burp Suite)  
- [Arjun by @s0md3v](https://github.com/s0md3v/Arjun)

---

## 🔓 Excessive Data Exposure

APIs may leak sensitive data.

Example response:

```json
{
  "id": 1337,
  "username": "JohnDoe",
  "email": "john@example.com",
  "passportNumber": "123456789",
  "location": "New York, USA"
}
```

These are considered PII and must be reviewed.

---

## 🔍 Finding Hidden Endpoints

Look in:

- JS files  
- Page source  
- API docs  

Tools:

- [LinkFinder](https://github.com/GerbenJavado/LinkFinder)
- JS Link Finder (Burp BApp)

---

## 📚 Documentation is Gold

If docs say “Only Owner can invite users” and you find Admin can — you have a bug.

Use wording in the docs as part of your proof in the report.

---

## 🛰️ Stay Updated

- Fork their Postman collections  
- Follow them on X/Twitter  
- Enable notifications or newsletter  
- Watch changelogs, HackerOne updates

---

## 💬 Final Words

- Persistence matters more than talent  
- Don’t give up after 30 mins of testing  
- Study the app deeply  
- Learn from public reports, blogs, tools

---

Everything is free online. Stay sharp. Stay ethical. Keep pushing.