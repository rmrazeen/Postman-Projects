# Book List API

This project demonstrates the use of the **Book List API** to manage book-related operations, such as retrieving a list of books, placing orders, and updating customer information. The API collection is designed to interact with an online bookstore where users can retrieve book details, register API clients, and order books.

## Features

- **Welcome Message**: A basic request to check if the API is accessible.
- **Register API Client**: Register a new API client using a unique name and email.
- **Book List**: Retrieve a list of available books filtered by type (e.g., non-fiction).
- **Single Book**: Fetch details of a single book, including checking its stock status.
- **Order Book**: Place an order for a book by specifying the book ID and customer name.
- **Get All Orders**: Retrieve all orders made by the client.
- **Get Order By ID**: Fetch details of a specific order using its ID.
- **Update Order Name**: Update the customer name in an order.
- **Delete Order**: Remove an order by its ID.

## Getting Started

### Prerequisites

- **Postman** or any other API testing tool.
- **API URL**: `https://simple-books-api.glitch.me`
- A valid `accessToken` for authenticated requests.

### Importing the Collection

1. Download or clone this repository.
2. Open **Postman**.
3. Go to **File > Import**.
4. Select the `Book List.postman_collection.json` file.
5. Configure the necessary environment variables, such as `BaseUrl` and `accessToken`.

### Environment Variables

Make sure to set the following environment variables for smooth operation:

- `BaseUrl`: The base URL for the API (`https://simple-books-api.glitch.me`).
- `accessToken`: A valid API token to authenticate requests.
- `BookId`: The ID of a book for retrieving or ordering.
- `OrderID`: The ID of an order used for updates or deletion.

## API Endpoints

### 1. Welcome Message

**GET** `/status`

- Description: Retrieves the API's current status.
- Example Test:
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });
  ```

### 2. Register API Client

**POST** `/api-clients`

- Registers a new API client.
- Example Request Body:
  ```json
  {
    "clientName": "John Doe",
    "clientEmail": "johndoe@example.com"
  }
  ```

### 3. Get Book List

**GET** `/books?type=non-fiction`

- Retrieves a list of non-fiction books.
- Example Test:
  ```javascript
  pm.test("Book found", () => {
      pm.expect(response).to.be.an("object");
  });
  ```

### 4. Get Single Book

**GET** `/books/:bookId`

- Retrieves details of a specific book.
- Example Test:
  ```javascript
  pm.test("Is in stock", () => {
      pm.expect(response["current-stock"]).to.be.above(0);
  });
  ```

### 5. Order Book

**POST** `/orders`

- Places an order for a specific book.
- Example Request Body:
  ```json
  {
    "bookId": 3,
    "customerName": "John Doe"
  }
  ```

### 6. Get All Orders

**GET** `/orders`

- Retrieves all orders placed by the client.

### 7. Get Order by ID

**GET** `/orders/:orderId`

- Retrieves a specific order by its ID.
  
### 8. Update Order Name

**PATCH** `/orders/:orderId`

- Updates the customer name for a specific order.
- Example Request Body:
  ```json
  {
    "customerName": "Jane Doe"
  }
  ```

### 9. Delete Order

**DELETE** `/orders/:orderId`

- Deletes a specific order by its ID.
  

For any issues or suggestions, feel free to reach out.
```
