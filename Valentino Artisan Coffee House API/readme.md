# Valentino Artisan Coffee House API

This API is designed to manage orders, products, and clients for Valentino's Artisan Coffee House. It allows users to interact with the system by performing operations such as checking the API status, retrieving product and order information, registering clients, and placing orders.

## Features
- **API Status Check**: Get the current status of the API.
- **Product Management**: Retrieve all available products or a single product by ID.
- **Client Registration**: Register new API clients.
- **Order Management**: Create and retrieve customer orders.

## Getting Started

### Prerequisites
- Postman (or any API client)
- A valid API Key (if required)

### Importing the Collection
1. Download or clone this repository.
2. Open Postman.
3. Click on **File > Import**.
4. Select the `Valentino Artisan Coffee House API.postman_collection.json` file from the project directory.

### Environment Setup
Before running the requests, make sure to set the environment variables:

- `baseUrl`: Base URL of the API (e.g., `https://valentinos-coffee.herokuapp.com`)
- `apiKey`: Your valid API Key (for authenticated requests)
- `OrderID`: Set after creating an order
- `customerName`: The customer's name used during order creation

You can define these variables in **Postman** under **Environment** settings.

## Endpoints

### 1. API Status
**GET** `/status`  
- Check if the API is running.

Example Test:
```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

### 2. Get All Products
**GET** `/products`  
- Retrieve a list of all products.

### 3. Get Single Product
**GET** `/products/:productId`  
- Retrieve information for a specific product by its ID.

Example Test:
```javascript
pm.test("Product ID is Correct", () => {
    let response = pm.response.json();
    pm.expect(response.id).to.eql(parseInt(pm.collectionVariables.get("productId")));
});
```

### 4. Create a New Order
**POST** `/orders`  
- Place a new order for a customer, including details of the products.

Example Request Body:
```json
{
  "customerName": "{{customerName}}",
  "products": [
    {
      "id": 2001,
      "quantity": 1
    },
    {
      "id": 2002,
      "quantity": 3
    }
  ]
}
```

### 5. Get All Orders
**GET** `/orders`  
- Retrieve a list of all customer orders.

### 6. Register a New Client
**POST** `/clients`  
- Register a new API client with an email address.

### Example Postman Tests

```javascript
pm.test("Order ID Format", () => {
    // Ensure the Order ID is formatted as expected (with or without hyphens)
    pm.expect(response.id).to.match(/^[A-Z0-9]+(-[A-Z0-9]+)*$/);
});
```

## Authentication
For some requests, an API Key is required. Pass the API Key in the headers as follows:
- **Header**: `x-api-key`
- **Value**: `{{apiKey}}`

## Running the Tests
This collection includes several test scripts that automatically verify the status codes, response formats, and other essential conditions. You can run these tests directly in Postman.

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## How to Upload and Showcase on GitHub

### 1. Create a GitHub Repository
- Go to [GitHub](https://github.com) and create a new repository.
- Name the repository (e.g., `valentino-coffee-house-api`).

### 2. Upload Files
- Clone the repository to your local machine.
- Place your `README.md` and `Valentino Artisan Coffee House API.postman_collection.json` in the root folder of the repository.
- Commit and push the changes:
```bash
git add .
git commit -m "Initial commit"
git push origin main
```
