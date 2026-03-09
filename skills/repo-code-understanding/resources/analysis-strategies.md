# Large Codebase Analysis Strategies

Use these strategies when processing large repositories:

## 1. Prioritization Strategy
Analyze components in this order for maximum efficiency:
1. **Entry points** (main application files, server startup, index.js/App.tsx)
2. **Core modules** (shared utilities, common components, business domain core)
3. **Feature modules** (individual business features, organized by domain)
4. **Infrastructure modules** (database, API layer, third-party integrations)
5. **Configuration and tooling** (build configs, deployment scripts, CI/CD)

## 2. Splitting Large Modules
If a module exceeds ~50 files or 10,000 lines:
- Split by subdomain or functionality
- Analyze subcomponents independently
- Create a parent summary page that links to subcomponent documentation
- Look for natural boundaries in directory structure

## 3. Dependency-First Analysis
For each module:
1. First identify and understand its dependencies
2. Map dependencies to already analyzed modules if possible
3. Flag circular dependencies for special attention
4. Note external dependencies that require separate research

## 4. Smart Batching
- Group related modules together for analysis
- Process batches of similar complexity
- Allow time for context switching between unrelated domains
- Generate interim summaries after each batch for early feedback

## 5. Abstraction Layers
When analyzing, maintain multiple levels of abstraction:
- **High level:** What does this component do? (1 sentence)
- **Medium level:** What are its main parts and how do they interact? (1 paragraph)
- **Low level:** What are the key implementation details? (detailed section)
- Skip low-level details for well-understood utility libraries
