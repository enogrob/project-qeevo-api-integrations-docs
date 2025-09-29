---
mode: 'agent'
---

# Project Documentation Generator - Estácio Lead Integration

## Context & Setup

**Project Focus**: Estácio Lead Integration - A Node.js/TypeScript service for integrating student enrollment with Estácio University's API.

**Source Materials**:
- Repository: #folder:src/estacio-lead-integration 
- Documentation: #folder:src/estacio-lead-integration/__docs__
- Repository URL: From #file:resources/quero-edu:estacio-lead-integration.webloc
- Main README: #file:src/estacio-lead-integration/README.md

**Output Location**: Generate documentation as `/home/roberto/Projects/project-qeevo-api-integrations-docs/contents/estacio-lead-integration.md`

**Language**: Portuguese (Brazil)

## Pre-Analysis Steps

Before generating documentation:

1. **Code Analysis**: Examine the TypeScript source code structure, focusing on:
   - Main entry points and services
   - Database models and migrations  
   - API endpoint definitions
   - Authentication mechanisms
   - Error handling patterns

2. **Configuration Review**: Analyze:
   - Environment variables (.env.example)
   - Package.json dependencies and scripts
   - Docker configuration
   - Database configuration

3. **Documentation Review**: Read all available docs in __docs__ folder

4. **External Research**: If needed, fetch information from the GitHub repository URL

## Documentation Structure Requirements

### Formatting Rules
- Exactly one blank line after each section title
- Use proper markdown syntax throughout
- Include anchor links in table of contents
- Use collapsible sections for complex diagrams
- Wrap code in appropriate language blocks

### Required Sections

#### 1. Contents (Table of Contents)
Generate comprehensive TOC with anchor links:
```markdown
## Contents

- [Processo de Inscrição da Estácio](#processo-de-inscrição-da-estácio)
- [Arquitetura](#arquitetura)
- [Perspectivas Alternativas](#perspectivas-alternativas)
[... continue for all sections]
```

#### 2. Processo de Inscrição da Estácio

**Requirements**:
- 2-3 paragraphs explaining the integration's purpose and functionality
- Based on actual README.md and source code analysis
- Include key features: enrollment registration, LGPD sync, notification system
- Mention integration with Quero Educação ecosystem

#### 3. Arquitetura

**Mermaid Diagram Requirements**:
- Use get-syntax-docs-mermaid tool first for proper syntax
- Create flowchart showing main system components
- Include: API endpoints, database layer, external Estácio API, job processing
- Validate with mermaid-diagram-validator
- Preview with mermaid-diagram-preview
- Use pastel colors and emoticons for visual clarity
- Use subgraphs for logical grouping

**Base diagram on**:
- Package.json scripts (registerQB, registerQC, syncLGPD, etc.)
- Source code structure analysis
- Database migrations and models

#### 4. Perspectivas Alternativas

Create additional diagrams using collapsible sections:
- **Sequence Diagram**: API interaction flow
- **State Diagram**: Student registration process states  
- **ER Diagram**: Database relationships (if applicable)

Format:
```html
<details>
<summary><strong>Sequence Diagram - Processo de Inscrição</strong> (Clique para expandir)</summary>

```mermaid
sequenceDiagram
    [diagram content]
```

</details>
```

#### 5. Lista de IES/integradores com integração ativa

**Extract from**:
- Source code references
- Configuration files
- Environment variables
- Documentation files

**Format**: Table with columns for Institution, Integration Type, Status, Notes

#### 6. Esquema de payloads esperados por tipo de evento

**Requirements**:
- Extract actual TypeScript interfaces/types from source code
- Document these event types based on package.json scripts:
  - registerQB (Quero Bolsa enrollment)
  - registerQC (Quero Curso enrollment)  
  - syncLGPDQB/syncLGPDQC (LGPD compliance)
- Use JSON schema format or TypeScript interface format
- Include validation rules if found in code

#### 7. Padrão de autenticação por tipo de integração

**Analyze**:
- Environment variables for API keys
- Authentication middleware in source code
- Different auth patterns for different endpoints

#### 8. Endpoints de envio

**Extract from**:
- API route definitions in source code
- Environment variable configurations
- External API integrations mentioned in code

#### 9. Regras de negócio por integração ativa

**Identify from**:
- Business logic in service files
- Validation rules
- Error handling patterns
- Database constraints

#### 10. Definição de eventos mínimos por tipo de ação

**Base on**:
- Event definitions in code
- Database event logging
- API response patterns

#### 11. Formato de resposta esperado das APIs externas

**Extract from**:
- API client implementations
- Response handling code
- Error response patterns

#### 12. Status de processamento - follow_ups table

**Analyze**:
- Database migration files
- Model definitions for follow_ups
- Status enum definitions in code

**Document these statuses**:
- ERROR: [definition from code]
- SUCCESS: [definition from code]  
- NEW: [definition from code]
- PROCESSING: [definition from code]

#### 13. References

**Include**:
- GitHub repository URL from .webloc file
- Official Estácio API documentation (if found)
- Quero Educação related documentation
- Technical resources referenced in code comments
- Database/architecture documentation links

## Quality Assurance

### Technical Accuracy
- All code examples must be syntactically correct
- Verify all information against actual source code
- Use exact names from codebase (functions, variables, endpoints)
- Distinguish between implementation details and assumptions

### Documentation Standards  
- Follow markdown best practices
- Ensure all links work correctly
- Validate Mermaid diagrams before inclusion
- Maintain consistent terminology throughout

### Completeness Check
- Every section must contain actual information or state "Not available in codebase"
- Include code snippets where relevant
- Reference specific files and line numbers when possible
- Provide context for technical decisions

## Error Handling

**If information is missing**:
- State clearly: "Esta informação não está disponível no código-fonte atual"
- Suggest where additional information might be found
- Do not fabricate technical details
- Mark assumptions clearly as "Presumido baseado em [evidence]"

**For external dependencies**:
- Use fetch_webpage tool to get official documentation if URLs are available
- Clearly mark information source (code vs. external docs vs. inference)

## Final Validation

Before completing:
1. Validate all Mermaid diagrams
2. Check all internal links work
3. Ensure consistent Portuguese language usage
4. Verify all technical details against source code
5. Preview any generated diagrams

---

**Note**: This prompt generates comprehensive technical documentation based on actual codebase analysis. All technical details must be verifiable against the source code or clearly marked as external research.