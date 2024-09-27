# hot-coffee

## Learning Objectives

- REST API
- JSON
- Logging
- Basic software design principles
- Layered software architecture

## Abstract

In this project, you will build a **coffee shop management system**.

Similar projects are used by businesses to work in a competitive environment and make a profit.
This application will simulate the backend of a coffee shop's ordering system, allowing users to create, modify, close, and delete orders. Additionally, it will manage other data like menu items and inventory.

This project will teach you one of the best ways to turn business logic into code and make that code easily maintainable and scalable.

## Context

Have you ever wondered how your favorite coffee shop manages to handle a flurry of orders during the morning rush, keep track of inventory so they never run out of your preferred blend, or remember that you like your coffee with an extra shot of espresso?

Behind the scenes, coffee shops rely on sophisticated management systems that coordinate orders, inventory, menu items, and customer preferences in real-time. These systems ensure that baristas can focus on crafting the perfect cup while the technology handles the complexities of order processing, stock management, and data recording.

The `hot-coffee` (coffee shop management system) project is a simplified version of these real-world applications, designed to give you hands-on experience with the core principles behind such operational software. Imagine an application that allows staff to:

- Manage Orders: Create, modify, close, and delete customer orders efficiently.
- Oversee Inventory: Track ingredient stock levels to prevent shortages and ensure freshness.
- Update the Menu: Add new drinks or pastries, adjust prices, and keep the offerings current.
- Engage with Customers: Maintain records of customer preferences and order histories to personalize service.

While commercial systems are complex and built for large-scale operations, this project focuses on the essentials:

- REST API: You'll build a web service that allows different parts of the system—or even different systems—to communicate seamlessly over HTTP.
- JSON Data Handling: Learn how to encode and decode JSON to transmit data in a format that's both human-readable and machine-friendly.
- Layered Architecture: Implement a structured approach to software design that separates concerns into different layers, making your code more maintainable and scalable.
- Logging: Use Go's log/slog package to record significant events, aiding in debugging and monitoring the system's health.
This project is a practical exploration of how order management and inventory systems operate under the hood, including how they handle data processing, manage concurrent requests, and ensure data integrity. By building the coffee shop management system, you'll dive into key topics like RESTful API design, data serialization, and software architecture principles.

Whether you're aiming to grasp the basics of backend development or preparing to work on real-world enterprise applications, this project offers a hands-on approach to learning these essential concepts. It's an opportunity to understand not just how to write code, but how to design systems that solve real problems—skills that are invaluable in the software development industry.

---


## Resources

- Read about REST API
- Read about JSON
- Read about Logging
- Read about Layered software architecture

## General Criteria

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:

```sh
$ go build -o hot-coffee .
```
---
## Mandatory Part

### Baseline
Your task is to develop the `hot-coffee` application, a **coffee shop management system** that provides a RESTful API for managing orders, menu items, and inventory. The application should be built using Go and adhere to a layered software architecture with a focus on clean code, maintainability, and scalability.

#### Outcomes:

- Implement HTTP endpoints for creating, retrieving, updating, and deleting orders.
- Store data locally in JSON files.
- Use a three-layered architecture:
  - Presentation Layer (Handlers): Manages HTTP requests and responses.
  - Business Logic Layer (Services): Contains core functionality and business rules.
  - Data Access Layer (Repositories): Handles data storage and retrieval.
- Utilize Go's `log/slog` package for logging significant events.
- Perform aggregations based on the stored data (e.g., total sales, popular menu items).

#### Notes:

- The API should handle JSON requests and responses.
- Data must be persisted in JSON files in a thread-safe manner.
- The application should start an HTTP server listening on a configurable port.

#### Constraints:
- Handle errors gracefully and return appropriate HTTP status codes.
- All actions and errors must be logged.

### Functional Requirements

#### Layered Software Architecture
Implement the application using a **three-layered architecture**, ensuring that each layer has a clear responsibility and that they interact through well-defined interfaces.

**Presentation Layer (Handlers):**

- **Responsibilities**:
  - Handle HTTP requests and responses.
  - Parse input data and format output data.
  - Invoke appropriate methods from the Business Logic Layer.
