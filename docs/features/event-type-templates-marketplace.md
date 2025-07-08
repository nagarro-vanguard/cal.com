# Event Type Templates & Marketplace

**Feature Version**: v1.0 (Core MVP)
**Implementation Date**: July 2025
**Implementation Approach**: Prompt-First Development

## Executive Summary

The Event Type Templates & Marketplace feature allows users to create, share, and discover pre-configured event type templates, reducing setup time and promoting best practices across the Cal.com ecosystem.

## Problem Statement

**Current Pain Points:**

- Users spend significant time recreating similar event types
- New users struggle with optimal event type configuration
- Teams lack consistency in event type setup across members
- No way to share proven event type configurations
- Repeated manual configuration leads to inconsistencies and errors

**Success Metrics:**

- 40% reduction in time to create new event types
- 60% of new event types created from templates
- 80% user satisfaction with template discovery
- 25% increase in event type feature adoption

## Core User Personas

### 1. **Template Creator** (Sarah - Marketing Manager)

- **Profile**: Experienced Cal.com user, creates many similar events
- **Goals**: Share successful event configurations, establish team standards
- **Frustrations**: Recreating similar event types, inconsistent team setups
- **Use Cases**: Create consultation templates, workshop templates, interview templates

### 2. **Template Consumer** (Mike - Sales Rep)

- **Profile**: New to Cal.com, wants quick setup
- **Goals**: Get started quickly with proven configurations
- **Frustrations**: Complex setup process, not knowing best practices
- **Use Cases**: Find sales call templates, demo templates, follow-up templates

### 3. **Team Lead** (Jennifer - Head of Operations)

- **Profile**: Manages team consistency and best practices
- **Goals**: Ensure team uses consistent, optimized event types
- **Frustrations**: Team members using different configurations
- **Use Cases**: Create organization templates, enforce standards

## Core Requirements (MVP)

### 1. Template Creation

**User Story**: As a template creator, I want to convert my existing event types into reusable templates so that others can benefit from my configurations.

**Acceptance Criteria:**

- Convert any existing event type to a template with one click
- Add template metadata (title, description, category, tags)
- Set template visibility (private, team, organization, public)
- Preview template before publishing
- Edit template details after creation

**Technical Requirements:**

- Template model with event type configuration JSON
- Template metadata storage
- Permission-based visibility controls
- Template versioning (future consideration)

### 2. Template Discovery

**User Story**: As a template consumer, I want to browse and search templates so that I can find configurations that match my needs.

**Acceptance Criteria:**

- Browse templates by category (sales, marketing, support, interviews, etc.)
- Search templates by title, description, and tags
- Filter by template source (public, organization, team)
- View template details and preview configuration
- See template usage statistics and ratings

**Technical Requirements:**

- Template search and filtering API
- Category taxonomy
- Usage analytics tracking
- Template preview component

### 3. Template Application

**User Story**: As a template consumer, I want to create event types from templates so that I can quickly set up optimized configurations.

**Acceptance Criteria:**

- Create new event type from template with one click
- Customize template values during creation (title, duration, etc.)
- Preview final event type before saving
- Option to save customizations back to personal templates
- Clear indication that event type was created from template

**Technical Requirements:**

- Template-to-event-type conversion logic
- Customization interface
- Template attribution tracking
- Event type creation integration

### 4. Template Management

**User Story**: As a template creator, I want to manage my templates so that I can keep them updated and relevant.

**Acceptance Criteria:**

- View all my created templates
- Edit template metadata and configuration
- Archive/delete templates
- View template usage analytics
- Manage template permissions

**Technical Requirements:**

- Template CRUD operations
- Analytics dashboard
- Permission management
- Template lifecycle management

## Core Categories (MVP)

1. **Sales & Business Development**
   - Discovery calls
   - Demo sessions
   - Proposal presentations
   - Follow-up meetings

2. **Recruiting & Interviews**
   - Phone screenings
   - Technical interviews
   - Panel interviews
   - Reference calls

3. **Customer Support**
   - Support consultations
   - Onboarding sessions
   - Training calls
   - Escalation meetings

4. **Consultations & Services**
   - Initial consultations
   - Strategy sessions
   - Coaching calls
   - Service delivery

## User Interface Requirements

### Template Gallery

- **Layout**: Grid view with template cards
- **Template Card**: Title, description, category, creator, usage count, rating
- **Filters**: Category, source (public/org/team), popularity, recent
- **Search**: Real-time search with autocomplete
- **Navigation**: Breadcrumbs, pagination

### Template Creation Flow

1. **Source Selection**: "Create from existing event type" or "Create from scratch"
2. **Configuration**: Copy event type settings, add metadata
3. **Details**: Title, description, category, tags, visibility
4. **Preview**: Show how template will appear to consumers
5. **Publish**: Confirm and make available

### Template Application Flow

1. **Preview**: Show template configuration details
2. **Customize**: Allow editing of key fields (title, duration, description)
3. **Review**: Preview final event type configuration
4. **Create**: Generate event type and redirect to event type page

## Technical Architecture

### Database Schema

