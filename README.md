# Blog Management Application

## Table of Contents

- [Introduction](#introduction)
- [Setup Instructions](#setup-instructions)
- [Running the Application](#running-the-application)
- [Design Decisions](#design-decisions)
- [Application Structure](#application-structure)

## Introduction
This is a full-stack Blog Management application developed using .NET Core for the backend and Angular for the frontend. 
The application allows users to create, read, update, and delete blog posts. The backend follows the CQRS (Command Query Responsibility Segregation) pattern, and the frontend is developed using Angular.

## Setup Instructions

### Prerequisites
- .NET Core SDK 6.0 or later
- Node.js 14 or later
- Angular CLI 12 or later
- Visual Studio 2022 (recommended for .NET development)
- Git

### Backend Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/sudiptamax/BlogApplication.git

2. Navigate to the API directory:
3. cd Blog-Management-Application/BlogAPI
4. dotnet restore
5. dotnet ef database update
6. dotnet run


### Frontend Setup

1. Navigate to the frontend directory:
2. Install the dependencies:
3. npm install
4. npm install -g @angular/cli
5. npm install @angular/core @angular/common @angular/compiler @angular/platform-browser @angular/platform-browser-dynamic @angular/router
6. npm install @angular/forms
7. npm install ag-grid-angular ag-grid-community
8. npm install @angular/material @angular/cdk @angular/animations
9. npm install @angular/core
10. npm install @angular-devkit/build-angular
11. ng start
12. http://localhost:4200.

### Running the Application

**Backend**
To run the backend API server:
Step 1: Install .NET 6/7 SDK
Step 2: Install Visual Studio 2022

dotnet run

**Frontend**
cd Blog-Management-Application/BlogUI/ui

npm install -g @angular/cli
npm install @angular/core @angular/common @angular/compiler @angular/platform-browser @angular/platform-browser-dynamic @angular/router
npm install @angular/forms
npm install ag-grid-angular ag-grid-community
npm install @angular/material @angular/cdk @angular/animations
npm install @angular/core
npm start

Navigate to http://localhost:4200 in your web browser.

### Design Decisions:


## BlogAppApi

The backend API is designed using the CQRS pattern to separate the concerns of reading and writing data. This allows for more scalable and maintainable code. Commands handle operations that change the state (create, update, delete), while queries handle read-only operations.
Angular Frontend

The frontend is developed using Angular for its powerful features and modular structure, which helps in creating a maintainable and scalable front-end application.
Encrypted IDs

For security reasons, blog post IDs are encrypted using SHA-256. This ensures that sensitive data is not exposed directly through URLs.

BlogAPI (CQRS-Based)

    CQRS Pattern (Command Query Responsibility Segregation):
        Decision: The CQRS pattern was chosen to separate the concerns of reading and writing data. Commands handle write operations, and queries handle read operations.
        Rationale: This separation allows each to be optimized and scaled independently, enhancing performance and flexibility. For example, write operations can be processed asynchronously to improve user experience, while read operations can be optimized for fast data retrieval.
        Implementation:
            Commands are responsible for modifying state (e.g., creating, updating, deleting blog posts).
            Queries are responsible for retrieving state (e.g., fetching blog posts).

    Handlers:
        Decision: Each command and query is handled by a dedicated handler.
        Rationale: This promotes the Single Responsibility Principle (SRP), where each handler has one reason to change, making the system easier to maintain and test.
        Implementation: For instance, CreateBlogPostHandler handles the logic for creating a blog post, while GetBlogPostByIdHandler handles retrieving a blog post by its ID.

    Repository Pattern:
        Decision: The repository pattern abstracts data access logic and provides a clean separation between the domain and data mapping layers.
        Rationale: This abstraction allows for easy swapping of the underlying data store (e.g., switching from JSON file storage to SQL Server) without affecting the rest of the application.
        Implementation: The IBlogRepository interface defines the contract, and BlogRepository implements this interface. It interacts with the data store (e.g., a JSON file in the current setup).
        
Dependency Injection (DI):

    Decision: Dependency injection is used throughout the application to manage dependencies.
    Rationale: DI promotes loose coupling, making the code more testable and easier to maintain.
    Implementation: In Startup.cs, services like IBlogRepository are registered with the DI container, allowing them to be injected into classes like handlers and controllers.

Singleton vs. Scoped Design Pattern:

    Decision: The repository was registered as a singleton in the DI container.
    Rationale: Given that the current storage mechanism is a JSON file, it was decided to use a singleton pattern to maintain state across the application's lifetime. In a more conventional setup with a database, a scoped lifetime might be preferred to ensure that each request has its own context.
    Implementation: services.AddSingleton<IBlogRepository, BlogRepository>(); in Startup.cs.

Error Handling Middleware:

    Decision: A custom middleware component is used for centralized error handling.
    Rationale: This ensures consistent error responses across the API and allows for logging and other error-handling logic to be easily managed.
    Implementation: The ErrorHandlerMiddleware class catches exceptions, logs them, and returns a structured error response.

Encryption and Security:

    Decision: Sensitive data such as blog post IDs are encrypted.
    Rationale: Encrypting IDs helps protect the integrity and security of the data, making it difficult for unauthorized users to manipulate or guess record IDs.
    Implementation: The EncryptionHelper class provides methods to encrypt and decrypt data, which is used by handlers and the repository.

Testing:

    Decision: Unit tests were created for various components to ensure the correctness of the application.
    Rationale: Testing is crucial for maintaining code quality, especially as the application evolves. Unit tests provide a safety net for refactoring and extending the application.
    Implementation: Tests for the API (e.g., BlogApiIntegrationTests) ensure that commands and queries function as expected.

## BlogUi

    Component-Based Architecture:
        Decision: The UI is built using Angular’s component-based architecture.
        Rationale: Components allow for the modularization of the UI, making it easier to manage, reuse, and test individual parts of the application.
        Implementation: Components like ListingComponent, CreateUpdateBlogComponent, and BlogDetailComponent encapsulate specific pieces of functionality.

    Reactive Forms:
        Decision: Reactive forms are used for the blog creation and update forms.
        Rationale: Reactive forms offer more control over form data and validation, making it easier to manage complex forms.
        Implementation: The CreateUpdateBlogComponent uses Angular’s FormGroup and FormControl to manage form state and validation.

    State Management:
        Decision: State is managed locally within components or passed between components using Angular’s dependency injection and services.
        Rationale: Angular’s built-in DI system allows for clean and maintainable state management within the application.
        Implementation: The BlogService handles data fetching and manipulation, while components subscribe to these services to update the UI.

    Routing:
        Decision: Angular's routing module is used to manage navigation between different views.
        Rationale: This enables a single-page application (SPA) experience, where different views can be loaded dynamically without full page reloads.
        Implementation: The AppRoutingModule defines routes for components like ListingComponent, CreateUpdateBlogComponent, and BlogDetailComponent.

    Responsive Design:
        Decision: The UI is designed to be responsive, adapting to different screen sizes.
        Rationale: With the growing use of mobile devices, it's important for the application to be usable on a wide range of devices.
        Implementation: CSS media queries and Angular’s flexible grid system are used to ensure the UI looks good on all devices.

    Error Handling:
        Decision: Error handling is performed at the service level, with errors propagated to the components.
        Rationale: Centralized error handling at the service level allows for consistent error management and user feedback across the application.
        Implementation: The BlogService handles errors using RxJS operators like catchError, and components display error messages accordingly.

    Optimization:
        Decision: Angular’s built-in tools for optimizing bundle size and performance are used.
        Rationale: Optimizing the client-side application is crucial for providing a fast and smooth user experience.
        Implementation: Angular CLI tools like ng build --prod are used to minify and bundle the application for production.

    Scalability:
        Decision: The UI is designed with scalability in mind, allowing for easy addition of new features and components.
        Rationale: As the application grows, it’s important that the codebase remains maintainable and extensible.
        Implementation: Components are designed to be modular and reusable, and state management can be extended to more complex systems like NgRx if needed.
        
## Integration Between BlogAPI and Angular UI

    API Design:
        Decision: The BlogAPI follows RESTful principles, providing clear and consistent endpoints for CRUD operations.
        Rationale: RESTful APIs are easy to consume and integrate well with front-end frameworks like Angular.
        Implementation: Endpoints like /api/blogs are consumed by Angular services to fetch, create, update, and delete blog posts.

    Data Transfer:
        Decision: Data is transferred between the API and UI in JSON format.
        Rationale: JSON is lightweight, easy to parse, and well-supported by Angular and .NET.
        Implementation: The BlogService in Angular handles HTTP requests to the API and processes the JSON responses.

    Error Propagation:
        Decision: Errors from the API are propagated to the UI to provide feedback to the user.
        Rationale: Providing users with meaningful error messages improves the user experience and helps them understand what went wrong.
        Implementation: The API returns structured error responses, which are handled by Angular’s service layer and displayed in the UI.

  Conclusion

  The design decisions made for the BlogAPI and Angular client UI reflect a focus on scalability, maintainability, and performance. 
  By leveraging modern design patterns like CQRS, the application is well-structured and can easily adapt to changing requirements. 
  The separation of concerns between the API and UI, as well as within each layer, ensures that the codebase remains clean, modular, and easy to extend.

## Application Structure

# Backend (BlogAppApi)

    Controllers: Handle HTTP requests and delegate to appropriate handlers.
    Handlers: Contain the business logic for handling commands and queries.
    Commands: Represent operations that change the state.
    Queries: Represent read-only operations.
    Repositories: Handle data access logic.
    Models: Represent the data structures used in the application.


- BlogApi/
    - Commands/
        - CreateBlogPostCommand.cs
        - DeleteBlogPostCommand.cs
        - UpdateBlogPostCommand.cs
    - Controllers/
        - BlogController.cs
    - Data/
        - blogPosts.json
    - Handlers/
        - CreateBlogPostHandler.cs
        - DeleteBlogPostHandler.cs
        - GetAllBlogPostsHandler.cs
        - GetBlogPostByEncryptedIdHandler.cs
        - UpdateBlogPostHandler.cs
    - Middleware/
        - ErrorHandlerMiddleware.cs
    - Models/
        - BlogPost.cs
    - Queries/
        - GetAllBlogPostsQuery.cs
        - GetBlogPostByEncryptedIdQuery.cs
    Repositories/
        - BlogRepository.cs
        - IBlogRepository.cs
    Shared/
        EncryptionHelper.cs
    - Validators/
    - appsettings.json
    - appsettings.Development.json
    - Program.cs
    - Startup.cs
    - UnitTest/
        - BlogApiIntegrationTests.cs
        - BlogControllerTests.cs
        - BlogServiceTests.cs
        - GlobalUsings.cs



## Frontend (BlogUI)

Components: Angular components for different views.
Services: Handle API calls and data manipulation.
Models: Define the data structures used in the frontend.
Routes: Define the navigation and routing for the application.


## BlogUI
- ui
    - angular.json
    - package.json
    - tsconfig.json
    - src
        - app
            - components
                - about
                    - about.component.css
                    - about.component.html
                    - about.component.spec.ts
                    - about.component.ts
                - blog-detail
                    - blog-detail.component.css
                    - blog-detail.component.html
                    - blog-detail.component.spec.ts
                    - blog-detail.component.ts
                - create-update-blog
                    - create-update-blog.component.css
                    - create-update-blog.component.html
                    - create-update-blog.component.spec.ts
                    - create-update-blog.component.ts
                - home
                    - home.component.css
                    - home.component.html
                    - home.component.spec.ts
                    - home.component.ts
                - listing
                    - listing.component.css
                    - listing.component.html
                    - listing.component.spec.ts
                    - listing.component.ts
            - models
                - blog.model.ts
            - services
                - blog-detail.service.spec.ts
                - blog-detail.service.ts
                - blog.service.spec.ts
                - blog.service.ts
                - notification.service.spec.ts
                - notification.service.ts
            - app-routing.module.ts
            - app.component.css
            - app.component.html
            - app.component.spec.ts
            - app.component.ts
            - app.module.ts
            - assets
        - environments
            - environment.prod.ts
            - environment.ts
        - index.html
        - main.ts
        - styles.css
    - .editorconfig
    - .gitignore
    - package-lock.json
    - README.md
    - tsconfig.app.json
    - tsconfig.spec.json


This README provides a guide to setting up, running, and understanding the design decisions behind your Blog Management application.
Potential Scalability Enhancements:

There are several ways this application can scale with a huge number of blog posts, as this application implementation architecture is able to scale in the following areas:


## Client-Side (Angular Application) Scalability

    Lazy Loading Modules: Implement lazy loading for feature modules to improve initial load time.
    Pagination and Infinite Scroll: Use pagination or infinite scroll to load and display only a subset of blog posts at a time.
    Optimized Data Fetching: Implement delay for search inputs and avoid fetching data on every keypress.
    Caching and State Management: Use caching mechanisms and state management libraries like NgRx or Akita to reduce redundant API calls.

## Server-Side (API) Scalability

    Database Optimization: Use indexing, sharding, and partitioning to optimize database performance.
    Efficient Querying: Implement efficient querying strategies, such as using proper indexes and avoiding N+1 query problems.
    Caching: Use caching layers like Redis or Memcached to cache frequent read queries.
    Load Balancing: Implement load balancing to distribute incoming requests across multiple servers.
    Horizontal Scaling: Scale horizontally by adding more instances of the API server.
    Asynchronous Processing: Use message queues like RabbitMQ or Kafka for handling background tasks and asynchronous processing.
    Microservices Architecture: Break down the application into microservices to handle different functionalities independently.

## Database Sharding:

    NoSQL Databases: Use NoSQL databases like MongoDB or Cassandra for handling large volumes of unstructured data.
    Cloud Storage: Utilize cloud storage solutions like Amazon S3 or Azure Blob Storage for storing large files.
    Monitoring: Use monitoring tools like Prometheus, Grafana, or New Relic to monitor the application’s performance and health.
    Regular Maintenance: Perform regular maintenance tasks such as database optimization, code refactoring, and dependency updates.
   
   
   
   

   
