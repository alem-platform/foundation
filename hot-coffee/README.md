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

- **Manage Orders:** Create, modify, close, and delete customer orders efficiently.
- **Oversee Inventory:** Track ingredient stock levels to prevent shortages and ensure freshness.
- **Update the Menu:** Add new drinks or pastries, adjust prices, and keep the offerings current.

While commercial systems are complex and built for large-scale operations, this project focuses on the essentials:

- **REST API:** You'll build a web service that allows different parts of the system—or even different systems—to communicate seamlessly over HTTP.
- **JSON Data Handling:** Learn how to encode and decode JSON to transmit data in a format that's both human-readable and machine-friendly.
- **Data Storage with JSON Files:** Instead of using a database, you will store all data locally in JSON files, managing orders, menu items, and ingredients.
- **Layered Architecture:** Implement a structured approach to software design that separates concerns into different layers, making your code more maintainable and scalable.
- **Logging:** Use Go's log/slog package to record significant events, aiding in debugging and monitoring the system's health.
This project is a practical exploration of how order management and inventory systems operate under the hood, including how they handle data processing, manage concurrent requests, and ensure data integrity. By building the coffee shop management system, you'll dive into key topics like RESTful API design, data serialization, and software architecture principles.

Whether you're aiming to grasp the basics of backend development or preparing to work on real-world enterprise applications, this project offers a hands-on approach to learning these essential concepts. It's an opportunity to understand not just how to write code, but how to design systems that solve real problems—skills that are invaluable in the software development industry.

---
## Resources

