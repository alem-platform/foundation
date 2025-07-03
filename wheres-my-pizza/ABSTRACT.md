# wheres-my-pizza

## Learning Objectives

- Message Queue Systems
- RabbitMQ Integration
- Concurrent Programming
- Microservices Architecture

## Abstract

In this project, you will build a distributed restaurant order management system. Using Go, you will create several microservices that communicate asynchronously via a RabbitMQ message broker, with order data persisted in a PostgreSQL database. This system will simulate a real-world restaurant workflow, from an order being placed via an API, to it being cooked by a kitchen worker, and finally its status being tracked. This project teaches a fundamental lesson in modern software engineering: think about the architecture first. Before writing a single line of code, you must design how services will interact, how data will flow, and how the system can scale.