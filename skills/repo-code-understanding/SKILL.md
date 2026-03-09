---
name: repo-code-understanding
description: Large-scale code repository understanding with incremental analysis support, task splitting, and structured documentation generation.
---

# Repository Code Understanding

## Description
This skill provides comprehensive code understanding for large-scale code repositories. It supports splitting analysis into manageable subtasks, generates structured documentation organized by module, and allows incremental updates based on new MR/commit changes. It can handle monorepos and large codebases efficiently without requiring full reanalysis on every update.

## Usage Scenario
Trigger this skill when:
- The user asks to "understand this repository" or "generate code documentation"
- Onboarding to a new codebase and need to understand the overall architecture
- Need to maintain up-to-date code documentation as the project evolves
- Analyze large codebases that cannot be processed in a single pass
- Incrementally update documentation after new MRs/commits are merged

## Instructions

### Full Repository Analysis Flow
1. **Initial Assessment**
   * Scan the repository structure to understand project type, size, and technology stack
   * Identify main modules, entry points, and architectural layers
   * Estimate analysis complexity and split into appropriate subtasks
   * Create an analysis plan outlining which modules to analyze in which order

2. **Subtask Processing**
   * Process each module/subtask independently
   * For each module, analyze:
     - Core responsibilities and business purpose
     - Public API and interfaces
     - Dependencies (internal and external)
     - Key data structures and flow
     - Important implementation patterns
   * Store intermediate results for each module

3. **Documentation Generation**
   * Combine subtask results into a unified documentation structure
   * Follow the organization pattern defined in `templates/`
   * Generate high-level architecture overview
   * Create cross-module relationship maps
   * Export all documentation to the specified output directory

### Incremental Analysis Flow
1. **Change Detection**
   * Receive MR/commit diff of new changes
   * Identify which modules/components are affected by the changes
   * Load existing documentation for those modules

2. **Targeted Reanalysis**
   * Only reanalyze the affected modules
   * Update module documentation to reflect changes
   * Adjust cross-module references if needed
   * Update architecture overview if changes are significant

3. **Documentation Update**
   * Merge updated module documentation back into the main documentation set
   * Maintain version history of changes
   * Flag sections that may need human review

## Output Structure
All documentation is organized in the `code-docs/` directory with this structure:
```
code-docs/
├── README.md                 # High-level overview and navigation
├── architecture.md           # System architecture, layers, and component relationships
├── modules/
│   ├── module-1.md           # Detailed documentation for module 1
│   ├── module-2.md           # Detailed documentation for module 2
│   └── ...
├── api/
│   ├── public-api.md         # Public API documentation
│   └── internal-api.md       # Internal API documentation
└── changelog.md              # Incremental update history
```

## Resources
- Refer to `resources/analysis-strategies.md` for large codebase processing techniques
- Refer to `resources/module-classification.md` for module categorization guidelines
- Refer to `resources/incremental-update-rules.md` for incremental analysis logic
