# Variables and Environments

At an intermediate level, you need to understand how to manage state and dynamic data across multiple API requests.

## Variable Scopes

Postman resolves variables based on scope, from narrowest to broadest. If a variable with the same name exists in two scopes, the one with the narrower scope takes precedence.

1. **Local:** Temporary variables created and used during a single request execution (using scripts).
2. **Data:** Variables derived from CSV or JSON files when running a collection via the Collection Runner.
3. **Environment:** Variables tied to a specific environment (e.g., `dev`, `staging`, `prod`). Crucial for switching base URLs or credentials without changing the request itself.
4. **Collection:** Variables available to any request within a specific collection. Good for collection-specific configuration.
5. **Global:** Variables available across your entire Postman workspace. Use sparingly to avoid naming collisions.

## Dynamic Variables

Postman uses the `faker` library to generate dummy data. You can use these in your request bodies, headers, or URLs by using the `{{$variableName}}` syntax.
* `{{$guid}}`: A random v4 style GUID.
* `{{$timestamp}}`: The current Unix timestamp in seconds.
* `{{$randomEmail}}`: A random email address.
* `{{$randomInt}}`: A random integer between 1 and 1000.

*Interview Tip:* Mentioning dynamic variables shows you know how to avoid hardcoded mock data.

## Request Chaining

This is a common interview topic. How do you take the ID from a `POST` response and use it in a `GET` request?
You do this using the **Tests** tab (post-response script) of the first request to set a variable.

**Example: Request 1 (POST /users)**
```javascript
// Parse the response
const response = pm.response.json();

// Set the user ID as an environment variable
pm.environment.set("userId", response.data.id);
```

**Example: Request 2 (GET /users/{{userId}})**
The URL now uses `{{userId}}`, which was populated by the previous request.
