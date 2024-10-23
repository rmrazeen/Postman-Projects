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

Here’s the requested GitHub Markdown file with test cases:

```md
# API Test Cases

## Test 003: Verify "Book List" API

**Description**: Validate the response of the "Book List" API and ensure non-fiction books are available.

### Test Steps:
1. Send a `GET` request to `/books?type=non-fiction`.
2. Filter the response for books where `available` is true.

### Assertions:
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

## Test 004: Verify Single Book Retrieval

**Description**: Verify that a single book's details can be retrieved and that the book is in stock.

### Test Steps:
1. Send a `GET` request to `/books/:bookId`.

### Assertions:
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

## Test 005: Verify Book Order Creation

**Description**: Validate the creation of a book order and ensure the order ID is generated.

### Test Steps:
1. Send a `POST` request to `/orders` with a valid book ID and customer name.

### Assertions:
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

## Test 006: Verify Retrieval of All Orders

**Description**: Validate the retrieval of all orders placed by the user.

### Test Steps:
1. Send a `GET` request to `/orders`.

### Assertions:
- **Status Code Check**: 
   ```javascript
   pm.test("Status code is 200", function () {
       pm.response.to.have.status(200);
   });
   ```

---

## Test 007: Verify Retrieval of a Single Order by ID

**Description**: Verify that a specific order can be retrieved using its ID and matches the stored global variable.

### Test Steps:
1. Send a `GET` request to `/orders/:orderId`.

### Assertions:
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

## Test 008: Verify Order Name Update

**Description**: Verify that the order's customer name can be updated successfully.

### Test Steps:
1. Send a `PATCH` request to `/orders/:orderId` to update the customer name.

### Assertions:
- **Status Code Check**: 
   ```javascript
   pm.test("Status code is 204", function () {
       pm.response.to.have.status(204);
   });
   ```

---

## Test 009: Verify Updated Customer Name

**Description**: Validate that the updated customer name contains the expected first name "Razeen."

### Test Steps:
1. Send a `GET` request to `/orders/:orderId` to retrieve the updated order details.

### Assertions:
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

## Test 010: Verify Order Deletion by ID

**Description**: Ensure the order can be deleted successfully.

### Test Steps:
1. Send a `DELETE` request to `/orders/:orderId`.

### Assertions:
- **Status Code Check**: 
   ```javascript
   pm.test("Status code is 204", function () {
       pm.response.to.have.status(204);
   });
   ```

---
```


## Postconditions
- The collection variable `accessToken` is set for use in subsequent requests.

---
