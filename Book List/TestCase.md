Here is the complete set of test cases in a consistent GitHub Markdown format, with the tester name **Razeen**:

```md
# API Test Cases

## Test Case: Verify Status Endpoint

**Test ID**: TC_StatusEndpoint_001  
**Tester Name**: Razeen  
**Test Name**: Verify the response of the status endpoint and ensure the status field is "OK".  
**Description**: This test ensures that the `/status` endpoint responds with a 200 status code and that the response body contains a `"status"` field with the value `"OK"`.

---

### Preconditions
- Base URL is set to `{{BaseUrl}}` (e.g., `https://simple-books-api.glitch.me`).

---

### Test Steps

1. **Request**:
   - Send a `GET` request to `/status`.
   - Use the following request URL: `{{BaseUrl}}/status`

2. **Expected Response**:
   - HTTP status code: `200 OK`
   - The response body should contain a `"status"` field with the value `"OK"`.

3. **Assertions**:
   - **Status Code Check**: Validate that the response status code is 200.
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```
   - **Log Status Field**: Parse the response as JSON and log the `"status"` field to the console.
     ```javascript
     const response = pm.response.json();
     console.log(response.status);
     ```
   - **Validate Status Field**: Ensure that the `"status"` field in the response is `"OK"`.
     ```javascript
     pm.test("Test status should be OK", function () {
         pm.expect(response.status).to.eql("OK");
     });
     ```

---

### Postconditions
- Optional: The next request can be set to `Book List` (commented out in the script).
  ```javascript
  // pm.execution.setNextRequest("Book List");
  ```

---

## Test Case: Verify Client Registration and Access Token Generation

**Test ID**: TC_RegisterClient_002  
**Tester Name**: Razeen  
**Test Name**: Verify API client registration and ensure access token is generated.  
**Description**: This test verifies the successful registration of an API client and checks whether an `accessToken` is generated.

---

### Preconditions
- Base URL is set to `{{BaseUrl}}` (e.g., `https://simple-books-api.glitch.me`).
- API is available and reachable.

---

### Test Steps

1. **Request**:
   - Send a `POST` request to `/api-clients`.
   - The request body should contain a randomly generated `clientName` and `clientEmail`.
     ```json
     {
       "clientName": "{{$randomUserName}}",
       "clientEmail": "{{$randomUserName}}@example.com"
     }
     ```

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

## Test Case: Verify "Book List" API

**Test ID**: TC_BookList_003  
**Tester Name**: Razeen  
**Test Name**: Verify "Book List" API.  
**Description**: Validate the response of the "Book List" API and ensure non-fiction books are available.

---

### Test Steps

1. **Request**:  
   - Send a `GET` request to `/books?type=non-fiction`.

2. **Expected Response**:  
   - HTTP status code: `200 OK`.
   - Filter books that are available (assuming it’s a boolean).

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```
   - **Available Non-Fiction Books Check**:
     ```javascript
     const response = pm.response.json();
     const nonFictionBooks = response.filter(book => book.available === true);
     console.log(nonFictionBooks[0]); // Log the first available book
     if (nonFictionBooks.length > 0) {
         pm.globals.set("BookId", nonFictionBooks[0].id);
     }
     ```

---

## Test Case: Verify Single Book Retrieval

**Test ID**: TC_SingleBook_004  
**Tester Name**: Razeen  
**Test Name**: Verify Single Book Retrieval.  
**Description**: Verify that a single book’s details can be retrieved and that the book is in stock.

---

### Test Steps

1. **Request**:  
   - Send a `GET` request to `/books/:bookId`.

2. **Expected Response**:  
   - HTTP status code: `200 OK`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```
   - **In Stock Check**:
     ```javascript
     const response = pm.response.json();
     pm.test("Is in stock", function () {
         pm.expect(response["current-stock"]).to.be.above(0);
     });
     ```

---

## Test Case: Verify Book Order Creation

