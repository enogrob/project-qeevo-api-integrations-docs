---
mode: "agent"
---

# Kroton Lead Integration - Acceptance Criteria Validator

## Context & Setup

**Project Focus**: Kroton Lead Integration - A Node.js/TypeScript service for integrating student enrollment with Kroton University's API.

**Source Materials**:
- Repository: #folder:src/kroton-lead-integration
- Documentation: #folder:src/kroton-lead-integration/__docs__
- Repository URL: From #file:resources/quero-edu:kroton-lead-integration.webloc
- Main README: #file:src/kroton-lead-integration/README.md

**Output Location**: Generate validation report as `/home/roberto/Projects/project-qeevo-api-integrations-docs/contents/kroton-lead-integration.md`

**Language**: Portuguese (Brazil)

## Mission

Enhance the Kroton Lead Integration documentation by analyzing the codebase and directly improving the existing documentation to meet all acceptance criteria. Focus on making actionable improvements rather than just identifying gaps.

## Documentation Enhancement Strategy

### 🔍 **AC1: Complete API Integration Catalog**
**Target**: Ensure 100% of current API integrations are documented in the main documentation.

**Enhancement Actions**:
- Analyze the codebase to identify all API endpoints, authentication methods, and integration patterns
- Add missing API integrations to the documentation with complete specifications
- Enhance existing API documentation with missing details (parameters, responses, error codes)
- Add authentication patterns and rate limiting information where missing
- Include practical code examples for each integration

**Direct Improvements to Make**:
- Add new sections for undocumented APIs
- Enhance existing API sections with missing technical details
- Include authentication examples and configuration steps
- Add troubleshooting sections for common API issues

### 📋 **AC2: Event Patterns Documentation**
**Target**: All event patterns documented with complete payload schemas and real examples.

**Enhancement Actions**:
- Scan the codebase for all event types and their payload structures
- Add missing event documentation with complete schemas
- Include real-world payload examples for each event type
- Enhance existing event documentation with missing scenarios
- Add event flow diagrams where they don't exist

**Direct Improvements to Make**:
- Add new event type sections with complete payload schemas
- Include JSON examples for each event type
- Add event sequence diagrams using Mermaid
- Enhance error event documentation with recovery patterns

### 👥 **AC3: Business Rules Clarity**
**Target**: Make business rules understandable by non-technical stakeholders.

**Enhancement Actions**:
- Rewrite technical sections using business-friendly language
- Add business context and rationale for each integration rule
- Create decision trees and workflow diagrams for complex processes
- Add glossary of technical terms with business explanations
- Include stakeholder-focused troubleshooting guides

**Direct Improvements to Make**:
- Simplify technical jargon in existing sections
- Add "Business Context" subsections to technical areas
- Create visual workflow diagrams
- Add FAQ sections for common business questions
- Include step-by-step guides for non-technical users

### 🔧 **AC4: Integration Health & Monitoring**
**Target**: Complete operational documentation for system health tracking.

**Enhancement Actions**:
- Document logging patterns and monitoring setup
- Add health check endpoints and monitoring strategies
- Enhance status tracking documentation (follow_ups table)
- Include retry mechanisms and fallback strategies
- Add deployment and configuration guides

**Direct Improvements to Make**:
- Add monitoring and alerting sections
- Enhance status tracking documentation
- Include operational runbooks
- Add configuration management sections
- Create deployment checklists

## Enhancement Process

1. **Code Deep Dive**: Thoroughly analyze source code to understand actual implementation
2. **Documentation Gap Analysis**: Compare code reality with current documentation
3. **Direct Content Enhancement**: Make specific improvements to existing documentation
4. **New Content Creation**: Add missing sections based on code analysis
5. **Stakeholder Optimization**: Ensure content is accessible to business users

## Output Instructions

**Primary Action**: Directly enhance the existing documentation file at the specified output location by:

1. **Analyzing Current State**: Review existing documentation structure and content
2. **Code-Based Enhancements**: Add missing technical details found in codebase
3. **Content Improvements**: Rewrite unclear sections for better stakeholder understanding
4. **New Section Addition**: Add completely missing documentation areas
5. **Format Optimization**: Ensure consistent formatting and navigation

**Enhancement Priorities**:
1. **Critical Gaps**: Add missing API integrations and event patterns
2. **Clarity Improvements**: Simplify technical language for business users
3. **Operational Details**: Add monitoring, deployment, and troubleshooting content
4. **Examples & Diagrams**: Include practical examples and visual aids
5. **Navigation**: Improve table of contents and cross-references

**Success Measure**: The enhanced documentation should enable stakeholders to understand and work with the integrations without requiring engineering team support.