- Read about REST and API [here](https://en.wikipedia.org/wiki/REST), [here](https://en.wikipedia.org/wiki/API) and [here](https://aws.amazon.com/what-is/restful-api/).
- Read about JSON [here](https://www.json.org/json-en.html) and [here](https://developer.mozilla.org/en-US/docs/Glossary/JSON).
- Read about Logging [here](https://gobyexample.com/logging), [here](https://pkg.go.dev/log/slog@go1.23.1) and [here](https://betterstack.com/community/guides/logging/logging-in-go/).

---

## General Criteria

- Your code MUST be written in accordance with [gofumpt](https://github.com/mvdan/gofumpt). If not, you will be graded `0` automatically.
- Your program MUST be able to compile successfully.
- Your program MUST not exit unexpectedly (any panics: `nil-pointer dereference`, `index out of range` etc.). If so, you will be get `0` during the defence.
- Only built-in packages are allowed. If not, you will get `0` grade.
- The project MUST be compiled by the following command in the project's root directory:

```sh
$ go build -o hot-coffee .
```
- If an error occurs during startup (e.g., invalid command-line arguments, failure to bind to a port), the program must exit with a non-zero status code and display a clear, understandable error message.
  During normal operation, the server must handle errors gracefully, returning appropriate HTTP status codes to the client without crashing.
---
## Mandatory Part

### Baseline
Your task is to develop the `hot-coffee` application, a **coffee shop management system** that provides a RESTful API for managing orders, menu items, and inventory. The application should be built using Go and adhere to a layered software architecture with a focus on clean code, maintainability, and scalability.

#### Outcomes:

- Use a three-layered architecture:
  - Presentation Layer (Handlers): Manages HTTP requests and responses.
  - Business Logic Layer (Services): Contains core functionality and business rules.
  - Data Access Layer (Repositories): Handles data storage and retrieval.
- Implement HTTP endpoints for creating, retrieving, updating, and deleting orders, menu items, inventory, aggregations.
- Store data locally in JSON files. Data should be stored in separate JSON files for each entity, such as `orders.json`, `menu_items.json` and `inventory.json` located in a designated `data/` directory.
- Perform aggregations based on the stored data (e.g., total sales, popular menu items).
- Utilize Go's `log/slog` package for logging significant events.

#### Notes:

- The API should handle JSON requests and responses.
- Data must be persisted in JSON files.
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
  - Organize handlers based on entities (e.g., `order_handler.go`, `menu_handler.go`, `inventory_handler.go`).
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
│   │   └── ...
│   ├── service/
│   │   ├── order_service.go
│   │   ├── menu_service.go
│   │   ├── inventory_service.go
│   │   └── ...
│   └── dal/
│       ├── order_repository.go
│       └── ...
├── models/
│   ├── order.go
│   └── ...
├── go.mod
├── go.sum
└── ...
```

**Notes:**

- `cmd/`: Entry point of the application.
- `internal/`: Application code divided into layers.
  - `handler/`: Presentation layer handling HTTP requests.
  - `service/`: Business logic layer with interfaces and implementations.
  - `dal/`: Data access layer with interfaces and implementations.
- `models/`: Data models shared across layers.
- `go.mod` and `go.sum`: Go module files.

*You can use any layer names, but their essence must be conveyed in the name.

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
  ID          string                `json:"product_id"`
  Name        string                `json:"name"`
  Description string                `json:"description"`
  Price       float64               `json:"price"`
  Ingredients []MenuItemIngredient  `json:"ingredients"`
}

type MenuItemIngredient struct {
  IngredientID string `json:"ingredient_id"`
  Quantity     int    `json:"quantity"`
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

#### Updating Inventory Upon Order Fulfillment
When an order is created and processed, the application must:

- Check Inventory Levels:
  - Before confirming an order, verify that there are sufficient quantities of all required ingredients in `inventory.json`.
  - If any ingredient is insufficient, the order should not be processed, and an appropriate error message should be returned.
- Deduct Ingredients:
  - Upon successful processing of an order, deduct the required quantities of each ingredient from `inventory.json`.

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

### Data Storage with JSON Files
As part of the project requirements, you must use JSON files as the data storage mechanism instead of a database. The application should store all data locally in JSON files, with each entity stored in its own separate file within the `data/` directory. 

#### Entities to Store:

- Orders: Information about customer orders.
- Menu Items: Details about the products available in the coffee shop.
- Inventory Items: Inventory of ingredients required to prepare menu items.

#### Requirements:

- **No Database Usage:** Do not use any database systems. All data must be stored and retrieved from JSON files.
- **Separate Files for Each Entity:** Each entity should have its own JSON file:
  - `orders.json` for orders.
  - `menu_items.json` for menu items.
  - `inventory.json` for inventory items.
- **Include Ingredients in Menu Items**: `menu_items.json` should include the list of ingredients required for each menu item.
- **Update Inventory on Order Fulfillment**: When an order is fulfilled, the application must update `inventory.json` by deducting the quantities of ingredients used.
- **Data Format**: Data should be stored in structured JSON formats that accurately represent the entities.

#### Examples of JSON Files:

1. `orders.json`:

```json
[
  {
    "order_id": "order123",
    "customer_name": "Alice Smith",
    "items": [
      {
        "product_id": "latte",
        "quantity": 2
      },
      {
        "product_id": "muffin",
        "quantity": 1
      }
    ],
    "status": "open",
    "created_at": "2023-10-01T09:00:00Z"
  },
  {
    "order_id": "order124",
    "customer_name": "Bob Johnson",
    "items": [
      {
        "product_id": "espresso",
        "quantity": 1
      }
    ],
    "status": "closed",
    "created_at": "2023-10-01T09:30:00Z"
  }
]
```

2. `menu_items.json`:
```json
[
  {
    "product_id": "latte",
    "name": "Caffe Latte",
    "description": "Espresso with steamed milk",
    "price": 3.50,
    "ingredients": [
      {
        "ingredient_id": "espresso_shot",
        "quantity": 1
      },
      {
        "ingredient_id": "milk",
        "quantity": 200
      }
    ]
  },
  {
    "product_id": "muffin",
    "name": "Blueberry Muffin",
    "description": "Freshly baked muffin with blueberries",
    "price": 2.00,
    "ingredients": [
      {
        "ingredient_id": "flour",
        "quantity": 100
      },
      {
        "ingredient_id": "blueberries",
        "quantity": 20
      },
      {
        "ingredient_id": "sugar",
        "quantity": 30
      }
    ]
  },
  {
    "product_id": "espresso",
    "name": "Espresso",
    "description": "Strong and bold coffee",
    "price": 2.50,
    "ingredients": [
      {
        "ingredient_id": "espresso_shot",
        "quantity": 1
      }
    ]
  }
]
```

**Note:** The ingredients field in each menu item lists the ingredients required to prepare that item. The quantity is specified in units appropriate for the ingredient (e.g., grams, milliliters).

3. `inventory.json`:

```json
[
  {
    "ingredient_id": "espresso_shot",
    "name": "Espresso Shot",
    "quantity": 500, // Number of shots
    "unit": "shots"
  },
  {
    "ingredient_id": "milk",
    "name": "Milk",
    "quantity": 5000, // In milliliters
    "unit": "ml"
  },
  {
    "ingredient_id": "flour",
    "name": "Flour",
    "quantity": 10000, // In grams
    "unit": "g"
  },
  {
    "ingredient_id": "blueberries",
    "name": "Blueberries",
    "quantity": 2000, // In grams
    "unit": "g"
  },
  {
    "ingredient_id": "sugar",
    "name": "Sugar",
    "quantity": 5000, // In grams
    "unit": "g"
  }
]
```


### Logging
- Use Go's `log/slog` package for logging throughout the application.
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

**Examples:**

```http 
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Invalid product ID in order items."
}
```

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Insufficient inventory for ingredient 'Milk'. Required: 200ml, Available: 150ml."
}

```

### Usage

Your program must be able to print usage information.

Outcomes:
- Program prints usage text.

```sh
$ ./hot-coffee --help
Coffee Shop Management System

Usage:
  hot-coffee [--port <N>] [--dir <S>] 
  hot-coffee --help

Options:
  --help       Show this screen.
  --port N     Port number.
  --dir S      Path to the data directory.
```

---

## Support

Pay special attention to the interfaces. 

Don't forget to add drinks and food to the menu and bring the ingredients to your coffee shop before it opens.

## Guidelines from Author
Before you start writing code, think carefully about the structure of your future application. First, learn how the layered architecture of the application works. This will help to save a lot of time.

Try to implement the addition of a new inventory item and three levels first. This can help with understanding the complex architecture of the application, after which it will be much more convenient to implement the rest of the functionality without rewriting a large amount of code.

Also remember about the SOLID.

## Author

This project has been created by:

Savva Savostyanov

Contacts:

- Email: [savvax@savvax.com](mailto:savvax@savvax.com)
- [GitHub](https://github.com/savvax/)
- [LinkedIn](https://www.linkedin.com/in/savvax/)