**Test ID**: TC_OrderBook_005  
**Tester Name**: Razeen  
**Test Name**: Verify Book Order Creation.  
**Description**: Validate the creation of a book order and ensure the order ID is generated.

---

### Test Steps

1. **Request**:  
   - Send a `POST` request to `/orders` with a valid book ID and customer name.

2. **Expected Response**:  
   - HTTP status code: `201 Created`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 201", function () {
         pm.response.to.have.status(201);
     });
     ```
   - **Order Creation Check**:
     ```javascript
     const response = pm.response.json();
     pm.test("Order Created", function () {
         pm.expect(response.created).to.be.true;
     });
     pm.test("Order ID created", function () {
         pm.expect(response.orderId).to.be.a('string');
     });
     pm.globals.set("OrderID", response.orderId);
     ```

---

## Test Case: Verify Retrieval of All Orders

**Test ID**: TC_GetAllOrders_006  
**Tester Name**: Razeen  
**Test Name**: Verify Retrieval of All Orders.  
**Description**: Validate the retrieval of all orders placed by the user.

---

### Test Steps

1. **Request**:  
   - Send a `GET` request to `/orders`.

2. **Expected Response**:  
   - HTTP status code: `200 OK`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```

---

## Test Case: Verify Retrieval of a Single Order by ID

**Test ID**: TC_GetByOrderID_007  
**Tester Name**: Razeen  
**Test Name**: Verify Retrieval of a Single Order by ID.  
**Description**: Verify that a specific order can be retrieved using its ID and matches the stored global variable.

---

### Test Steps

1. **Request**:  
   - Send a `GET` request to `/orders/:orderId`.

2. **Expected Response**:  
   - HTTP status code: `200 OK`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```
   - **Order ID Match Check**:
     ```javascript
     const response = pm.response.json();
     pm.test("Order ID matches global variable", function () {
         pm.expect(response.id).to.eql(pm.globals.get("OrderID"));
     });
     pm.collectionVariables.set("customerName", response.customerName);
     ```

---

## Test Case: Verify Order Name Update

**Test ID**: TC_OrderNameUpdate_008  
**Tester Name**: Razeen  
**Test Name**: Verify Order Name Update.  
**Description**: Verify that the order’s customer name can be updated successfully.

---

### Test Steps

1. **Request**:  
   - Send a `PATCH` request to `/orders/:orderId` to update the customer name.

2. **Expected Response**:  
   - HTTP status code: `204 No Content`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 204", function () {
         pm.response.to.have.status(204);
         ```

---
    
## Test Case: Verify Updated Customer Name

**Test ID**: TC_UpdateNameCheck_009  
**Tester Name**: Razeen  
**Test Name**: Verify Updated Customer Name.  
**Description**: Validate that the updated customer name contains the expected first name "Razeen."

---

### Test Steps

1. **Request**:  
   - Send a `GET` request to `/orders/:orderId` to retrieve the updated order details.

2. **Expected Response**:  
   - HTTP status code: `200 OK`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 200", function () {
         pm.response.to.have.status(200);
     });
     ```
   - **Customer Name Check**:
     ```javascript
     const response = pm.response.json();
     pm.test("Customer first name is Razeen", function () {
         pm.expect(response.customerName).to.include("Razeen");
     });
     ```

---

## Test Case: Verify Order Deletion by ID

**Test ID**: TC_DeleteByOrderID_010  
**Tester Name**: Razeen  
**Test Name**: Verify Order Deletion by ID.  
**Description**: Ensure the order can be deleted successfully.

---

### Test Steps

1. **Request**:  
   - Send a `DELETE` request to `/orders/:orderId`.

2. **Expected Response**:  
   - HTTP status code: `204 No Content`.

3. **Assertions**:
   - **Status Code Check**:
     ```javascript
     pm.test("Status code is 204", function () {
         pm.response.to.have.status(204);
     });
     ```

---


