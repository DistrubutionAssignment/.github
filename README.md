# .github
READMEHANS


Cloud Services and Distributed Systems – MVP Event System

This is a school project and a Minimum Viable Product (MVP) for a distributed event system built with a microservices architecture in ASP.NET Core and a React-based frontend.
https://ashy-cliff-0942cde03.6.azurestaticapps.net/

DISCLAIMER:
Some parts generated with ChatGPT o4. mini-high
noticed after handing in this assignment that "Quantity" was not removed from Events SQL-table. this was supposed to refactories to TicketAmount, but seems to have slipped past.
Placeholder images: https://picsum.photos/

Overview

Purpose: Implement a basic event system where:

The public can view all events.

Registered users can book events and view their own bookings.

Administrators have full control over bookings and events.

Architecture:

Four independent backend microservices (REST + Service Bus) in ASP.NET Core:

AuthService – Handles user registration, login, JSON Web Tokens (JWT), and roles.

EmailVerificationService – Listens to a Service Bus queue to send verification emails.

EventService – Provides CRUD operations for events.

BookingService – Manages bookings with JWT-based access control.

React frontend hosted as an Azure Static Web App.

Configuration and secrets stored in Azure Key Vault.

CI/CD pipelines in GitHub Actions deploy to Azure.

Functionality (MVP)

Public Events

GET /events – No authentication required.

User Workflow

Login and registration via AuthService.

Book events with a valid JWT.

GET /bookings/{userId} – Accessible only to authenticated users.

Admin Workflow

Role-based access via JWT claim ("Admin").

GET /bookings – View all bookings.

Full CRUD for both /events and /bookings via Swagger UI.

Email Verification

A verification email is sent upon user registration (optional and does not block other functionality).

Known Issues

Token Refresh in BookingService: After refreshing the token, the frontend loses the JWT for /bookings calls. Workaround: navigate away from the error message and log in again.

Admin Claim Displayed as "User": The frontend shows the admin role as "User" despite valid JWT and policy validation. The backend still enforces admin functionality correctly.

Design Choices and Patterns

Dependency Injection (DI): Utilizes ASP.NET Core’s built-in DI container.

Factory Pattern: Used for DTO-to-model mapping.

Event-Driven Communication: Service Bus (publish-subscribe) between AuthService and EmailVerificationService.

SOLID Principles: Clear separation of concerns between controllers, services, data context, and DTOs.

Note: The repository pattern was omitted due to the small scope of the projects. It could be introduced in AuthService for additional abstraction.

Deployment

Create Azure resources:

Key Vault for secrets (DefaultConnection, Jwt:Key, etc.).

Service Bus queue.

App Services for each backend service.

Azure Static Web App for the frontend.

Configure GitHub Secrets for each repository (e.g., AZURE_CREDENTIALS, VAULT_URI, SWAP_TOKEN).

Push to the main branch to trigger the CI/CD pipeline and deployment.
