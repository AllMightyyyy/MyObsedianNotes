---
tags:
  - JWT
  - DefinitionJWT
  - "#Auth"
---
## Table of Contents

- [Introduction to Spring Security](Introduction%20to%20Spring%20Security)
  - [1.Setup Spring Boot Project](1.Setup%20Spring%20Boot%20Project)
  - - [2.Create User and Role Entities](2.Create%20User%20and%20Role%20Entities)
  - - [3.Repositories](3.Repositories)
  - - [4.Create Authentication Request and Response DTOs](4.Create%20Authentication%20Request%20and%20Response DTOs)
  - - [5.JWT Utility Class](5.JWT%20Utility%20Class)
  - - [6.JWT Authentication Filter](6.JWT%20Authentication%20Filter)
  - - [7.Configure Spring Security](7.Configure%20Spring%20Security)
  - - - [8.Authentication Controller](8.Authentication%20Controller)
#### What is JWT ? 
* JWT ( JSON Web Token ), a secure way of sharing information between two parties ( typically client and server ) as a digitally signed token. JWTs are commonly used to authenticate users in web applications without requiring server-side session
#### How JWT Authentication Works :
* **User Logs In** : A user provides credentials ( username and password )
* **Server Verifies and Issues Token** : If credentials are valid, the server creates a JWT and sends it back to the client
* Client Stores Token : The client stores the token either in "localStorage" or "sessionStorage" on the frontEnd
* Client sends token for future requests : For every subsequent request, the client includes JWT token in the request headers
* Server validates JWT : the server check if the JWT is valid, before granting access to protected resources
#### Why use JWT ?
* **Stateless** : JWT's are self contained, meaning they don't require a server to keep track of sessions
* Scalability : Great for distributed systems or microservices where managing sessions on a central server would be complex
* Security : JWTs are signed, ensuring data integrity