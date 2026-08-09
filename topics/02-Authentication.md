# Authentication

Handling authentication effectively is a core skill for any backend developer. Postman supports numerous auth types out of the box.

## Common Authentication Types

1. **Basic Auth:** The simplest form of authentication. Postman takes a username and password, encodes them in Base64, and sends them in the `Authorization` header as `Basic dXNlcjpwYXNz`. It's not secure over plain HTTP (use HTTPS).
2. **API Key:** Often sent as a header (e.g., `x-api-key`) or as a query parameter. Postman allows you to set this at the Collection or Folder level so you don't have to add it to every request.
3. **Bearer Token:** The most common form for modern web apps (often a JWT). It's sent in the `Authorization` header as `Bearer <token>`.
4. **OAuth 2.0:** Postman has a built-in OAuth 2.0 flow generator. You can configure the Auth URL, Access Token URL, Client ID, and Client Secret. Postman will handle the browser redirect flow to fetch the token and attach it to your requests.

## Inheriting Auth

The best practice in Postman is to set your Authorization at the **Collection** or **Folder** level and set individual requests to "Inherit auth from parent". This keeps your requests DRY (Don't Repeat Yourself).

## Automating Token Refresh (Intermediate/Advanced)

Tokens expire. Instead of manually fetching a new token, you can automate this using a Pre-request Script at the Collection level.

**Workflow:**
1. Check if the token (stored as an environment variable) is expired or close to expiring.
2. If expired, use `pm.sendRequest()` to make an HTTP call to your `/login` or `/refresh` endpoint.
3. Extract the new token from the response and update the environment variable.
4. The actual request will then run using the fresh token.

**Code Example (Collection-level Pre-request Script):**
```javascript
// Get the stored token expiry time
const tokenExpiry = pm.environment.get("tokenExpiry");
const currentTime = Date.now();

// If no token or token is expired, fetch a new one
if (!tokenExpiry || currentTime > tokenExpiry) {
    const loginRequest = {
        url: pm.environment.get("baseUrl") + "/auth/login",
        method: "POST",
        header: {
            "Content-Type": "application/json"
        },
        body: {
            mode: "raw",
            raw: JSON.stringify({
                email: pm.environment.get("userEmail"),
                password: pm.environment.get("userPassword")
            })
        }
    };

    pm.sendRequest(loginRequest, function (err, response) {
        if (err) {
            console.log("Auth request failed:", err);
            return;
        }
        const jsonResponse = response.json();
        pm.environment.set("authToken", jsonResponse.token);
        // Set expiry to 55 minutes from now (assuming 1-hour token)
        pm.environment.set("tokenExpiry", currentTime + (55 * 60 * 1000));
        console.log("Token refreshed successfully.");
    });
}
```

*Interview Tip:* Discussing automated token refresh using `pm.sendRequest()` demonstrates strong intermediate knowledge of Postman scripting.
