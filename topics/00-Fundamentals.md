# Postman Fundamentals

Before diving into intermediate concepts, it's crucial to be able to confidently explain what Postman is and how it functions at a basic level.

## What is Postman?
Postman is an API (Application Programming Interface) platform that simplifies the process of building, testing, and using APIs. At its core, it acts as a very powerful HTTP client. It allows you to construct complex HTTP requests (GET, POST, PUT, DELETE, etc.), send them to a server, and inspect the response, all within an intuitive graphical user interface (GUI).

*Interview Tip:* Don't just say "it's for testing APIs." Say it's an "API development platform that facilitates the entire API lifecycle, from design and mock servers to automated testing and documentation."

## HTTP Status Codes

You **must** know these. Interviewers will test your understanding of status codes even outside of Postman questions.

| Range | Category | Common Codes |
|---|---|---|
| **1xx** | Informational | `100 Continue` — Server received the request headers, client should proceed to send the body. |
| **2xx** | Success | `200 OK` — Standard success. `201 Created` — Resource successfully created (common after POST). `204 No Content` — Success, but no body returned (common after DELETE). |
| **3xx** | Redirection | `301 Moved Permanently` — Resource moved to a new URL. `304 Not Modified` — Cached version is still valid. |
| **4xx** | Client Error | `400 Bad Request` — Malformed request (e.g., invalid JSON). `401 Unauthorized` — Authentication required or failed. `403 Forbidden` — Authenticated but not authorized. `404 Not Found` — Resource doesn't exist. `429 Too Many Requests` — Rate limited. |
| **5xx** | Server Error | `500 Internal Server Error` — Generic server failure. `502 Bad Gateway` — Server acting as proxy got a bad response. `503 Service Unavailable` — Server is temporarily down (maintenance/overload). |

*Interview Tip:* Know the difference between `401 Unauthorized` (you haven't logged in) and `403 Forbidden` (you're logged in but don't have permission). This is commonly asked.

## Query Parameters vs. Path Parameters

Understanding the difference is fundamental to working with REST APIs.

*   **Path Parameters:** Part of the URL path itself. Used to identify a specific resource.
    *   Example: `GET /users/123` — `123` is the path parameter identifying a specific user.
    *   In Postman, you'd write: `{{baseUrl}}/users/:userId` and Postman creates a field for `userId`.

*   **Query Parameters:** Appended after a `?` in the URL. Used for filtering, sorting, or pagination.
    *   Example: `GET /users?page=2&sort=name` — `page` and `sort` are query parameters.
    *   In Postman, you can type them in the URL or use the **Params** tab (key-value editor), which is cleaner.

## Collections and Workspaces

*   **Workspaces:** The top-level organizational unit in Postman. Think of it like a project folder. You can have Personal Workspaces (private), Team Workspaces (shared with your team), and Public Workspaces (open source).
*   **Collections:** Inside a workspace, a Collection is a group of saved API requests. You can organize them with folders (e.g., a "Users" folder, an "Auth" folder). Collections can also have:
    *   **Collection-level scripts** (pre-request and tests that run for every request in the collection).
    *   **Collection-level variables** and **auth** (inherited by all child requests).
    *   **Documentation** auto-generated from request descriptions.

## How Does It Work?
Under the hood, Postman works by sending raw HTTP requests over the network. 
When you click "Send" in the Postman UI, the application:
1. Translates your UI inputs (URL, headers, body) into a standard HTTP request message.
2. Opens a network connection to the target server.
3. Transmits the request.
4. Waits for the server to process the request and send back an HTTP response.
5. Parses the HTTP response and displays the status code, headers, and body in a human-readable format.

## How Do We Check Requests and Responses?

The Postman interface is divided into two main sections: the **Builder** (top) and the **Response** (bottom).

### Sending a Request (The Builder)
1.  **Method:** Choose the HTTP method (e.g., `GET` for retrieving data, `POST` for creating data) from the dropdown.
2.  **URL:** Enter the endpoint URL you want to hit (e.g., `https://api.example.com/users`).
3.  **Params:** Add query parameters (the key-value pairs that appear after the `?` in a URL).
4.  **Headers:** Add required metadata (e.g., `Content-Type: application/json` or `Authorization`).
5.  **Body:** (For POST/PUT/PATCH) Add the data you want to send. The most common format is raw JSON.

### Checking the Response
Once you hit "Send", you'll look at the bottom half of the screen to evaluate the outcome:
1.  **Status Code:** Located in the top right of the response pane. This is the most critical check (e.g., `200 OK`, `201 Created`, `400 Bad Request`, `500 Internal Server Error`).
2.  **Time & Size:** Shows how long the request took and the size of the payload.
3.  **Body:** The actual data returned by the server (usually formatted as JSON, XML, or HTML).
4.  **Headers:** The metadata returned by the server (useful for debugging CORS issues or checking caching headers).
