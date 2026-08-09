# Postman Best Practices

To really stand out in an interview, you should understand not just *how* to use a tool, but how to use it *well* in a professional setting. Here are some key best practices for Postman.

## 1. Keep It DRY (Don't Repeat Yourself)
*   **Auth:** Don't set Authentication on every single request. Set it at the Folder or Collection level and have child requests inherit it.
*   **Base URLs:** Never hardcode base URLs (like `http://localhost:8080` or `https://api.prod.com`). Always use an `{{apiUrl}}` environment variable so you can easily switch between local development and production.

## 2. Organized Collections
*   **Folders:** Group requests logically using folders. A common pattern is grouping by resource (e.g., a "Users" folder containing GET, POST, PUT, DELETE for `/users`).
*   **Naming Conventions:** Name your requests clearly. Instead of "Request 1", use "Get User by ID" or "Create New Post". This acts as self-documenting code.
*   **Descriptions:** Use the Description field for Collections, Folders, and Requests. This markdown field is perfect for adding notes about what an endpoint expects.

## 3. Robust Scripting
*   **Avoid Magic Numbers:** If you are testing for specific IDs or values, try to use dynamic variables or extract them from previous requests rather than hardcoding them in your tests.
*   **Clear Error Messages:** When writing `pm.test()`, give your tests clear, descriptive names. If a test fails in a CI/CD pipeline, the name is all you'll see.
    *   *Bad:* `pm.test("Status 200", ...)`
    *   *Good:* `pm.test("User creation returns 201 Created", ...)`

## 4. Security
*   **Never Hardcode Secrets:** Do not hardcode API keys or passwords in the request URL or body. Use Variables.
*   **Initial vs. Current Values:** When using variables for secrets (like an OAuth token), put the value in the **Current Value** column, NOT the **Initial Value**. Initial values are synced to Postman servers and shared with your team if you export the collection. Current values stay strictly on your local machine.

## 5. Version Control Your Collections
*   **Export as JSON:** You can export a collection as a JSON file and commit it to your Git repository. This gives you version history and diffs for your API tests just like regular code.
*   **Postman API:** Alternatively, use the Postman API to programmatically fetch collections and environments, which can be integrated into your build scripts.
*   **Why it matters:** If a teammate changes a shared collection and breaks tests, you can review the diff or revert to a known-good version.

## 6. Use the Collection Runner Effectively
*   **Run Order:** The Collection Runner executes requests in the order they appear top-to-bottom. Use this to create end-to-end workflows (e.g., Create User → Get User → Update User → Delete User).
*   **Data Files:** Feed CSV or JSON data files into the Runner to execute the same requests with different inputs (data-driven testing). This is especially useful for testing edge cases and validation rules.
*   **Delays:** Add a delay between requests (e.g., 500ms) if you're hitting rate-limited APIs during testing.
