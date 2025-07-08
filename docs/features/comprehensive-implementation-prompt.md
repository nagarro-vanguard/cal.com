# Codebase-First Implementation Plan: Event Type Templates & Marketplace

## Purpose

You are an expert architectural detective analyzing the Cal.com codebase to design a perfectly integrated Event Type Templates & Marketplace feature. Your mission is to discover existing patterns, understand the architectural DNA, and create an implementation plan that feels like it was always part of the system.

## Analysis Strategy

**DEEP DIVE MINDSET**: Spend significant time exploring the codebase to understand:
- How Cal.com's architecture "thinks" about features
- What patterns repeat across similar functionality
- Where natural integration points exist
- How existing systems solve similar problems

**PATTERN DISCOVERY FOCUS**:
- Event Type lifecycle and management patterns
- PBAC permission system integration strategies
- Team/Organization hierarchy and context propagation
- tRPC API design philosophy and conventions
- UI component composition and reusability patterns
- Database schema evolution and relationship modeling (Prisma)
- Feature flags and gradual rollout strategies

**PRINCIPLE**: Every architectural decision must feel inevitable given the existing codebase

## Requirements Reference

Refer to the comprehensive requirements document at `/docs/features/event-type-templates-marketplace.md` for:

- User personas and stories
- Functional requirements (Template Creation, Discovery, Application, Management)
- Core categories and UI requirements
- Success criteria and constraints

## Phase-Based Discovery & Planning

### Phase 1: Architectural Archaeology

**1. Event Type System Deep Dive**:

- Study existing event type creation flows (`/apps/web/pages/event-types/`)
- Analyze data models and relationships in Prisma schema
- Understand permission integration with current event types
- Document state management patterns used in event type flows

**2. Pattern Mining Expedition**:

- Examine similar "template-like" functionality in the codebase
- Identify reusable UI components for creation and management flows
- Study tRPC router organization and naming conventions
- Analyze how team/org context propagates through the system

**3. Integration Point Discovery**:

- Map where templates would naturally fit in existing navigation
- Understand current onboarding and quickstart patterns
- Study existing marketplace-like features (app store integration)
- Identify touchpoints with permissions, billing, and feature flags

**DELIVERABLE**: A comprehensive "Codebase Pattern Analysis" section documenting discovered patterns and their implications for templates implementation.

### Phase 2: Bite-Sized Architectural Blueprint (For Review)

Present a lean, reviewable structure covering only WHAT needs to be built:

**Database Evolution**:

- New models required and their relationships to existing schema
- Migration strategy that respects existing data integrity
- Indexing approach for performance

**API Surface Design**:

- tRPC router additions following discovered conventions
- Integration points with existing event type APIs
- Permission enforcement strategy

**UI Integration Strategy**:

- Component hierarchy leveraging existing design system
- Navigation and discovery flow integration points
- State management approach consistent with existing patterns

**Permission Architecture**:

- PBAC integration approach respecting existing boundaries
- Team/organization context inheritance strategy

### 🛑 STOP HERE FOR ARCHITECTURAL REVIEW 🛑

### Phase 3: Detailed Implementation Architecture (Post-Review)

For each approved layer, provide surgical architectural decisions:

**Database Implementation Strategy**:

- Precise Prisma model definitions following naming patterns
- Relationship modeling aligned with existing schema philosophy
- Migration sequencing that maintains system stability

**API Architecture Details**:

- tRPC router structure mirroring existing organizational patterns
- Input/output schema design following Cal.com conventions
- Error handling strategy consistent with current approach
- Caching integration with existing performance patterns

**UI Component Architecture**:

- Component composition strategy leveraging existing design tokens
- Integration with current navigation and layout systems
- Form handling and validation patterns matching existing flows
- Loading states and error boundaries following established patterns

**Permission System Integration**:

- Granular permission checks at each system layer
- Context propagation through component hierarchy
- API-level authorization following existing patterns
- UI-level permission-based rendering strategies

**Performance & Scalability Approach**:

- Database query optimization aligned with existing patterns
- Caching strategy integrated with current performance architecture
- Bundle optimization maintaining existing loading performance

## Critical Success Principles

**Pattern Preservation**: Every solution must feel like a natural extension of existing Cal.com architecture

**Zero Disruption**: Implementation cannot break or degrade existing functionality

**Seamless Integration**: Templates should feel native, not bolted-on

**Permission Respect**: Must work flawlessly within existing PBAC framework

**Performance Maintenance**: No regression in existing system performance

## Expected Deliverable

A comprehensive markdown document: "Event Type Templates Implementation Plan" containing:

1. **Codebase Pattern Analysis**: Deep documentation of discovered patterns and integration points
2. **High-Level Architecture Blueprint**: Reviewable structure focused on WHAT and WHY
3. **Detailed Implementation Strategy**: Surgical approach for each system layer post-review
4. **Integration Roadmap**: Bite-sized implementation phases with clear success criteria
5. **Risk Mitigation**: Identified potential conflicts and mitigation strategies

**Focus**: Architectural planning that enables confident implementation without requiring code-level specifics during review phase.
