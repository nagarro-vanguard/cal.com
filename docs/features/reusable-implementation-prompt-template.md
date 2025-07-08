# Implement New Feature: {{FEATURE_NAME}}

## Purpose

You are an expert architect managing the design and implementation of the Cal.com NextJS codebase, creating a mental model on how the code flows between multiple layers and charting out implementation architecture.

## Configuration

- **THIRD_PARTY_LIBS_TO_INSTALL**: {{LIBS_IF_NEEDED}}
- **PACKAGE_MANAGER**: `yarn`
- **CODEBASE**: Cal.com (NextJS scheduling platform)

## Requirements

{{FEATURE_SPECIFIC_REQUIREMENTS}}

## Instructions

- Navigate through the Cal.com codebase and understand all architectural layers involved in this codebase
- Take a pause, and build a mental model on the code flows and data flows between layers, especially:
  - tRPC API patterns and request/response flows
  - Prisma schema relationships and data persistence layers
  - PBAC permission system integration and context propagation
  - Team/Organization hierarchy and context inheritance
  - UI component composition and state management patterns
  - Event type management and configuration patterns
  - Feature flag and billing integration points
- Your mental model is critical for implementing features that feel native to Cal.com's architecture
- Once you have accurately understood the code flows and data flows deeply:
  - Analyze the `Requirements` and `Configuration` section deeply
  - Build an enterprise-grade, step-by-step implementation plan on how you'd approach implementing the `Requirements`
  - Output your plan to a **markdown** file. Use the level-1 heading as the file name
- Present your plan to user for final review

## Cal.com Specific Guidelines

When implementing new features that modify data and require system integration:

- **Permission Integration**: Ensure PBAC compliance at all layers (API, UI, database)
- **Multi-Layer Synchronization**: Consider how changes affect local state, context state, and server state
- **Performance Maintenance**: Maintain existing caching and query patterns, no performance degradation
- **UI Consistency**: Follow existing design system, component patterns, and responsive design
- **Database Evolution**: Respect existing Prisma schema conventions, relationship patterns, and migration strategies
- **API Design**: Follow established tRPC router patterns, input/output schemas, and error handling
- **State Management**: Ensure immediate UI updates and consistency across component hierarchy
- **Testing Strategy**:
  - **Unit Tests**: Vitest for business logic, utilities, and pure functions
  - **E2E Tests**: Playwright for critical user journeys and cross-feature integration
  - **Component Tests**: Vitest with jsdom for UI interactions and component behavior
  - **API Tests**: Jest for v2 API endpoints (apps/api/v2) and tRPC endpoint testing
  - **Permission Tests**: E2E tests for PBAC boundary testing and authorization flows
  - **Integration Tests**: Vitest for database operations and service integration
- **Error Handling**: Implement robust error boundaries with proper user feedback and recovery
- **Internationalization**: Consider i18n requirements for user-facing text and cultural considerations
- **Accessibility**: Follow existing a11y patterns, WCAG compliance, and keyboard navigation
- **Feature Flags**: Integrate with existing feature flag system for gradual rollouts
- **Team/Organization Context**: Ensure proper context propagation and hierarchy respect
- **Billing Integration**: Consider impact on existing billing and subscription features
- **Mobile Responsiveness**: Ensure seamless experience across all device sizes
- **Router Coordination**: Coordinate with Next.js router for server-side data consistency

The goal is to provide users with a seamless, responsive experience where actions trigger immediate visual feedback while maintaining data consistency, performance, and security across the entire Cal.com platform.

## Testing Coverage Requirements

Ensure comprehensive testing across all architectural layers using Cal.com's existing testing infrastructure:

- **Database Layer**: Vitest for migration scripts, model relationships, constraint validation
- **API Layer**:
  - Jest for v2 API endpoints (apps/api/v2) - input validation, business logic, error scenarios
  - Vitest for tRPC endpoints - permission enforcement, request/response validation
- **UI Layer**:
  - Vitest with jsdom for component rendering, form submissions, state management
  - Playwright E2E for complete user workflows and booking flows
- **Integration Layer**:
  - Playwright E2E for cross-feature workflows, team/organization context, permission boundaries
  - Vitest for service integration and database query testing
- **Performance Layer**: Manual testing for query optimization, caching effectiveness, bundle size impact
- **Security Layer**: Playwright E2E for permission bypass attempts, data leakage prevention

Each test should validate both the happy path and edge cases, ensuring robust implementation that maintains Cal.com's reliability standards.
