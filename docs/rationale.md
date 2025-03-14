# Design Decisions and Rationale

This document outlines the major technical choices made during the development of the crypto payment gateway and the rationale behind each decision.

## 1. **Binance Smart Chain (BSC) as the Initial Blockchain**

### Reason:
- **Low Transaction Fees**: The platform will handle small transactions, often less than $10. BSC provides a cost-effective option with its lower transaction fees compared to networks like Ethereum.
- **Fast Transaction Speeds**: BSC offers fast block times, ensuring that users don't experience long wait times during transactions.
- **High Adoption**: BSC has wide support and adoption across various wallets and exchanges, which makes it a suitable choice for both crypto-native and less-experienced users.

## 2. **FastAPI for Transaction Management**

### Reason:
- **Asynchronous Capabilities**: FastAPI supports async operations natively, making it well-suited for handling multiple blockchain interactions simultaneously (e.g., querying blockchain statuses, monitoring transactions).
- **High Performance**: FastAPI offers better performance than Django or Flask for API-related tasks, making it suitable for a service that needs to manage real-time blockchain interactions and payment verification.
- **Scalability**: FastAPI’s non-blocking features help scale efficiently with high traffic and numerous transactions.

## 3. **Django for Seller Dashboard**

### Reason:
- **Rich Admin Interface**: Django’s built-in admin panel allows rapid development of the seller management interface. Sellers can manage wallet addresses, track transactions, and view logs with minimal custom development.
- **ORM and Database Management**: Django’s ORM simplifies database operations and integrates well with complex relational database models, making it ideal for managing seller data and transaction history.
- **Security**: Django comes with robust security features (CSRF protection, SQL injection prevention) and simplifies handling authentication and authorization.

## 4. **React for Buyer UI**

### Reason:
- **Separation of Concerns**: React allows us to build a modern, user-friendly interface for buyers without coupling it with the backend. This separation makes it easy to scale, update, and maintain.
- **Performance**: React is highly performant when rendering dynamic content, making it a great choice for a user interface that needs to update frequently (e.g., showing transaction statuses).
- **Component Reusability**: React's component-based architecture allows for building reusable UI components, making future scaling and enhancements easier.

## 5. **Traefik as the API Gateway**

### Reason:
- **Dynamic Routing**: Traefik is excellent for routing between microservices dynamically, which fits well with our microservice-based architecture.
- **Load Balancing**: Traefik provides built-in load balancing, ensuring even distribution of API calls between the services.
- **SSL/TLS Support**: Traefik simplifies SSL certificate management, improving the security of the system by enabling HTTPS with ease.

## 6. **PostgreSQL as the Database**

### Reason:
- **Relational Data Management**: Our system needs to store structured data such as sellers, wallet addresses, transaction histories, and logs. PostgreSQL’s relational database model fits well for these requirements.
- **Asynchronous Support**: PostgreSQL has excellent support for asynchronous queries, which is compatible with FastAPI’s async capabilities.
- **Scalability**: PostgreSQL can scale well for high-traffic applications and offers robust transaction integrity, which is crucial for handling financial data.

## 7. **RabbitMQ as the Message Broker**

### Reason:
- **Asynchronous Communication**: RabbitMQ is chosen for handling inter-service communication asynchronously, ensuring decoupling between the services. This fits well with the event-driven design of the system.
- **Reliability**: RabbitMQ is a proven solution for reliable message delivery, which is essential when handling sensitive financial transactions.
- **Scalability**: RabbitMQ is known for scaling easily, handling large volumes of messages between microservices.

## 8. **HashiCorp Vault for Secret Management**

### Reason:
- **Security**: HashiCorp Vault is designed to handle sensitive data such as API keys, blockchain private keys, and credentials. It encrypts secrets and ensures that only authorized services can access them.
- **Centralized Secret Management**: Vault allows us to centralize all sensitive information and provides a secure way to access these secrets across multiple microservices.
- **Dynamic Secrets**: Vault also supports dynamic secrets, which could be beneficial in generating time-bound or one-time access credentials for future needs.

## 9. **Prometheus and ELK Stack for Monitoring and Logging**

### Reason:
- **Prometheus for Monitoring**: Prometheus is used for real-time monitoring and metric collection from all microservices. It is well-suited for monitoring API health, transaction throughput, and performance metrics.
- **ELK Stack for Logging**: The ELK (Elasticsearch, Logstash, Kibana) stack is chosen for centralized logging, enabling the team to easily search and analyze logs across services. This is critical for debugging, error tracking, and audit trails.

## 10. **Microservice Architecture**

### Reason:
- **Separation of Concerns**: By dividing the system into multiple microservices (Transaction Manager, Seller Dashboard, Buyer UI), we ensure that each service has a clear responsibility and can be developed, maintained, and scaled independently.
- **Scalability**: Microservice architecture allows horizontal scaling, where different parts of the system can scale based on demand (e.g., scaling the transaction manager separately from the seller dashboard).
- **Technology Flexibility**: Microservices allow us to choose different technologies for different components (e.g., FastAPI for transaction handling, Django for the dashboard), ensuring that we use the best tool for each task.

## 11. **Event-Driven Design**

### Reason:
- **Decoupling**: Event-driven design ensures that services remain decoupled. For instance, the Seller Dashboard doesn't need to directly query the blockchain. Instead, it listens for transaction events via RabbitMQ and updates the database accordingly.
- **Scalability**: Event-driven architectures are inherently scalable, as they allow for the independent scaling of services based on the number of events.
- **Real-time Event Processing**: This approach allows the platform to process blockchain events (e.g., transaction confirmations) in real time, which is crucial for providing accurate transaction statuses to users.


## 12. **Rationale for Choosing Tortoise ORM**

### 1. **Asynchronous by Design**
Tortoise ORM is designed to work natively with asynchronous Python frameworks such as FastAPI, which we are using for the transaction service. Since blockchain interactions (such as generating wallet addresses, tracking transactions, etc.) are often I/O-bound, leveraging an async ORM will allow us to maintain non-blocking operations, improving the overall performance of our application, especially under heavy load.

### 2. **Lightweight and Fast**
Compared to more complex ORM frameworks like SQLAlchemy, Tortoise ORM is lightweight and straightforward. This aligns well with our project goals of creating a highly performant and scalable crypto payment gateway. The minimalist nature of Tortoise ORM ensures that we only include essential features, which contributes to faster performance.

### 3. **Simple API**
Tortoise ORM offers a clean and simple API similar to Django ORM, making it easier for developers familiar with Django to transition to Tortoise. Its declarative style allows us to define models clearly and concisely, reducing boilerplate code while maintaining readability.

### 4. **Compatibility with Supported Databases**
Tortoise ORM supports multiple relational databases such as PostgreSQL, which is the database we have chosen for this project. This gives us flexibility if we decide to scale or introduce other databases in the future. The native support for PostgreSQL’s asynchronous drivers further ensures smooth integration.

### 5. **Schema Generation**
Tortoise ORM provides built-in schema generation features, which can be useful in simplifying the database migration process during development and deployment. It also supports easy rollback mechanisms, reducing the risk during schema changes or updates.

### 6. **Active Community and Documentation**
Tortoise ORM has an active and growing community, along with well-maintained documentation. This support is crucial for addressing any challenges that arise and for ensuring that the ORM is continuously improved and updated.

