# airbnb-clone-project
##  Objective
The backend for the Airbnb Clone project is designed to provide a robust and scalable foundation for managing user interactions, property listings, bookings, and payments. This backend will support various functionalities required to mimic the core features of Airbnb, ensuring a smooth experience for users and hosts.

##  Project Goals
1. **User Management:** Implement a secure system for user registration, authentication, and profile management.
1. **Property Management:** Develop features for property listing creation, updates, and retrieval.
1. **Booking System:** Create a booking mechanism for users to reserve properties and manage booking details.
1. **Payment Processing:** Integrate a payment system to handle transactions and record payment details.
1. **Review System:** Allow users to leave reviews and ratings for properties.
1. **Data Optimization:** Ensure efficient data retrieval and storage through database optimizations.

## Feature Breakdown
1. **API Documentation**
  - OpenAPI Standard: The backend APIs are documented using the OpenAPI standard to ensure clarity and ease of integration.
  - Django REST Framework: Provides a comprehensive RESTful API for handling CRUD operations on user and property data.
  - GraphQL: Offers a flexible and efficient query mechanism for interacting with the backend.
1. **User Management and Authentication**
  - Endpoints: /users/, /users/{user_id}/
  - Features: Register new users, authenticate, and manage user profiles.
1. **Property Management**
  - Endpoints: /properties/, /properties/{property_id}/
  - Features: Create, update, retrieve, and delete property listings.
1. **Booking System**
  - Endpoints: /bookings/, /bookings/{booking_id}/
  - Features: Make, update, and manage bookings, including check-in and check-out details.
1. **Payment Processing**
  - Endpoints: /payments/
  - Features: Handle payment transactions related to bookings.
1. **Review System**
  - Endpoints: /reviews/, /reviews/{review_id}/
  - Features: Post and manage reviews for properties.
1. **Database Optimizations**
  - Indexing: Implement indexes for fast retrieval of frequently accessed data.
  - Caching: Use caching strategies to reduce database load and improve performance.

## Technology Stack
- Django: A high-level Python web framework used for building the RESTful API.
- Django REST Framework: Provides tools for creating and managing RESTful APIs.
- PostgreSQL: A powerful relational database used for data storage.
- GraphQL: Allows for flexible and efficient querying of data.
- Celery: For handling asynchronous tasks such as sending notifications or processing payments.
- Redis: Used for caching and session management.
- Docker: Containerization tool for consistent development and deployment environments.
- CI/CD Pipelines: Automated pipelines for testing and deploying code changes.
## Database Design
This database schema outlines the structure and relationships between the main entities of an Airbnb clone application.
The Airbnb clone database will have the following tables:
1. users
2. properties
3. bookings
4. reviews
5. payments

#### 1. Users Table

Stores information about guests, hosts, and admins.

| Field Name    | Data Type       | Constraints                        | Description |
|----------------|-----------------|------------------------------------|-------------|
| user_id        | BIGINT (PK)     | PRIMARY KEY, AUTO INCREMENT        | Unique identifier for each user |
| first_name     | VARCHAR(100)    | NOT NULL                           | User’s first name |
| last_name      | VARCHAR(100)    | NOT NULL                           | User’s last name |
| email          | VARCHAR(255)    | UNIQUE, NOT NULL                   | User’s email (login credential) |
| password_hash  | VARCHAR(255)    | NOT NULL                           | Hashed password |
| phone_number   | VARCHAR(20)     | NULL                               | User’s contact number |
| user_type      | ENUM('guest','host','admin') | DEFAULT 'guest' | Type of user |
| created_at     | DATETIME        | DEFAULT CURRENT_TIMESTAMP          | Account creation timestamp |
| updated_at     | DATETIME        | ON UPDATE CURRENT_TIMESTAMP        | Last update timestamp |

---

####  2. Properties Table

Holds details about listings created by hosts.

