---
mode: 'agent'
goal: "Convert a flowchart image to Mermaid diagram syntax with maximum accuracy"
input:
  source_image: '#file:fluxo-inscricoes.png'
  output_file: '#file:fluxo-inscricoes.md'
  location: '#folder:src'

text_extraction_requirements:
  - Read EVERY visible text character in the flowchart image, no matter how small
  - Preserve ALL original Portuguese text exactly as written, including:
    - Complete phrases and sentences
    - Specific technical terms (e.g., "Semipresencial", "Prand")
    - System names and integration labels
    - Question marks, punctuation, and special characters
  - Do NOT paraphrase, translate, or simplify any text
  - Do NOT add generic terms where specific ones exist in the image

flow_accuracy_requirements:
  - Map EVERY arrow and connection shown in the original flowchart
  - Preserve the exact decision logic (Sim/Não paths)
  - Maintain all parallel processing paths
  - Include ALL intermediate steps and validation points
  - Capture complex branching and convergence patterns
  - Preserve the exact sequence of operations

node_type_identification:
  - Yellow ovals: Start and end nodes
  - Pink/Magenta diamonds: Decision points with yes/no branches
  - Green rectangles: Process/action nodes
  - Other colored rectangles: Integration/system nodes
  - Identify and preserve the exact shape-to-function mapping

integration_specific_requirements:
  - Capture ALL system-specific terminology (Kroton, Estácio, etc.)
  - Include ALL educational modalities mentioned (Semipresencial, Prand, etc.)
  - Preserve ALL integration workflow steps
  - Maintain parallel processing for different systems
  - Include ALL validation and confirmation steps

mermaid_syntax_requirements:
  - Use `flowchart TD` for top-down direction
  - Define comprehensive CSS classes for styling
  - Use subgraphs to organize related processes
  - Apply consistent node naming conventions
  - Include descriptive comments in the code

styling_specifications:
  colors:
    - startEnd: Warm beige/yellow tones (#FFE4B5, #DEB887)
    - decision: Soft pink/magenta (#FFB6C1, #FF69B4)
    - process: Light green (#E0FFE0, #90EE90)
    - integration: Lavender/purple (#E6E6FA, #DDA0DD)
    - system: Light blue (#F0F8FF, #87CEEB)
    - warning: Light coral (#FFE4E1, #FFA07A)
  enhancements:
    - Add contextually relevant emoticons (🎯, 📝, ✅, 🏫, 📚, etc.)
    - Use subgraphs with descriptive titles
    - Apply consistent visual hierarchy
    - Ensure proper spacing and organization

accuracy_validation_checklist:
  text_accuracy:
    - [ ] Every visible text element has been captured exactly
    - [ ] No generic terms used where specific ones exist
    - [ ] All Portuguese text preserved without translation
    - [ ] Technical terms and system names are correct
  
  flow_accuracy:
    - [ ] All decision points have correct branching logic
    - [ ] All parallel processes are maintained
    - [ ] Integration workflows are complete and accurate
    - [ ] Start and end points match the original exactly
  
  visual_accuracy:
    - [ ] Node types match the original shapes and colors
    - [ ] Subgraph organization reflects the original layout
    - [ ] Flow direction matches the original orientation
    - [ ] All connections and arrows are preserved

error_prevention:
  - Do NOT create simplified or summarized versions
  - Do NOT assume text content - read it directly from the image
  - Do NOT use placeholder text or generic descriptions
  - Do NOT omit steps that seem redundant but are shown in the image
  - Do NOT reorganize the flow unless it improves accuracy

output_format:
  - Complete Mermaid flowchart with all required elements
  - Properly formatted with consistent indentation
  - Include comprehensive CSS class definitions
  - Add descriptive comments for complex sections
  - Validate syntax before finalizing