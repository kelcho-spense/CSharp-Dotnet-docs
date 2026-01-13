# Security – ASP.NET Core Web API

Once you create a Web API Service, the most important thing you need to take care of is security, which means you need to control access to your Web API Services. So, let’s start the discussion with the definition of Authentication and Authorization. **Authentication** and **authorization** are critical to building secure web applications, including ASP.NET Web API applications. Authentication is the process of verifying the identity of a user. At the same time, Authorization determines what actions and resources the Authenticated user is allowed to access based on their identity and roles. **Authentication** is performed first, and then Authorization

## Authentication in Web API:
Authentication is the process of identifying the user. For example, one user, let’s say, James, logs in with his username and password, and the server uses his username and password to authenticate James. Common authentication mechanisms for Web APIs include:

* **Bearer Token Authentication**: Clients include a token (e.g., JWT or OAuth token) in the request headers, which the server validates.
* **Basic Authentication**: Clients send a username and password with each request, typically encoded in Base64 format.
* **API Key Authentication**: Clients include an API key in the request, validated on the server.
* **OAuth 2.0**: OAuth 2.0 is a framework for securing APIs. It allows clients to obtain access tokens used to authenticate requests.
* **Custom Authentication**: You can implement custom authentication logic based on your requirements.
* **Windows Authentication**: If you are running your application in a Windows environment, you can use Windows Authentication to authenticate users based on their Windows credentials.

## Authorization in Web API:
Authorization is deciding whether the authenticated user can perform an action on a specific resource (Web API Resource). For example, James (an authenticated user) has permission to get a resource but does not have permission to create a resource. So, once a user is authenticated, you can implement authorization by defining Roles and Policies or using attributes in your controllers and actions.

* **Role-Based Authorization**: You can use role-based authorization to restrict access to specific resources or actions based on the user’s role. Users are assigned roles (e.g., “**Admin**,” “**User**”) that determine their access rights. You can decorate controllers or actions with the `[Authorize(Roles = “RoleName”)]` attribute.
* **Policy-Based Authorization**: You can define and apply custom policies to controllers or actions. Policies can be based on a combination of roles, claims, or custom requirements. Policies are defined with specific access requirements, and endpoints are protected using the `[Authorize(Policy = “PolicyName”)]` attribute.
* **Claim-Based Authorization**: Resource access is based on the user’s claims in claims-based authorization. You can use the `[Authorize]` attribute with specific claims as parameters.
* **Custom Authorization**: For more complex scenarios, you can implement custom authorization logic by extending the authorization framework or implementing custom middleware.