| Field Name      | Data Type       | Constraints                        | Description |
|-----------------|-----------------|------------------------------------|-------------|
| property_id     | BIGINT (PK)     | PRIMARY KEY, AUTO INCREMENT        | Unique property ID |
| host_id         | BIGINT (FK)     | REFERENCES users(user_id)          | The host who owns this property |
| title           | VARCHAR(255)    | NOT NULL                           | Listing title |
| description     | TEXT            | NULL                               | Description of the property |
| address         | VARCHAR(255)    | NOT NULL                           | Property address |
| city            | VARCHAR(100)    | NOT NULL                           | City |
| country         | VARCHAR(100)    | NOT NULL                           | Country |
| price_per_night | DECIMAL(10,2)   | NOT NULL                           | Cost per night |
| max_guests      | INT             | DEFAULT 1                          | Maximum allowed guests |
| created_at      | DATETIME        | DEFAULT CURRENT_TIMESTAMP          | Listing creation timestamp |
| updated_at      | DATETIME        | ON UPDATE CURRENT_TIMESTAMP        | Last update timestamp |

---

####  3. Bookings Table

Tracks reservations made by guests.

| Field Name     | Data Type       | Constraints                        | Description |
|----------------|-----------------|------------------------------------|-------------|
| booking_id     | BIGINT (PK)     | PRIMARY KEY, AUTO INCREMENT        | Unique booking ID |
| property_id    | BIGINT (FK)     | REFERENCES properties(property_id) | The booked property |
| guest_id       | BIGINT (FK)     | REFERENCES users(user_id)          | Guest who made the booking |
| check_in_date  | DATE            | NOT NULL                           | Check-in date |
| check_out_date | DATE            | NOT NULL                           | Check-out date |
| total_price    | DECIMAL(10,2)   | NOT NULL                           | Total cost of stay |
| status         | ENUM('pending','confirmed','cancelled', 'completed') | DEFAULT 'pending' | Booking status |
| created_at     | DATETIME        | DEFAULT CURRENT_TIMESTAMP          | Booking creation timestamp |
| updated_at     | DATETIME        | ON UPDATE CURRENT_TIMESTAMP        | Last update timestamp |

---

####  4. Reviews Table

Guests leave reviews for properties they’ve stayed at.

| Field Name  | Data Type       | Constraints                        | Description |
|--------------|-----------------|------------------------------------|-------------|
| review_id    | BIGINT (PK)     | PRIMARY KEY, AUTO INCREMENT        | Unique review ID |
| booking_id   | BIGINT (FK)     | REFERENCES bookings(booking_id)    | Review is tied to a booking |
| property_id  | BIGINT (FK)     | REFERENCES properties(property_id) | The reviewed property |
| guest_id     | BIGINT (FK)     | REFERENCES users(user_id)          | Guest who wrote the review |
| rating       | INT             | CHECK (rating BETWEEN 1 AND 5)     | Star rating (1–5) |
| comment      | TEXT            | NULL                               | Review text |
| created_at   | DATETIME        | DEFAULT CURRENT_TIMESTAMP          | Review creation timestamp |

---

#### 5. Payments Table

Records all payment transactions related to bookings.

| Field Name      | Data Type                                            | Constraints                        | Description |
|-----------------|------------------------------------------------------|------------------------------------|-------------|
| payment_id      | BIGINT (PK)                                          | PRIMARY KEY, AUTO INCREMENT        | Unique payment ID |
| booking_id      | BIGINT (FK)                                          | REFERENCES bookings(booking_id)    | Booking being paid for |
| user_id         | BIGINT (FK)                                          | REFERENCES users(user_id)          | The paying user (guest) |
| amount          | DECIMAL(10,2)                                        | NOT NULL                           | Payment amount |
| payment_method  | ENUM('credit_card','paypal','bank_transfer','other') | NOT NULL                           | Payment method |
| status          | ENUM('pending', 'completed',' failed', 'refunded')   | DEFAULT 'pending'                  | Payment status |
| transaction_date| DATETIME                                             | DEFAULT CURRENT_TIMESTAMP          | Payment timestamp |


## Team Roles
- Backend Developer: Responsible for implementing API endpoints, database schemas, and business logic.
- Frontend Developer: Responsible for implementing the User Interface and business logic on the frontend.
- Database Administrator: Manages database design, indexing, and optimizations.
- DevOps Engineer: Handles deployment, monitoring, and scaling of the backend services.
- QA Engineer: Ensures the backend functionalities are thoroughly tested and meet quality standards.

## CI/CD Pipeline Overview
The goal of the CI/CD pipeline is to ensure that every change pushed to the repository is automatically **built**, **tested**, **containerized**, and optionally **deployed**.
---
### CI/CD Tools

Before setting up the pipeline, ensure you have the following:

