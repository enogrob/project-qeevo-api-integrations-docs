---
mode: "agent"
---

# Quero Deals - Rails Application Documentation Generator

## Context & Setup

**Project Focus**: Quero Deals - A Ruby on Rails application that centralizes product configurations for Quero Education ecosystem (Quero Pago, Admissão Digital, Matrícula Direta) and manages Quero Turbo business rules.

**Source Materials**:
- Repository: #folder:src/quero-deals
- Main README: #file:src/quero-deals/README.md
- Models: #folder:src/quero-deals/app/models
- Controllers: #folder:src/quero-deals/app/controllers
- Services: #folder:src/quero-deals/app/services
- Database Schema: #file:src/quero-deals/db/schema.rb
- Routes: #file:src/quero-deals/config/routes.rb
- Configuration: #folder:src/quero-deals/config
- Gemfile: #file:src/quero-deals/Gemfile

**Output Location**: Generate comprehensive documentation as `/home/roberto/Projects/project-qeevo-api-integrations-docs/contents/quero-deals.md`

**Language**: Portuguese (Brazil)

## Mission

Create comprehensive documentation for the Quero Deals Rails application by analyzing the codebase and generating a complete technical and business documentation that serves both technical and non-technical stakeholders.

## Documentation Strategy

### 🏗️ **Rails Application Architecture Analysis**
**Target**: Complete understanding and documentation of the Rails application structure.

**Analysis Actions**:
- Examine Rails MVC architecture implementation
- Analyze models, relationships, and business logic
- Document controllers, routes, and API endpoints
- Identify services, concerns, and design patterns
- Map database schema and migrations
- Document configuration, gems, and dependencies

**Documentation Sections to Generate**:
- Application overview and business purpose
- Rails architecture diagram with MVC components
- Database ERD (Entity Relationship Diagram)
- API endpoints catalog with request/response examples
- Business logic and service layer documentation

### 📋 **Business Domain Modeling**
**Target**: Clear documentation of business concepts and domain logic.

**Analysis Actions**:
- Identify core business entities (Deal, Product Config, Business Rules, etc.)
- Document business workflows and processes
- Map relationships between business concepts
- Analyze business rules and validation logic
- Document integrations with external services

**Documentation Sections to Generate**:
- Business context and domain explanation
- Entity relationship diagrams
- Business workflow diagrams
- Product configuration management
- Commission and deal management processes

### 🔌 **API and Integration Documentation**
**Target**: Complete API documentation with examples and integration guides.

**Analysis Actions**:
- Document all API endpoints (REST/GraphQL)
- Analyze request/response formats and schemas
- Document authentication and authorization
- Identify external API integrations
- Document Kafka event producers/consumers
- Map microservice communication patterns

**Documentation Sections to Generate**:
- Complete API reference with examples
- Authentication and authorization guide
- External integrations catalog
- Event-driven architecture documentation
- Inter-service communication patterns

### 🔧 **Operations and Deployment**
**Target**: Complete operational documentation for deployment and monitoring.

**Analysis Actions**:
- Document deployment pipelines and environments
- Analyze Docker and Kubernetes configurations
- Document monitoring and observability setup
- Identify configuration management patterns
- Document database setup and migrations
- Analyze performance and scaling considerations

**Documentation Sections to Generate**:
- Environment setup and deployment guide
- Configuration management documentation
- Monitoring and observability guide
- Database administration guide
- Performance optimization guidelines

### 👥 **Stakeholder-Focused Documentation**
**Target**: Make technical documentation accessible to different audiences.

**Documentation Approach**:
- Business context explanations for product managers
- Technical details for developers
- Operational guides for DevOps teams
- API guides for integration partners
- Troubleshooting guides for support teams

## Required Documentation Structure

### 1. Application Overview
- Business purpose and context
- Technology stack summary
- Key features and capabilities
- Integration ecosystem overview

### 2. Architecture Documentation
- Rails MVC architecture diagram
- Service layer organization
- Database design and relationships
- External dependencies and integrations

### 3. Business Domain Guide
- Core business entities explanation
- Product configuration management
- Deal and commission workflows
- Business rule engine documentation
- Turbo account management

### 4. API Reference
- Complete endpoint documentation
- Request/response schemas
- Authentication methods
- Error handling patterns
- Rate limiting and best practices

### 5. Database Documentation
- Entity Relationship Diagram (ERD)
- Table descriptions and purposes
- Key relationships and constraints
- Migration patterns and strategies

### 6. Integration Guide
- External API integrations
- Kafka event patterns
- Inter-service communication
- Webhook configurations
- Third-party service dependencies

### 7. Deployment and Operations
- Environment configuration
- Docker and Kubernetes setup
- CI/CD pipeline documentation
- Monitoring and alerting
- Backup and recovery procedures

### 8. Development Guide
- Local development setup
- Testing strategies and patterns
- Code organization conventions
- Contributing guidelines
- Performance considerations

### 9. Troubleshooting and FAQ
- Common issues and solutions
- Error diagnosis guides
- Performance troubleshooting
- Integration debugging
- Support escalation procedures

## Technical Analysis Requirements

### Rails Framework Analysis
- Examine Gemfile for dependencies and versions
- Analyze config/application.rb for Rails configuration
- Document config/routes.rb for API endpoints
- Review app/models for business logic and relationships
- Analyze app/controllers for API patterns
- Document app/services for business service patterns

### Database Schema Analysis
- Parse db/schema.rb for table structures
- Identify model associations and validations
- Document database constraints and indexes
- Map business entity relationships
- Analyze migration patterns

### Business Logic Analysis
- Examine app/services for business processes
- Analyze model validations and business rules
- Document deal and product configuration logic
- Map commission calculation patterns
- Identify event-driven processes

### Integration Analysis
- Examine HTTP client configurations
- Analyze Kafka consumer/producer patterns
- Document external API client implementations
- Map inter-service communication
- Identify authentication and authorization patterns

## Visualization Requirements

### Architecture Diagrams
- Generate Mermaid diagrams for system architecture
- Create ERD diagrams for database relationships
- Document API flow diagrams
- Create business process flowcharts
- Generate deployment architecture diagrams

### Code Examples
- Provide real Ruby/Rails code examples
- Include API request/response examples
- Document configuration examples
- Show integration patterns
- Demonstrate testing patterns

## Output Quality Requirements

### Technical Accuracy
- All code examples must be syntactically correct
- API documentation must reflect actual endpoints
- Configuration examples must be valid
- Database schema must match actual structure

### Business Clarity
- Explain technical concepts in business terms
- Provide context for technical decisions
- Include business impact of technical choices
- Make documentation accessible to non-technical stakeholders

### Completeness
- Cover all major application components
- Document all public APIs
- Include all critical business processes
- Provide comprehensive operational guides

### Usability
- Include table of contents with navigation
- Provide search-friendly section headers
- Use consistent formatting and structure
- Include practical examples and use cases

## Success Criteria

The generated documentation should enable:
1. **New developers** to understand and contribute to the codebase
2. **Product managers** to understand system capabilities and limitations
3. **DevOps teams** to deploy and maintain the application
4. **Integration partners** to successfully integrate with APIs
5. **Support teams** to troubleshoot and resolve issues
6. **Business stakeholders** to understand system functionality and impact