- **Implementation Details**:
  - Organize handlers based on entities (e.g., order_handler.go, menu_handler.go, inventory_handler.go).
  - Use Go's `net/http` package to set up routes and handle requests.
  - Validate input data and return meaningful error messages.
  
**Business Logic Layer (Services):**

- **Responsibilities**:
  - Implement core business logic and rules.
  - Define interfaces for services to promote decoupling.
  - Perform data processing and call methods from the Data Access Layer.
  - Handle aggregations and computations based on business requirements.
- **Implementation Details**:
  - Define service interfaces (e.g., OrderService, MenuService, InventoryService) in separate files.
  - Provide concrete implementations of these interfaces in corresponding implementation files (e.g., order_service_impl.go).
  - Implement methods for aggregations (e.g., GetTotalSales, GetPopularMenuItems).
  - Ensure that services are independent and can be tested in isolation.

**Data Access Layer (Repositories)**:

- **Responsibilities**:
  - Manage data storage and retrieval operations.
  - Interact with JSON files to persist and read data.
  - Ensure data integrity and consistency.
  - Provide interfaces for repositories to allow flexibility.
- **Implementation Details**:
  - Create repository interfaces for each entity (e.g., OrderRepository, MenuRepository, InventoryRepository).
  - Implement these interfaces.
  - Organize data in separate JSON files for each entity, stored in the `data/` directory.

**Examples:**

Organize your project files to reflect the layered architecture and promote maintainability.

```go
hot-coffee/
├── cmd/
│   └── main.go
├── internal/
│   ├── handler/
│   │   ├── order_handler.go
│   │   ├── menu_handler.go
│   │   └── inventory_handler.go
│   ├── service/
│   │   ├── order_service.go
│   │   ├── order_service_impl.go
│   │   ├── menu_service.go
│   │   ├── menu_service_impl.go
│   │   ├── inventory_service.go
│   │   └── inventory_service_impl.go
│   └── repository/
│       ├── order_repository.go
│       ├── order_repository_impl.go
│       ├── menu_repository.go
│       ├── menu_repository_impl.go
│       ├── inventory_repository.go
│       └── inventory_repository_impl.go
├── models/
│   ├── order.go
│   ├── menu_item.go
│   └── inventory_item.go
├── go.mod
└── go.sum
```

Notes:

- `cmd/`: Entry point of the application.
- `internal/`: Application code divided into layers.
  - `handler/`: Presentation layer handling HTTP requests.
  - `service/`: Business logic layer with interfaces and implementations.
  - `repository/`: Data access layer with interfaces and implementations.
- `models/`: Data models shared across layers.
- `go.mod` and `go.sum`: Go module files.


**Data Models**

Define data models for each entity, ensuring they are serializable to JSON and include all necessary fields.

- **Order (`models/order.go`):** 

```go
type Order struct {
    ID           string       `json:"order_id"`
    CustomerName string       `json:"customer_name"`
    Items        []OrderItem  `json:"items"`
    Status       string       `json:"status"`
    CreatedAt    string       `json:"created_at"`
}

type OrderItem struct {
    ProductID string `json:"product_id"`
    Quantity  int    `json:"quantity"`
}
```

- **Menu Item (`models/menu_item.go`):**
```go
type MenuItem struct {
    ID          string  `json:"product_id"`
    Name        string  `json:"name"`
    Description string  `json:"description"`
    Price       float64 `json:"price"`
}
```

- **Inventory Item (`models/inventory_item.go`):**
```go
type InventoryItem struct {
    IngredientID string `json:"ingredient_id"`
    Name         string `json:"name"`
    Quantity     int    `json:"quantity"`
    Unit         string `json:"unit"`
}
```

### API Endpoints

Implement the following RESTful API endpoints:

- **Orders:**

  - `POST /orders`: Create a new order.
  - `GET /orders`: Retrieve all orders.
  - `GET /orders/{id}`: Retrieve a specific order by ID.
  - `PUT /orders/{id}`: Update an existing order.
  - `DELETE /orders/{id}`: Delete an order.
  - `POST /orders/{id}/close`: Close an order.

- **Menu Items:**

  - `POST /menu`: Add a new menu item.
  - `GET /menu`: Retrieve all menu items.
  - `GET /menu/{id}`: Retrieve a specific menu item.
  - `PUT /menu/{id}`: Update a menu item.
  - `DELETE /menu/{id}`: Delete a menu item.

