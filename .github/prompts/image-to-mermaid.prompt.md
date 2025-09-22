---
mode: 'agent'
goal: "Convert a flowchart image to Mermaid diagram syntax"
input:
  source_image: '#file:fluxo-inscricoes.png'
  output_file: '#file:fluxo-inscricoes.md'
  location: '#folder:src'

requirements:
  - Analyze flowchart starting with "Aluno se interessa pela bolsa (CTA - Quero essa bolsa)" and ending with "Fim"
  - Maintain original Portuguese text and terminology
  - Preserve exact process flow and logic
  - Use native Mermaid flowchart syntax

styling:
  colors:
    - Use pastel color palette for nodes and connections
    - Apply consistent color coding for different node types
  enhancements:
    - Add relevant emoticons to nodes
    - Implement subgraphs for logical grouping
    - Ensure clear visual hierarchy

output_format:
  - Valid Mermaid flowchart syntax
  - Properly indented and organized code
  - Include flowchart direction declaration
  - Document any complex logic or conditions

validation:
  - Verify all nodes are connected correctly
  - Confirm text matches source exactly
  - Test diagram renders properly
  - Ensure complete flow from start to end