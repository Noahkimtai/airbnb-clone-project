# airbnb-clone-project

## Technology Stack
- Django: A high-level Python web framework used for building the RESTful API.
- Django REST Framework: Provides tools for creating and managing RESTful APIs.
- PostgreSQL: A powerful relational database used for data storage.
- GraphQL: Allows for flexible and efficient querying of data.
- Celery: For handling asynchronous tasks such as sending notifications or processing payments.
- Redis: Used for caching and session management.
- Docker: Containerization tool for consistent development and deployment environments.
- CI/CD Pipelines: Automated pipelines for testing and deploying code changes.

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