```sql
-- Template model
CREATE TABLE EventTypeTemplate (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    category VARCHAR(100),
    tags JSON,
    configuration JSON NOT NULL, -- EventType configuration
    creatorId INTEGER REFERENCES User(id),
    organizationId INTEGER REFERENCES Team(id), -- NULL for public
    teamId INTEGER REFERENCES Team(id), -- NULL for non-team templates
    visibility ENUM('private', 'team', 'organization', 'public') DEFAULT 'private',
    usageCount INTEGER DEFAULT 0,
    rating DECIMAL(3,2) DEFAULT 0,
    createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Template usage tracking
CREATE TABLE TemplateUsage (
    id SERIAL PRIMARY KEY,
    templateId INTEGER REFERENCES EventTypeTemplate(id),
    userId INTEGER REFERENCES User(id),
    eventTypeId INTEGER REFERENCES EventType(id),
    createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Template ratings (future)
CREATE TABLE TemplateRating (
    id SERIAL PRIMARY KEY,
    templateId INTEGER REFERENCES EventTypeTemplate(id),
    userId INTEGER REFERENCES User(id),
    rating INTEGER CHECK (rating >= 1 AND rating <= 5),
    review TEXT,
    createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### API Endpoints

```typescript
// Template CRUD
GET    /api/v2/templates                    // List templates with filters
POST   /api/v2/templates                    // Create template
GET    /api/v2/templates/:id                // Get template details
PUT    /api/v2/templates/:id                // Update template
DELETE /api/v2/templates/:id                // Delete template

// Template application
POST   /api/v2/templates/:id/apply          // Create event type from template
POST   /api/v2/event-types/:id/to-template  // Convert event type to template

// Analytics
GET    /api/v2/templates/:id/analytics      // Template usage analytics
GET    /api/v2/users/me/templates           // My templates dashboard
```

### Component Architecture

```typescript
// Core components
- TemplateGallery: Main discovery interface
- TemplateCard: Individual template display
- TemplateCreationWizard: Multi-step template creation
- TemplateApplicationModal: Apply template to new event type
- TemplatePreview: Show template configuration
- TemplateManagementDashboard: Creator's template management

// Integration points
- EventTypeForm: Add "Create from template" option
- EventTypeList: Add "Save as template" action
- Navigation: Add "Templates" section
```

## Implementation Phases

### Phase 1: Core Infrastructure (Week 1-2)

- Database schema implementation
- Basic CRUD API endpoints
- Template model and validation
- Permission system integration

### Phase 2: Template Creation (Week 2-3)

- Convert event type to template functionality
- Template creation wizard UI
- Template preview component
- Basic template management

### Phase 3: Template Discovery (Week 3-4)

- Template gallery interface
- Search and filtering functionality
- Category taxonomy implementation
- Template card components

### Phase 4: Template Application (Week 4-5)

- Apply template to create event type
- Customization interface during application
- Integration with existing event type creation flow
- Usage tracking implementation

## Prompt-First Implementation Strategy

### Development Approach

1. **Prompt Engineering**: Use AI assistance to generate initial code structure
2. **Iterative Refinement**: Refine prompts based on code quality and requirements
3. **Demo Recording**: Document the prompt-to-code process for each component
4. **Best Practices**: Establish patterns for prompt-driven development

### Prompt Documentation

Each major component will include:

- **Initial Prompt**: The AI prompt used to generate the component
- **Refinement Prompts**: Follow-up prompts for improvements
- **Generated Code**: The resulting code with minimal manual edits
- **Demo Video**: Screen recording of the prompt-to-implementation process

### Demo Structure

1. **Requirements Analysis**: Show how requirements translate to prompts
2. **Database Schema**: Prompt for schema design and migration
3. **API Development**: Prompt for endpoint creation and validation
4. **Frontend Components**: Prompt for React component development
5. **Integration**: Prompt for connecting frontend and backend
6. **Testing**: Prompt for test case generation

## Success Criteria

### Functional Requirements

- [ ] Users can create templates from existing event types
- [ ] Users can discover templates through search and categories
- [ ] Users can apply templates to create new event types
- [ ] Templates respect permission boundaries (team/org/public)
- [ ] Template usage is tracked and displayed

### Technical Requirements

- [ ] All APIs follow existing Cal.com patterns
- [ ] Integration with existing PBAC system
- [ ] Performance: Template gallery loads < 2 seconds
- [ ] Mobile-responsive template interfaces
- [ ] Comprehensive error handling and validation

### User Experience Requirements

- [ ] Template creation takes < 3 minutes
- [ ] Template discovery is intuitive and fast
- [ ] Template application reduces event type creation time by 60%
- [ ] Clear visual feedback throughout all flows
- [ ] Consistent with Cal.com design system

## Future Enhancements (Out of Scope for MVP)

- Template versioning and update notifications
- Community ratings and reviews
- Template recommendations based on usage patterns
- Premium template marketplace with monetization
- Template analytics and A/B testing
- Bulk template operations
- Template import/export functionality
- Integration with external template sources

## Constraints and Assumptions

### Technical Constraints

- Must integrate with existing PBAC permission system
- Must support existing event type configuration options
- Must maintain backward compatibility with current event types
- Must follow existing API versioning strategy

### Business Constraints

- MVP timeline: 5 weeks maximum
- No monetization features in initial release
- Public templates require manual approval (future)
- Must support all existing Cal.com deployment modes

### Assumptions

- Users want to share successful event type configurations
- Template discovery will drive feature adoption
- Team consistency is a valuable use case
- Simple categorization is sufficient for MVP
- Usage analytics provide sufficient creator feedback

---

**Next Steps:**

1. Technical design review with engineering team
2. UI/UX mockups for key flows
3. Prompt engineering strategy session
4. Demo recording setup and timeline
5. Implementation sprint planning
