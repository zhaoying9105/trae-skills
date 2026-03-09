# Module Classification Guidelines

Classify each module into one of these categories for consistent documentation:

## Core Modules
- **Business Domain Core:** Central business logic, domain models, and core rules
- **Shared Utilities:** Common helper functions, constants, and utilities used across the codebase
- **Foundation Components:** Base UI components, design system elements, and shared UI primitives
- **Infrastructure Abstractions:** Wrappers around databases, external APIs, and third-party services

## Feature Modules
- **User-Facing Features:** End-to-end feature implementations visible to end users
- **Admin Features:** Backoffice and administrative functionality
- **Integration Features:** Modules that connect different systems or services
- **Experimental Features:** Work-in-progress or beta functionality

## Infrastructure Modules
- **API Layer:** REST, GraphQL, or RPC API endpoints and handlers
- **Data Access:** Database repositories, ORM models, and data persistence logic
- **Authentication & Authorization:** Security, access control, and identity management
- **Background Processing:** Queues, workers, cron jobs, and async processing
- **Deployment & Configuration:** Infrastructure as code, environment configs, and deployment scripts

## Documentation Modules
- **Public API:** Externally facing interfaces, SDKs, and developer documentation
- **Internal Tools:** Engineering tools, debug utilities, and internal dashboards
- **Tests:** Test utilities, fixtures, and shared testing infrastructure

## Module Complexity Rating
Rate each module by complexity for prioritization:
- **Low:** < 10 files, well-defined purpose, few dependencies
- **Medium:** 10-50 files, moderate complexity, clear boundaries
- **High:** > 50 files, complex logic, many dependencies, or critical path
