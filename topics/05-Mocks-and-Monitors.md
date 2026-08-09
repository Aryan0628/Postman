# Mocks and Monitors

These features are useful when working in larger teams or maintaining production systems.

## Mock Servers

A Mock Server simulates the behavior of a real API by accepting requests and returning predefined responses.

**Why use a Mock Server?**
1. **Frontend Development:** Frontend developers can start building the UI against the mock API while the backend is still being developed.
2. **Isolating Tests:** When testing microservices, you can mock external dependencies to ensure your tests only evaluate your code, not the third-party service's uptime.

**How it works in Postman:**
1. You create an "Example" response for a specific request in your collection.
2. You create a Mock Server tied to that collection. Postman provides a unique URL.
3. When you send a request to the Mock Server URL, Postman looks at the path, method, and headers, finds the matching Example, and returns it.

## Monitors

Monitors allow you to run a Postman Collection on a schedule (e.g., every 5 minutes, every hour).

**Use Cases:**
1. **Health Checks (Uptime Monitoring):** Verify that your production APIs are up and responding correctly.
2. **Performance Monitoring:** Track the `pm.response.responseTime` over time to detect degradation.
3. **Scheduled Testing:** Run end-to-end integration tests periodically against a staging environment.

*Interview Tip:* Mentioning Monitors shows you think about "Day 2" operations—what happens *after* the code is deployed to production.
