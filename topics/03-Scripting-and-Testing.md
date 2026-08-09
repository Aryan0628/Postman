# Scripting and Testing

Postman executes JavaScript code in two distinct phases: before a request is sent (**Pre-request Scripts**) and after a response is received (**Tests**).

## The `pm` API

The `pm` (Postman) object provides access to request and response data, variables, and testing functions.

*   `pm.environment.get("var_name")`: Get an environment variable.
*   `pm.variables.set("var_name", "value")`: Set a local variable.
*   `pm.request.url`: Access or modify the URL before sending.
*   `pm.response.json()`: Parse the response body as JSON.

## Pre-request Scripts

Use cases:
*   Generating timestamps or hashes required by specific APIs (e.g., AWS Signature).
*   Executing pre-requisite API calls using `pm.sendRequest()`.
*   Sanitizing or formatting data before it's sent in the request body.

## Tests and Assertions

Postman uses the Chai Assertion Library syntax (`pm.expect`).

**Common Assertions:**

1. **Status Code Check:**
```javascript
pm.test("Successful response", function () {
    pm.response.to.have.status(200);
});
```

2. **Response Body Structure Check:**
```javascript
pm.test("Response body contains user data", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an('object');
    pm.expect(jsonData).to.have.property('id');
    pm.expect(jsonData).to.have.property('email');
});
```

3. **Response Time Check:**
```javascript
pm.test("Response time is acceptable", function () {
    pm.expect(pm.response.responseTime).to.be.below(500); // ms
});
```

4. **Header Check:**
```javascript
pm.test("Content-Type is present", function () {
    pm.response.to.have.header("Content-Type", "application/json");
});
```