- **Inventory:**

  - `POST /inventory`: Add a new inventory item.
  - `GET /inventory`: Retrieve all inventory items.
  - `GET /inventory/{id}`: Retrieve a specific inventory item.
  - `PUT /inventory/{id}`: Update an inventory item.
  - `DELETE /inventory/{id}`: Delete an inventory item.

- **Aggregations:**

  - `GET /reports/total-sales`: Get the total sales amount.
  - `GET /reports/popular-items`: Get a list of popular menu items.

**Examples:**

- **Create Order Request:**
```http 
POST /orders
Content-Type: application/json

{
  "customer_name": "John Doe",
  "items": [
    {
      "product_id": "espresso",
      "quantity": 2
    },
    {
      "product_id": "croissant",
      "quantity": 1
    }
  ]
}
```

- **Total Sales Aggregation Response:**
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "total_sales": 1500.50
}
```
### Logging
- Use Go's log/slog package for logging throughout the application.
- Log significant events, errors information with appropriate levels (`Info`, `Warning`, `Error`).
- Include contextual information in logs (e.g., timestamps, IDs).

**Example:**

```go
slog.Info("Order created", "orderID", newOrder.ID)
slog.Error("Failed to update inventory", err)
```

### Error Handling and Validation
- Validate input and provide meaningful error messages.
- Return appropriate HTTP status codes:
  - `200 OK` for successful GET requests.
  - `201 Created` for successful POST requests.
  - `400 Bad Request` for invalid input.
  - `404 Not Found` when resources are not found.
  - `500 Internal Server Error` for unexpected errors.
- Ensure error responses include a message explaining the error.

**Example:**

```http 
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Invalid product ID in order items."
}
```





---

**Order Management**
- **Create Orders**: Implement an endpoint to create new orders with customer details and items.
- **Retrieve Orders**: Implement endpoints to retrieve all orders or a specific order by ID.
- **Update Orders**: Implement an endpoint to modify existing orders.
- **Delete Orders**: Implement an endpoint to delete orders.

**Constraints**:

- Validate all input data.
- Ensure that product IDs in orders exist in the menu.
- Only allow modifications to orders that are still open.
- Closed orders cannot be modified or deleted.

**Additional Data Management** 

- **Menu Management**:

  - **Create Menu Item**: Add new products to the menu.
  - **Retrieve Menu Items**: Get a list of all menu items or a specific item.
  - **Update Menu Item**: Modify details of a menu item.
  - **Delete Menu Item**: Remove an item from the menu.

- **Inventory Management**:

  - Track ingredient stock levels.
  - Decrease stock levels when orders are placed.
  - Prevent orders if insufficient stock is available.
  - Implement endpoints to update and retrieve inventory levels.

- **Customer Management**:

  - Store customer details and order history.
  - Implement endpoints to retrieve customer information and order history.

### Logging
- Use `log/slog` to log significant events, errors, and information useful for debugging.
- Include contextual information in logs (e.g., timestamps, order IDs).
- Log at appropriate levels: `Info`, `Warning`, `Error`.

### Error Handling and Validation
- Validate input and provide meaningful error messages.
- Return appropriate HTTP status codes:
  - `200 OK` for successful GET requests.
  - `201 Created` for successful POST requests.
  - `400 Bad Request` for invalid input.
  - `404 Not Found` when resources are not found.
  - `500 Internal Server Error` for unexpected errors. 
- Ensure error responses include a message explaining the error.

### Layered Software Architecture



---


### Usage

Your program must be able to print usage information.

Outcomes:
- Program prints usage text.

```sh
$ ./hot-coffee --help
Coffee Shop Managment System

Usage:
  hot-coffee [-port <N>] [-dir <S>] 
  hot-coffee --help

**Options:**
- --help     Show this screen.
- --port N   Port number
- --dir S    Path to the data directory
```

## Support



## Guidelines from Author



## Author

This project has been created by:

Savva Savostyanov

Contacts:

- Email: [savvax@savvax.com](mailto:savvax@savvax.com)
- [GitHub](https://github.com/savvax/)
- [LinkedIn](https://www.linkedin.com/in/savvax/)
