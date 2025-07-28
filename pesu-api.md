> [!WARNING]  
> Still under planning, do not make a merge request to `main`.


# PESU API

## Introduction

PESU API aims to expose all the functions available through the python library `pesuacademy-py`, which contains functions to access a variety of information from the PESU Academy website, by means of webscraping, in the form of a REST API. This results in the library functions becoming language agnostic, thus greatly increasing its usage scope.

## Problem with REST APIs

The only limiting factor in implementing this API, is that the `pesuacademy-py` library uses a stateful object to call its functions, which is against the stateless principle of REST. The only viable and reasonable solution for this, is to use JWTs to contain the session. Here’s a simple workflow of what to do:

- The `/auth` endpoint is called, with username and password
- An object of the `PESUAcademy` class is created with the credentials.
- If the creation throws an error due to wrong credentials, a 401 status code is returned.
- If the object is created successfully, a credentials object is created in the following form:

```json
{
  "username": "SRN",
  "password": "password",
  "expiry": "(1 day)"
}
```

- The object is then **encrypted**, signed using the JWT secret, and sent as a session cookie.
- Future requests decrypt the token, create the object again, call the required function, and return the result.
- If the expiry time is reached on any requests, the cookie is deleted, and the user is supposed to sign in again.

## Possible endpoints

- `POST /login`
  - Login to PESU Academy using the given credentials
  - If credentials are wrong, send a 401 status code
  - If authentication is successful, a 200 status code is sent along with the encrypted session token as a cookie
- `GET /profile`
  - If signed in, returns the profile information for the signed
  - Else, returns a 401 status code
- `GET /courses`
  - If signed in, returns the courses for all semesters
  - If a particular semester is specified, returns the courses for that semester
  - Else, returns a 401 status code
- `GET /attendance`
  - If signed in, returns the attendance for all semesters
  - If a particular semester is specified, returns the attendance for that semester
  - Else, returns a 401 status code
