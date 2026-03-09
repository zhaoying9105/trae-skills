# Repository Code Understanding (OpenClaw Skill)

Large-scale code repository understanding with incremental analysis support, task splitting, and structured documentation generation.

## Features
- ✅ Full repository analysis with task splitting for large codebases
- ✅ Incremental updates based on MR/commit diffs
- ✅ Structured documentation generation
- ✅ Automatic architecture diagram generation
- ✅ Module-level detailed documentation

## Requirements
- Git CLI installed
- Node.js 18+ (for diagram generation)

## Usage
1. Install the skill: `clawhub install repo-code-understanding`
2. Full analysis:
   ```
   "Analyze the current repository and generate documentation"
   "Generate architecture docs for this project"
   ```
3. Incremental update:
   ```
   "Update documentation based on latest commit a1b2c3d"
   "Incrementally analyze the new MR changes"
   ```

## Output Structure
```
code-docs/
├── README.md            # Navigation and overview
├── architecture.md      # System architecture and component relationships
├── modules/             # Individual module documentation
├── api/                 # API documentation
└── changelog.md         # Update history
```

## License
MIT
