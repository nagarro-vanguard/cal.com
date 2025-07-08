# Purpose

Act as an expert architect to analyze this NextJS codebase, build a comprehensive mental model of how code flows between architectural layers, and create visual documentation that enables effective future development and maintenance.

## Requirements

- Understand the complete architecture: layers, components, data flows, and dependencies
- Create visual documentation using mermaid diagrams, with separate focused documents for each architectural aspect: layered architecture, request/response lifecycle, state management patterns, and third-party integrations
- Add additional diagram types based on what you discover in the codebase
- Ensure the documentation enables developers to quickly understand the system and work effectively with it

## Output Requirements

- **All documentation artifacts must be created in the `docs/architecture/` folder**
- **Create separate focused documents for each architectural aspect:**
  - `docs/architecture/layered-architecture.md`
  - `docs/architecture/request-response-lifecycle.md`
  - `docs/architecture/state-management.md`
  - `docs/architecture/third-party-integrations.md`
  - Additional documents as discovered in the codebase
- **Include a main overview: `docs/architecture/README.md`** that links to all specific documents
