# Authentication Study

Please take the time to read this [article](https://medium.com/ag-grid/a-plain-english-introduction-to-json-web-tokens-jwt-what-it-is-and-what-it-isnt-8076ca679843) before proceeding.

## Instructions

- Fork and Clone

Write your answers in the space provided in this readme.

## A Note on Passwords

We **never** store passwords in our database. Instead, we use a hashing function to create a password hash or digest. We store the password digest in our database.

![](password_digest.jpeg)

## JSON Web Token (JWT) Authentication

Here is a flow for using JWT for Authentication

![](jwt.jpeg)

1. The user signs up:

- The client creates a POST request to the `/signup` endpoint on the server with username, email, and password in the request body

2. The server creates a JSON Web Token (JWT) based on a header, payload, and secret
3. The server responds with the JWT
4. The client saves the JWT in localStorage to persist subsequent server requests

##

Answer the following questions:

1. Why do we need authentication in our Web Apps?

```
Authentication allows us to keep our network secure by only allowing users with authetication to gain access and allows us to restrict persmissions to users based on account types.
```

2. What is the point of a [JSON Web Token](https://jwt.io/introduction)? Why would we want to use it?

```
The point of a JWT is to create a safe way for passing information in container format between two parties instead of just having users passwords out in the open for any hacker to exploit. It uses cryptography and a secret word to encrypt information that is passed to ensure authenticity and safety.
```

3. Why would we hash a user's password when they sign up? What's the point?

```
Hashing a password is a process thats used to validate the authenticity of various types of input using cryptography. The point of it is to not have peoples passwords out in the open and to have a safe means of transporting information between two parties without the data being openly avaliable to anyone with access. Its used to encrpt the password so theyre now stored as the actual password.
```

4. Go [here](https://jwt.io). Create a JWT with the following as the payload (feel free to change the username/email):

```js
{
  "id": "1",
  "username": "neo",
  "email": "neoTheSilent@ga.com"
}
```

Paste your encoded JWT below:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjEiLCJ1c2VybmFtZSI6Im5lbyIsImVtYWlsIjoibmVvVGhlU2lsZW50QGdhLmNvbSIsInN1YiI6IjEyMzQ1Njc4OTAiLCJuYW1lIjoiTWFyayBIYXJtb24iLCJpYXQiOjE1MTYyMzkwMjJ9.jn9wTHqkutkg6HVLNV_48WPKgzpW9dqtFAsd1v2bT1g
```

**Bonus**: Read https://blog.angular-university.io/angular-jwt

## Submission

Submit a pull request utilizing the [PR Template](https://github.com/SEI-R-2-22/template_pull_request)