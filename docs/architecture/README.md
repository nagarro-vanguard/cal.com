# Cal.com Architecture Documentation

This directory contains comprehensive architectural documentation for the Cal.com platform, providing visual and textual guides to help developers understand and work with the system effectively.

## Architecture Overview

Cal.com is built as a modern, scalable monorepo application using Next.js, TypeScript, and a microservice-oriented package architecture. The system supports multi-tenancy, extensive third-party integrations, and a plugin-based app store model.

## Documentation Structure

### Core Architecture Documents

- **[Layered Architecture](./layered-architecture.md)** - Overview of the system's modular layers, from presentation to data access
- **[Request/Response Lifecycle](./request-response-lifecycle.md)** - How requests flow through the system, including API routes and tRPC
- **[State Management](./state-management.md)** - Client and server state management patterns and tools
- **[Third-Party Integrations](./third-party-integrations.md)** - OAuth, calendar, conferencing, and other external service integrations

### Additional Architecture Documents

- **[Plugin System Architecture](./plugin-system.md)** - App store, dynamic routing forms, and extensibility patterns
- **[Multi-Tenancy & RBAC](./multi-tenancy-rbac.md)** - Organizations, teams, and permission-based access control
- **[Data Flow & Event System](./data-flow-events.md)** - Booking workflows, event processing, and data synchronization

## Key Architectural Principles

### 1. Modular Monorepo Structure
- **Apps**: Main applications (web, API v1/v2, ui-playground)
- **Packages**: Shared libraries (features, UI components, utilities)
- **Feature-based organization**: Related functionality grouped together

### 2. Type-Safe API Layer
- **tRPC**: End-to-end type safety for API calls
- **Prisma**: Type-safe database access
- **Zod**: Runtime schema validation

### 3. Multi-Tenancy & Scalability
- **Organizations & Teams**: Hierarchical structure
- **RBAC**: Permission-based access control
- **Database isolation**: Logical separation of tenant data

### 4. Extensibility
- **App Store**: Plugin-based architecture for integrations
- **Dynamic Forms**: Runtime form generation and routing
- **Custom components**: Overridable UI components

## Technology Stack

### Frontend
- **Next.js 14**: App Router with SSR/SSG
- **React 18**: Component-based UI
- **Tailwind CSS**: Utility-first styling
- **Headless UI**: Accessible components

### Backend
- **tRPC**: Type-safe API layer
- **Prisma**: Database ORM
- **PostgreSQL**: Primary database
- **Node.js**: Runtime environment

### State Management
- **React Context**: Component-level state
- **Zustand**: Global state management
- **Jotai**: Atomic state management
- **React Hook Form**: Form state management

### Integrations
- **OAuth 2.0**: Authentication providers
- **Calendar APIs**: Google, Outlook, CalDAV
- **Video Conferencing**: Zoom, Google Meet, MS Teams
- **Payment Processing**: Stripe, PayPal
- **Email Services**: SendGrid, Postmark

## Getting Started

1. **For New Developers**: Start with [Layered Architecture](./layered-architecture.md) to understand the overall system structure
2. **For API Development**: Review [Request/Response Lifecycle](./request-response-lifecycle.md) and [State Management](./state-management.md)
3. **For Integration Work**: Check [Third-Party Integrations](./third-party-integrations.md) and [Plugin System Architecture](./plugin-system.md)
4. **For Platform Features**: Read [Multi-Tenancy & RBAC](./multi-tenancy-rbac.md) and [Data Flow & Event System](./data-flow-events.md)

## Conventions & Standards

- **File Naming**: kebab-case for files, PascalCase for components
- **Import Organization**: External → Internal → Relative imports
- **Type Definitions**: Co-located with implementation or in dedicated `types/` directories
- **Error Handling**: Consistent error boundaries and validation
- **Testing**: Unit tests, integration tests, and e2e tests with Playwright

## Contributing

When modifying the architecture:
1. Update relevant documentation in this directory
2. Ensure changes align with existing patterns
3. Consider impact on multi-tenancy and extensibility
4. Add appropriate tests and type definitions

For detailed implementation guides, refer to the specific architecture documents linked above.