- A **GitHub repository**, and **GitHub Actions** for your backend project.  
- **Docker** installed locally for building and testing images.  
- A **Docker Hub** or container registry account (e.g., GitHub Container Registry, AWS ECR).  
- Access credentials (e.g., Docker Hub token) stored as **GitHub Secrets**.  
- A valid **Dockerfile** in the project root directory.

### Local Development Workflow
1. Build and run locally using Docker:
  ```
  docker build
  docker run
  ```
1. Make code changes and commit.
1. Push to GitHub — the workflow runs automatically.

## API Security

Security is a critical part of any backend application, especially when handling sensitive data, user authentication, and financial transactions. This section outlines the key security measures that will be implemented and explains why each one is essential for protecting the system and its users.

---

### 1. Key Security Measures

#### **1.1 Authentication**
All API endpoints will be protected by robust authentication mechanisms such as **JWT (JSON Web Tokens)** or **OAuth 2.0**.

- **Implementation:**
  - Users must log in with valid credentials to receive a secure access token.
  - Tokens are verified with each request to ensure authenticity and session validity.

- **Why It Matters:**
  Authentication prevents unauthorized access to APIs, ensuring that only verified users and services can interact with the system.

---

#### **1.2 Authorization**
Authorization controls what authenticated users are allowed to do within the system.

- **Implementation:**
  - Role-Based Access Control (RBAC) will define user roles such as *admin*, *moderator*, and *standard user*.
  - Endpoints will enforce role restrictions using middleware.

- **Why It Matters:**
  Proper authorization ensures users can only perform actions permitted by their roles, preventing privilege escalation and data misuse.

---

#### **1.3 Rate Limiting**
Rate limiting restricts the number of API requests from a single client within a defined timeframe.

- **Implementation:**
  - Use middleware such as **express-rate-limit** (for Node.js) or **Django Ratelimit** (for Python).
  - Limit repeated requests to protect against brute-force and DoS attacks.

- **Why It Matters:**
  Rate limiting prevents abuse, maintains performance stability, and protects the API from malicious traffic.

---

#### **1.4 Data Encryption**
Sensitive data, including user credentials and payment information, will be encrypted in transit and at rest.

- **Implementation:**
  - Enable **HTTPS** with SSL/TLS certificates.
  - Use strong encryption algorithms (e.g., AES-256) for stored data.
  - Hash passwords using bcrypt or Argon2.

- **Why It Matters:**
  Encryption ensures that even if data is intercepted or breached, it remains unreadable and secure.

---

#### **1.5 Input Validation and Sanitization**
All incoming API requests will undergo strict validation to prevent injection attacks and data corruption.

- **Implementation:**
  - Validate request payloads using libraries like `Joi`, `Pydantic`, or `class-validator`.
  - Sanitize inputs to remove malicious code or scripts.

- **Why It Matters:**
  Input validation defends against SQL injection, cross-site scripting (XSS), and other common web vulnerabilities.

---

#### **1.6 Logging and Monitoring**
Comprehensive logging and monitoring will be implemented to track API activity and detect suspicious behavior.

- **Implementation:**
  - Use centralized logging tools (e.g., ELK Stack, Grafana Loki).
  - Implement alerting for abnormal request patterns or repeated failed logins.

- **Why It Matters:**
  Monitoring enhances visibility into system behavior, allowing for the early detection of security breaches.

---

#### **1.7 Secure API Keys and Secrets Management**
All API keys, credentials, and secrets will be securely managed and stored, never in plain text.

- **Implementation:**
  - Use environment variables and GitHub Secrets for sensitive data.
  - Rotate keys periodically.

- **Why It Matters:**
  Secure secret management prevents unauthorized access to third-party services and internal APIs.

---

### 2. Importance of Security Across Project Areas

| **Area** | **Why Security is Crucial** |
|-----------|------------------------------|
| **User Data** | Protects personal information such as emails, passwords, and profile data from leaks or identity theft. |
| **Payments & Transactions** | Ensures financial data and payment operations are secure, preventing fraud and unauthorized charges. |
| **Internal APIs** | Prevents unauthorized microservice interactions that could lead to data breaches or service disruptions. |
| **Admin Operations** | Ensures administrative endpoints are restricted to privileged users to prevent misuse of system-level controls. |
| **Logs and Monitoring Data** | Protects system and audit logs from tampering, which helps maintain accountability and compliance. |

