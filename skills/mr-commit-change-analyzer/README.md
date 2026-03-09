# MR/Commit Change Analyzer (OpenClaw Skill)

Analyze MR/Commit level code changes, provide intuitive and rich logical interpretation, including intent, impact scope, and potential issues.

## Features
- ✅ Automatic change type classification
- ✅ Impact scope analysis
- ✅ Risk assessment (high/medium/low)
- ✅ Structured output format
- ✅ Business-friendly explanations

## Requirements
- Git CLI installed

## Usage
1. Install the skill: `clawhub install mr-commit-change-analyzer`
2. Run in a git repository:
   ```
   "Analyze the latest commit using mr-commit-change-analyzer"
   "Explain the changes in MR !123"
   ```

## Output Structure
- Basic information (ID, author, date, statistics)
- Core intent of the changes
- Key changes grouped by module
- Impact analysis (compatibility, performance)
- Risk assessment
- Recommendations
- Critical code snippets with explanations

## License
MIT
