# Test Case: Verify Client Registration and Access Token Generation

**Test ID**: TC_RegisterClient_001  
**Test Name**: Verify API client registration and ensure access token is generated.  
**Description**: This test verifies the successful registration of an API client and checks whether an `accessToken` is generated.

---

## Preconditions
- Base URL is set to `{{BaseUrl}}` (e.g., `https://simple-books-api.glitch.me`).
- API is available and reachable.

---

## Test Steps

1. **Request**:
   - Send a `POST` request to the `/api-clients` endpoint.
   - The request body should contain a randomly generated `clientName` and `clientEmail`.
     ```json
     {
       "clientName": "{{$randomUserName}}",
       "clientEmail": "{{$randomUserName}}@example.com"
     }
     ```
   - Use the following request URL:  
     `{{BaseUrl}}/api-clients`

2. **Expected Response**:
   - HTTP status code: `201 Created`
   - The response should contain an `accessToken` field, and it should be a string.

3. **Assertions**:
   - **Status Code Check**: Validate that the response status code is 201.
     ```javascript
     pm.test("Status code is 201", function () {
         pm.response.to.have.status(201);
     });
     ```
   - **Access Token Generation**: Check if the response contains an `accessToken`, and ensure it is a string.
     ```javascript
     const response = pm.response.json();
     pm.test("Access Token is generated", function () {
         pm.expect(response.accessToken).to.be.a('string');
     });
     ```
   - **Set Collection Variable**: Store the `accessToken` in the collection variable for future requests.
     ```javascript
     pm.collectionVariables.set("accessToken", response.accessToken);
     ```

---

## Postconditions
- The collection variable `accessToken` is set for use in subsequent requests.

---
